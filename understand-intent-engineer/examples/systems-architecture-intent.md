# Example: Systems Architecture & Concurrency Intent

This case study shows how an AI model applies the **Understand Intent Engineer** skill to diagnose subtle concurrency bugs, backpressure hazards, and race conditions in a backend service.

---

## 1. The Context (Existing Go Backend Service)

A backend engineer has a high-throughput worker pool service in Go:

```go
package workerpool

import (
	"context"
	"fmt"
	"sync"
)

type Job struct {
	ID    string
	Data  []byte
}

type Pool struct {
	jobs    chan Job
	wg      sync.WaitGroup
	running bool
	mu      sync.Mutex
}

func NewPool(workerCount int) *Pool {
	p := &Pool{
		jobs:    make(chan Job), // Unbuffered channel
		running: true,
	}
	for i := 0; i < workerCount; i++ {
		go p.worker(i)
	}
	return p
}

func (p *Pool) Submit(j Job) error {
	p.mu.Lock()
	defer p.mu.Unlock()
	if !p.running {
		return fmt.Errorf("pool is closed")
	}
	p.jobs <- j // Blocks inside the lock!
	return nil
}

func (p *Pool) Stop() {
	p.mu.Lock()
	p.running = false
	p.mu.Unlock()
	close(p.jobs)
}

func (p *Pool) worker(id int) {
	for j := range p.jobs {
		// process job
		_ = j
	}
}
```

---

## 2. The User Prompt
> *"Under heavy load, Submit() hangs indefinitely and our HTTP handlers time out. What's wrong?"*

---

## 3. ❌ Shallow AI Response (Failed Intent)
The untrained AI sees that `Submit` is blocking and just increases channel size:
```go
// AI changes channel to buffered
jobs: make(chan Job, 1000000)
```
**Why this fails**:
- A large buffer masks the bug until the buffer fills, then hangs again.
- Holding `p.mu.Lock()` while sending on a channel (`p.jobs <- j`) means **no other goroutine can call `Submit()` OR `Stop()`** while one caller is blocked waiting for a worker to become available!
- If workers slow down, `Stop()` cannot acquire `p.mu.Lock()`, making the pool un-stoppable.
- Does not handle context cancellation or graceful worker termination.

---

## 4.  AI with "Understand Intent Engineer" Skill

### Step 1: Systems Archaeology
1. **Critical Anti-Pattern Discovered**:
   `p.jobs <- j` is executed *inside* `p.mu.Lock()`.
   If all workers are busy, the unbuffered channel write blocks. Because the caller holds `p.mu.Lock()`, every other HTTP handler calling `Submit()` queues up on the mutex lock, freezing the entire server. Furthermore, calling `Stop()` is blocked forever on `p.mu.Lock()`, resulting in a **system-wide deadlock**.
2. **Missing Lifecycle Primitives**:
   - No `context.Context` to allow callers to abort if their HTTP request is cancelled.
   - Workers are launched without tracking in `sync.WaitGroup`, so in-flight jobs are abandoned abruptly on shutdown.

---

### Step 2: Algorithmic & System Intent Triangulation
- **Surface Prompt**: *"Under heavy load, Submit() hangs indefinitely..."*
- **True Intent**: The engineer needs a production-grade, thread-safe worker pool that:
  1. Never holds synchronization locks across blocking I/O or channel operations.
  2. Implements proper backpressure and timeout mechanisms using `context.Context`.
  3. Provides graceful shutdown waiting for workers to drain in-flight jobs.

---

### Step 3: Production Engineering Implementation

```go
package workerpool

import (
	"context"
	"errors"
	"sync"
	"sync/atomic"
)

var (
	ErrPoolClosed   = errors.New("workerpool: pool is closed")
	ErrPoolTimeout  = errors.New("workerpool: submission timed out or cancelled")
)

type Job struct {
	ID   string
	Data []byte
}

type Pool struct {
	jobs    chan Job
	wg      sync.WaitGroup
	closed  atomic.Bool // Lock-free atomic check
	stopCtx context.Context
	cancel  context.CancelFunc
}

func NewPool(workerCount int, queueCapacity int) *Pool {
	ctx, cancel := context.WithCancel(context.Background())
	p := &Pool{
		jobs:    make(chan Job, queueCapacity), // Bounded buffer for backpressure
		stopCtx: ctx,
		cancel:  cancel,
	}

	p.wg.Add(workerCount)
	for i := 0; i < workerCount; i++ {
		go p.worker(i)
	}
	return p
}

// Submit enqueues a job respecting context cancellation without holding locks across channel sends.
func (p *Pool) Submit(ctx context.Context, j Job) error {
	if p.closed.Load() {
		return ErrPoolClosed
	}

	select {
	case p.jobs <- j:
		return nil
	case <-ctx.Done():
		return ctx.Err()
	case <-p.stopCtx.Done():
		return ErrPoolClosed
	}
}

// Stop initiates graceful shutdown: stops new submissions, closes queue, and waits for workers.
func (p *Pool) Stop() {
	if !p.closed.CompareAndSwap(false, true) {
		return // Idempotent shutdown
	}

	// Close context to unblock any pending Submit() calls
	p.cancel()

	// Closing the channel signals workers to drain remaining jobs and exit
	close(p.jobs)

	// Await full worker drain
	p.wg.Wait()
}

func (p *Pool) worker(id int) {
	defer p.wg.Done()

	for j := range p.jobs {
		// Process job safely
		p.processJob(j)
	}
}

func (p *Pool) processJob(j Job) {
	// Worker execution logic
}
```

---

### Step 4: Aligned Technical Communication
The AI explains:
> *"The deadlock occurred because `Submit()` held `p.mu.Lock()` while executing a blocking channel send (`p.jobs <- j`). When workers were saturated, callers held the mutex indefinitely, blocking all other requests and preventing `Stop()` from ever executing. I replaced the mutex with an `atomic.Bool` and a bounded select block with `context.Context` support, eliminating the deadlock and providing clean backpressure with graceful shutdown draining."*
