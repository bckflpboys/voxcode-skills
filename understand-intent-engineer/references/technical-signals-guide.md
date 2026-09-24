# Technical Signals Guide: Reading Deep Engineering Intent

Experienced engineers and scientists embed implicit architectural and mathematical clues in their codebases. This guide helps AI models decode these high-signal indicators.

---

## 1. Type-Level Signals

| Code Signal | Underlying Intent | Architectural Implication |
| :--- | :--- | :--- |
| `type Brand<K, T> = K & { __brand: T }` | Branded types (e.g. `UserId`, `EUR`, `USD`). | User is enforcing nominal type safety at compile time. Never cast or mix currencies/IDs without an explicit converter. |
| Generic constraints: `<T extends Serializable & Clone>` | Code is designed for distributed messaging or storage. | Invariant: Any new state or payload must support clean serialization without circular references. |
| Rust lifetime annotations: `'a`, `std::borrow::Cow` | Zero-copy architecture. | The author intentionally avoids cloning data into the heap; do not introduce `.clone()` or `.to_owned()` in hot paths. |
| Discriminated unions / ADTs (`type Action = { type: 'A' } \| { type: 'B' }`) | Exhaustive state machine transitions. | Ensure all switch/match statements remain exhaustive; do not swallow unhandled variants in a default block. |

---

## 2. Concurrency & Systems Signals

| Code Signal | Underlying Intent | Architectural Implication |
| :--- | :--- | :--- |
| Channel buffer size: `make(chan Event, 100)` | Bounded buffer with backpressure. | The author does not want unbounded memory growth. If the channel fills, the producer must block, drop, or reject. |
| `AtomicU64::fetch_add(1, Ordering::Relaxed)` | High-throughput lock-free counter. | The author intentionally bypassed mutex locking for throughput. Use minimal memory ordering invariants (`Relaxed`, `Acquire/Release`). |
| `sync.Pool` / buffer recycling | Mitigating garbage collection pauses. | The code is in a critical hot path where memory allocation causes GC pressure. Always put objects back into the pool. |
| Database transaction isolation: `ISOLATION LEVEL SERIALIZABLE` | Absolute consistency over throughput. | System is vulnerable to serialization anomalies or write skew (e.g. financial ledgers). Ensure retry logic handles rollback aborts. |

---

## 3. Mathematical & Scientific Signals

| Code Signal | Underlying Intent | Architectural Implication |
| :--- | :--- | :--- |
| `np.log(probs + 1e-12)` | Log-space calculation with epsilon smoothing. | Probabilities multiply towards underflow ($10^{-50} \to 0$). Always maintain calculations in log-domain: $\sum \log(p_i)$. |
| Matrix dimensions: `(B, T, D)` or `(N, C, H, W)` | Deep learning batching (Batch, Sequence, Channels). | Preserves tensor dimension alignment. Perform tensor operations using vectorized broadcasting along axis dimensions. |
| Comment: `// Symplectic integrator` or `Verlet` | Hamiltonian / energy-conserving physics. | Standard forward Euler accumulates artificial energy and causes orbital or spring explosions. Use Verlet, Leapfrog, or RK4. |
| `x = x & (x - 1)` (Kernighan's bit hack) | Bit-level counting or binary operations. | Low-latency bitboard or bitmask logic. Keep operations bitwise; avoid string conversions or high-level iteration. |
