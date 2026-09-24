---
name: understand-intent-engineer
description: >-
  Decodes unstated technical requirements, mathematical invariants, algorithmic goals,
  and systems architecture from terse developer requests, WIP code, equations, or scientific scripts.
  Hyper-specialized for software engineering, computer science, mathematics, physics,
  and high-performance computing. Instructs the agent to perform deep technical archaeology,
  anticipate race conditions, memory leaks, numerical instability, and algorithmic complexity ($O(N)$),
  and implement mathematically sound, idiomatic engineering solutions.
---

# Understand Intent Engineer Skill

The **Understand Intent Engineer** skill provides deep technical and scientific intuition. While general AI assistants treat coding requests as simple text manipulation, a senior staff engineer or research scientist reads between the lines to uncover:
- The **mathematical theorem or physical equation** the code attempts to model.
- The **time/space complexity ($O(N)$)** constraints and hardware runtime budget.
- The **concurrency model** (threads, goroutines, async event loops, actor systems, atomic operations).
- The **memory architecture** (stack vs. heap, cache line alignment, garbage collection overhead, pointer lifetimes).
- The **type invariants** and domain guarantees enforced by the compiler or type checker.

When an engineer or scientist writes a terse prompt like *"make this faster"*, *"fix the deadlock"*, or *"optimize this matrix multiplication"*, this skill directs the agent to uncover the underlying engineering intent and deliver a mathematically rigorous, architecturally elegant solution.

---

## The 4-Stage Engineering Intent Protocol

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

---

### Stage 1: Technical Archaeology & Invariant Extraction

Before touching code, inspect the underlying technical substrate:

1. **Type Boundaries & Invariants**:
   - Inspect generics, traits, interfaces, algebraic data types (ADTs), and brand types.
   - What invariants is the type system trying to guarantee? (e.g., non-nullability, thread-safety `Send/Sync`, immutability, unit dimensions like `Seconds` vs. `Milliseconds`).
2. **Runtime & Concurrency Environment**:
   - Is this running in a single-threaded event loop (Node.js/V8), a work-stealing threadpool (Go, Tokio), or a multi-process actor system (Erlang/Elixir)?
   - Are there synchronization primitives already in use (`Mutex`, `RwLock`, `AtomicBool`, channels, semaphores)?
3. **Mathematical & Scientific Formulations**:
   - Transcribe mathematical formulas, LaTeX snippets, loss functions, differential equations, or linear algebra operations.
   - Identify units of measurement, coordinate systems, matrix dimensions, and domain normalization schemes.
4. **Hardware & Resource Budgets**:
   - Is this an embedded/IoT system with strict RAM limits?
   - A GPU/CUDA kernel where coalesced memory access matters?
   - A cloud microservice where memory allocations drive AWS Lambda execution costs?

---

### Stage 2: Algorithmic & System Intent Triangulation

Separate the literal request from the unstated technical objectives:

1. **Complexity & Algorithmic Paradigm**:
   - If the code contains nested loops scanning an array ($O(N^2)$), determine whether the intent requires an index, a hash table ($O(1)$ amortized), a binary search ($O(\log N)$), or a spatial index (k-d tree, R-tree).
   - If dynamic programming or memoization was attempted, reconstruct the optimal substructure and state transition equation.
2. **Infer the Unstated "Why"**:
   - *Prompt*: *"Why does this simulation drift over time?"*
     - *Underlying Intent*: The code is using standard Euler integration which accumulates numerical error; the engineer needs a symplectic integrator (e.g., Verlet or Runge-Kutta 4th order) or compensated summation (Kahan summation).
   - *Prompt*: *"Can we make this batch processing queue more reliable?"*
     - *Underlying Intent*: The system lacks backpressure, causing queue memory exhaustion when consumers slow down; the engineer needs reactive streams, leaky-bucket throttling, or dead-letter queues.

---

### Stage 3: Rigorous Lateral Engineering

Anticipate deep failure modes that junior implementations overlook:

1. **Numerical Stability & Precision**:
   - Guard against catastrophic cancellation, floating-point rounding errors ($0.1 + 0.2 \neq 0.3$), division by zero, and integer overflow/underflow.
   - For probabilities, use log-space arithmetic ($\log(P(A) \cdot P(B)) = \log P(A) + \log P(B)$) to prevent underflow.
2. **Concurrency & Race Conditions**:
   - Audit for check-then-act races, deadlocks (inconsistent lock acquisition order), starvation, and ABA problems.
   - Ensure atomic transactions and idempotent replay capabilities.
3. **Memory & Cache Efficiency**:
   - Favor contiguous array layouts (struct-of-arrays or array-of-structs) to maximize L1/L2/L3 cache line hits and CPU prefetching.
   - Eliminate unnecessary heap allocations in hot paths (reuse buffers, object pools, stack allocations).

---

### Stage 4: Idiomatic & Invariant-Preserving Implementation

Write the code adhering strictly to the highest standards of the language and domain:

1. **Zero-Cost Abstractions & Idiomatic Patterns**:
   - Rust: Respect ownership, borrowing, lifetime elision, and iterator combinators without cloning unnecessarily.
   - TypeScript: Leverage distributive conditional types, template literal types, and exact type guards.
   - Python / NumPy: Vectorize array operations; avoid Python-level `for` loops across tensors.
   - Go: Keep goroutines lightweight, handle channels with `select`, and pass context for cancellation.
2. **Preserve Architectural Style**:
   - Do not force an OOP design pattern onto a functional codebase.
   - Do not introduce heavy runtime reflection where static types are preferred.
3. **High-Signal Technical Communication**:
   - State the algorithmic complexity (Big-$O$), the mathematical guarantee, and the trade-offs in 1–2 crisp sentences.

---

## Detailed References & Case Studies

- **[Engineering Intent Checklist](./references/engineering-intent-checklist.md)**: Systematic technical audit checklist.
- **[Technical Signals Guide](./references/technical-signals-guide.md)**: Deep dive into reading code, compiler, and formula signals.
- **[Mathematical & Algorithmic Example](./examples/math-algorithm-intent.md)**: Stabilizing a scientific simulation algorithm.
- **[Systems & Concurrency Example](./examples/systems-architecture-intent.md)**: Diagnosing and solving distributed state and queue backpressure.
- **[Technical Alignment Templates](./resources/technical-alignment-templates.md)**: Concise framing templates for engineering dialogue.
