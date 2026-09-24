# Technical Alignment Templates

When communicating technical choices, engineers and researchers appreciate high signal, zero fluff, and precise technical vocabulary. Use these framing templates.

---

## 1. Algorithmic Optimization & Complexity Framing

### Pattern
> *"I identified an algorithmic bottleneck in `[function/module]` where [current approach] scaled with [current Big-O complexity]. I refactored the routine using [new algorithm / data structure], reducing asymptotic time complexity to [new Big-O] while maintaining [space budget / invariants]."*

### Example
> *"I identified an algorithmic bottleneck in `findDuplicates` where nested slice scans were scaling as $O(N^2)$. I refactored the routine using a 64-bit Robin Hood hash set, reducing asymptotic lookup time to $O(N)$ amortized while keeping peak heap allocation under 4MB for 100k items."*

---

## 2. Concurrency & Deadlock Diagnosis Framing

### Pattern
> *"The deadlock was caused by [root cause: e.g. lock holding across blocking I/O / inconsistent lock acquisition order]. I restructured the synchronization model using [primitives: atomics / channels / lock-free ring buffer], eliminating the lock contention and adding [graceful shutdown / backpressure]."*

### Example
> *"The hang occurred because `Submit()` held a mutex across a blocking channel send. When workers saturated, all threads piled up on the lock, preventing `Stop()` from acquiring it. I replaced the mutex with an `atomic.Bool` check and a non-blocking `select` with context cancellation, ensuring zero thread contention on the hot path."*

---

## 3. Mathematical & Numerical Rigor Framing

### Pattern
> *"The numerical instability was due to [phenomenon: e.g. catastrophic cancellation / non-symplectic energy drift]. I reformulated the equation using [theorem / identity / stable formulation], guaranteeing [mathematical property: e.g. energy conservation / log-space underflow prevention]."*

### Example
> *"The orbital drift was caused by forward Euler integration failing to conserve Hamiltonian energy over time. I replaced it with Velocity Verlet (symplectic integration) and added a Plummer softening parameter $\epsilon = 10^{-3}$ to eliminate gravitational singularities at close proximity."*

---

## 4. Hardware Sympathy & Low-Latency Framing

### Pattern
> *"To minimize cache misses and GC overhead, I converted [data structure] from [original layout: e.g. pointer-heavy linked tree] to [cache-friendly layout: e.g. contiguous flat array / Arena allocation], maximizing L1/L2 prefetching and achieving [speedup / zero allocations]."*

### Example
> *"To reduce V8 garbage collection churn during audio processing, I moved sample buffering out of per-frame heap allocations into a pre-allocated `Float32Array` ring buffer, eliminating GC pauses entirely during playback."*
