# Engineering Intent Checklist

Use this checklist whenever evaluating code, mathematical formulations, or technical architectures.

---

## 1. Algorithmic Complexity & Data Structures
- [ ] **Time Complexity**: What is the current Big-$O$ time complexity ($O(1), O(\log N), O(N), O(N \log N), O(N^2)$)? Can it be improved?
- [ ] **Space Complexity & Allocations**: Does the algorithm allocate memory inside hot loops? Can allocations be moved outside, pre-sized, or converted into stack buffers?
- [ ] **Data Structure Selection**: Is an array scan being used where a hash set, balanced tree, bitset, or spatial index (k-d tree, R-tree) is required?
- [ ] **Iteration & Traversals**: Can multiple passes over a collection be fused into a single pass?

---

## 2. Concurrency, Threading & Asynchronous Systems
- [ ] **Concurrency Model**: What is the runtime model (single-threaded event loop, OS threads, green threads / coroutines, actors)?
- [ ] **Race Conditions**: Are there check-then-act vulnerabilities or non-atomic state mutations?
- [ ] **Deadlock Potential**: If multiple locks are acquired, is there a strict, global lock acquisition hierarchy?
- [ ] **Backpressure & Queue Saturation**: If producers outpace consumers, will memory grow unboundedly? Is there bounded buffer sizing or rate limiting?
- [ ] **Memory Visibility & Atomics**: Are shared variables accessed via appropriate atomic primitives (`compare_and_swap`, `fetch_add`) or memory fences?

---

## 3. Mathematical & Numerical Rigor
- [ ] **Floating-Point Precision**: Is $f32$ causing precision loss where $f64$ is required?
- [ ] **Catastrophic Cancellation**: Does the formula subtract nearly equal numbers ($a - b$ where $a \approx b$)?
- [ ] **Underflow / Overflow**: Are large multiplications or small probability chains converted into log-space?
- [ ] **Singularities & Bounds**: Are denominators safeguarded against zero ($x / (r + \epsilon)$)?
- [ ] **Symplectic Integration**: For physical simulations (orbital, molecular, rigid body), is energy conserved over time (e.g. Verlet vs Euler)?

---

## 4. Memory Layout & Hardware Sympathy
- [ ] **Cache Locality**: Are data structures laid out contiguously in memory to exploit L1/L2 cache prefetching (Array of Structs vs Struct of Arrays)?
- [ ] **Vectorization / SIMD**: Can loops be vectorized using compiler auto-vectorization or explicit array operations (NumPy, AVX/NEON)?
- [ ] **Garbage Collection Pressure**: Does the code create high GC churn through short-lived objects?

---

## 5. Type Invariants & API Contracts
- [ ] **Compile-Time Safety**: Can runtime assertions be lifted into the type system (e.g., non-empty lists, tagged unions, branded primitives)?
- [ ] **Backward Compatibility**: Does the modification break public API signatures, serialized binary schemas, or wire protocols?
- [ ] **Error Propagation**: Are errors typed and handled explicitly, avoiding uninformative string errors or silent panics?
