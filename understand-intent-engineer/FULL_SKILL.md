---
name: understand-intent-engineer
description: >-
  All-in-one specialized skill for decoding technical requirements, mathematical invariants,
  algorithmic complexity ($O(N)$), systems architecture, and scientific computing from codebases,
  formulas, and technical drafts. Contains the complete 4-stage engineering protocol, technical audit
  checklists, signal detection guides, case studies, and engineering alignment templates.
---

# Understand Intent Engineer Skill (All-in-One Edition) 🛠️⚡

> **Single-File Complete Edition**: Contains the entire Understand Intent Engineer protocol, asymptotic complexity guidelines, concurrency and systems checklists, deep technical signal guides, two full real-world case studies (numerical/BLAS optimization and systems deadlock/backpressure), and high-signal engineering alignment templates in one copy-pasteable document.

---

## Table of Contents
1. [Core Engineering Mindset](#1-core-engineering-mindset)
2. [The 4-Stage Engineering Intent Protocol](#2-the-4-stage-engineering-intent-protocol)
3. [Engineering Intent Audit Checklist](#3-engineering-intent-audit-checklist)
4. [Technical Signals Guide: Reading Deep Engineering Intent](#4-technical-signals-guide-reading-deep-engineering-intent)
5. [Case Study 1: Scientific Computing & BLAS Vectorization](#5-case-study-1-scientific-computing--blas-vectorization)
6. [Case Study 2: Systems Concurrency Deadlock & Backpressure](#6-case-study-2-systems-concurrency-deadlock--backpressure)
7. [High-Signal Technical Alignment Templates](#7-high-signal-technical-alignment-templates)

---

## 1. Core Engineering Mindset

While general AI coding assistants treat requests as simple text manipulation, a senior staff engineer or research scientist reads between the lines to uncover:
- The **mathematical theorem or physical equation** the code attempts to model.
- The **time/space complexity ($O(N)$)** constraints and hardware runtime budget.
- The **concurrency model** (threads, goroutines, async event loops, actor systems, atomic operations).
- The **memory architecture** (stack vs. heap, cache line alignment, garbage collection overhead, pointer lifetimes).
- The **type invariants** and domain guarantees enforced by the compiler or type checker.

When an engineer or scientist writes a terse prompt like *"make this faster"*, *"fix the deadlock"*, or *"optimize this matrix multiplication"*, this skill directs the agent to uncover the underlying engineering intent and deliver a mathematically rigorous, architecturally elegant solution.

---

## 2. The 4-Stage Engineering Intent Protocol

```
┌──────────────────────────────────────────────────────────┐
│ Stage 1: Technical Archaeology & Invariant Extraction    │
│ Types, memory models, compiler flags, formulas, runtimes │
└────────────────────────────┬─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│ Stage 2: Algorithmic & System Intent Triangulation       │
│ Unstated performance criteria, edge cases, theorems      │
└────────────────────────────┬─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│ Stage 3: Rigorous Lateral Engineering                    │
│ Concurrency, numerical stability, cache, vectorization   │
└────────────────────────────┬─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│ Stage 4: Idiomatic & Invariant-Preserving Implementation │
│ Purity, zero-cost abstractions, domain type boundaries   │
└──────────────────────────────────────────────────────────┘
```

### Stage 1: Technical Archaeology & Invariant Extraction
- **Type Boundaries**: Examine interfaces, generic constraints, ADTs, and lifetimes. What safety guarantees does the type system enforce?
- **Runtime Environment**: Identify execution model (single-threaded async event loop, threadpool, distributed actor cluster, GPU compute kernel).
- **Mathematical Formulations**: Transcribe formulas, linear algebra operations, state transitions, coordinate frames, and dimensional units.
- **Hardware Profile**: Consider memory budgets, L1/L2 cache locality, I/O bottlenecks, and network serialization costs.

### Stage 2: Algorithmic & System Intent Triangulation
- **Complexity Assessment**: Determine current vs. optimal Big-$O$ time and space complexity ($O(1)$, $O(\log N)$, $O(N)$, $O(N \log N)$).
- **Unstated Requirements**:
  - A caching request implies cache eviction strategy (LRU/LFU), TTL, and stampede prevention.
  - A queue request implies backpressure, worker pool sizing, and poison-pill / dead-letter handling.
  - An optimization request implies vectorization, memory recycling, or algorithm re-design.

### Stage 3: Rigorous Lateral Engineering
- **Numerical Stability**: Guard against catastrophic cancellation, precision loss in float math ($f32$ vs $f64$), and zero divisions. Use log-space math for probability chains.
- **Concurrency & Races**: Audit lock hierarchies, atomics, memory visibility, ABA hazards, and channel deadlocks.
- **Memory & Allocations**: Avoid heap allocations in hot inner loops; use buffer pools, zero-copy parsing, and contiguous memory arrays.

### Stage 4: Idiomatic & Invariant-Preserving Implementation
- **Language Purity**: Write code that native maintainers celebrate (Rust borrow semantics, Go channel multiplexing, Python NumPy vectorization).
- **High-Signal Dialogue**: State the Big-$O$ impact, memory profile, and mathematical safety in 1–2 crisp sentences without patronizing fluff.

---

## 3. Engineering Intent Audit Checklist

### A. Algorithmic Complexity & Data Structures
- [ ] **Time Complexity**: Current vs optimal Big-$O$ ($O(1), O(\log N), O(N), O(N \log N), O(N^2)$).
- [ ] **Space Complexity & Allocations**: Can allocations in hot loops be moved outside or pre-allocated?
- [ ] **Data Structure Selection**: Is an array scan used where a hash set, balanced tree, bitset, or spatial tree is needed?
- [ ] **Traversals**: Can multiple passes over a collection be fused into a single pass?

### B. Concurrency, Threading & Asynchronous Systems
- [ ] **Concurrency Model**: Single-threaded event loop, OS threads, green threads / coroutines, or actors?
- [ ] **Race Conditions**: Check-then-act vulnerabilities or non-atomic state mutations?
- [ ] **Deadlock Potential**: If multiple locks are acquired, is there a strict lock acquisition hierarchy?
- [ ] **Backpressure**: If producers outpace consumers, will memory grow unboundedly? Is there bounded buffer sizing?
- [ ] **Memory Visibility**: Are shared variables accessed via appropriate atomic primitives or memory fences?

### C. Mathematical & Numerical Rigor
- [ ] **Floating-Point Precision**: Is $f32$ causing precision loss where $f64$ is required?
- [ ] **Catastrophic Cancellation**: Does the formula subtract nearly equal numbers ($a - b$ where $a \approx b$)?
- [ ] **Underflow / Overflow**: Are probability chains converted to log-space?
- [ ] **Singularities**: Are denominators guarded against zero ($x / (r + \epsilon)$)?
- [ ] **Symplectic Integration**: For physical simulations, is energy conserved over time (Verlet vs Euler)?

### D. Memory Layout & Hardware Sympathy
- [ ] **Cache Locality**: Are data structures laid out contiguously in memory for L1/L2 prefetching?
- [ ] **Vectorization / SIMD**: Can loops be vectorized via explicit array operations or compiler SIMD?
- [ ] **Garbage Collection Pressure**: Does the code create GC churn through short-lived objects?

---

## 4. Technical Signals Guide: Reading Deep Engineering Intent

### Type-Level Signals
| Code Signal | Underlying Intent | Architectural Implication |
| :--- | :--- | :--- |
| `type Brand<K, T> = K & { __brand: T }` | Branded types (`UserId`, `EUR`, `USD`). | Nominal type safety at compile time. Never cast currencies or IDs without explicit converter. |
| Generic constraints: `<T extends Serializable>` | Distributed messaging / persistence. | Invariant: Payload must support clean serialization without circular references. |
| Rust lifetimes: `'a`, `std::borrow::Cow` | Zero-copy architecture. | Author intentionally avoids heap allocations; do not introduce `.clone()` in hot paths. |
| Discriminated unions / ADTs | Exhaustive state machine transitions. | Keep switch/match statements exhaustive; never swallow unhandled variants in defaults. |

### Concurrency & Systems Signals
| Code Signal | Underlying Intent | Architectural Implication |
| :--- | :--- | :--- |
| Channel buffer size: `make(chan Event, 100)` | Bounded buffer with backpressure. | Do not permit unbounded memory growth. If channel fills, producer must block or throttle. |
| `AtomicU64::fetch_add(1, Relaxed)` | High-throughput lock-free counter. | Author bypassed mutexes for throughput. Uphold minimal memory ordering invariants. |
| `sync.Pool` / buffer recycling | Mitigating GC pauses. | In a critical hot path where memory allocation causes GC pressure. Always recycle objects. |

### Mathematical & Scientific Signals
| Code Signal | Underlying Intent | Architectural Implication |
| :--- | :--- | :--- |
| `np.log(probs + 1e-12)` | Log-space calculation with epsilon smoothing. | Probabilities multiply towards underflow ($10^{-50} \to 0$). Always compute in log-space. |
| Tensor dimensions: `(B, T, D)` or `(N, C, H, W)` | Deep learning batching dimensions. | Perform tensor operations using vectorized broadcasting along axis dimensions. |
| `// Symplectic integrator` or `Verlet` | Hamiltonian / energy-conserving physics. | Forward Euler accumulates artificial energy causing explosion. Use Verlet or Leapfrog. |

---

## 5. Case Study 1: Scientific Computing & BLAS Vectorization

### Existing Code
```python
# Compute pairwise Gaussian RBF kernel matrix: K[i, j] = exp(-gamma * ||x_i - x_j||^2)
# Very slow when dataset has 10,000 points.
def rbf_kernel(X, gamma=0.5):
    N = len(X)
    K = []
    for i in range(N):
        row = []
        for j in range(N):
            diff = sum((X[i][d] - X[j][d]) ** 2 for d in range(len(X[i])))
            row.append(math.exp(-gamma * diff))
        K.append(row)
    return K
```
**User Prompt**: *"Can you make this function production ready and fast?"*

### Staff Engineering Solution:
1. **Linear Algebra Reformulation**:
   $$\|x_i - x_j\|^2 = \|x_i\|^2 + \|x_j\|^2 - 2 \langle x_i, x_j \rangle$$
2. **BLAS Level 3 Optimization**: Replaces nested scalar loops with `np.dot(X, X.T)`, fully parallelized across CPU cores.
3. **Memory Chunking**: Computes in `block_size` tiles to avoid RAM exhaustion on $N \ge 10,000$.
4. **Numerical Stability**: Clips distances at $0.0$ (`np.maximum(dist_sq, 0.0)`) to guard against negative floating-point precision artifacts along the diagonal.

Execution time drops from **several minutes** to **under 200ms** ($>1000\times$ speedup).

---

## 6. Case Study 2: Systems Concurrency Deadlock & Backpressure

### Existing Code
```go
func (p *Pool) Submit(j Job) error {
	p.mu.Lock()
	defer p.mu.Unlock()
	if !p.running { return fmt.Errorf("pool is closed") }
	p.jobs <- j // Blocks inside the lock!
	return nil
}
```
**User Prompt**: *"Under heavy load, Submit() hangs indefinitely and our HTTP handlers time out."*

### Systems Engineering Diagnosis & Fix:
- **Root Cause**: Holding `p.mu.Lock()` across a blocking channel send (`p.jobs <- j`) freezes all other callers when workers are saturated. Furthermore, `Stop()` cannot acquire `p.mu.Lock()`, resulting in a complete server deadlock.
- **Fix**:
  1. Replace mutex with `atomic.Bool` for lock-free status checks.
  2. Implement bounded channel with `select` respecting `context.Context` cancellation.
  3. Ensure `Stop()` cancels context, closes channel, and drains `sync.WaitGroup`.

---

## 7. High-Signal Technical Alignment Templates

### Algorithmic Optimization & Complexity
> *"I identified an algorithmic bottleneck in `findDuplicates` where nested slice scans were scaling as $O(N^2)$. I refactored the routine using a 64-bit Robin Hood hash set, reducing asymptotic lookup time to $O(N)$ amortized while keeping peak heap allocation under 4MB for 100k items."*

### Concurrency & Deadlocks
> *"The hang occurred because `Submit()` held a mutex across a blocking channel send. When workers saturated, all threads piled up on the lock, preventing `Stop()` from acquiring it. I replaced the mutex with an `atomic.Bool` check and a non-blocking `select` with context cancellation, ensuring zero thread contention on the hot path."*

### Numerical Stability
> *"The orbital drift was caused by forward Euler integration failing to conserve Hamiltonian energy over time. I replaced it with Velocity Verlet (symplectic integration) and added a Plummer softening parameter $\epsilon = 10^{-3}$ to eliminate gravitational singularities at close proximity."*
