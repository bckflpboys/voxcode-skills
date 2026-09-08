# Simulation Techniques for AI Agents

When an AI agent "simulates" a flow, it executes a rigorous, step-by-step mental evaluation of how variables, control flows, and state machines evolve across time and distributed components.

---

## 1. The Dynamic State Table Technique

Build an internal state transition table to track variables across each step of the pipeline.

### Example: Tracking an Authentication Flow
| Step | Action | Input State | Output / Mutation | Invariant / Expectation |
| :--- | :--- | :--- | :--- | :--- |
| **0. Form Submit** | User clicks submit | `email`, `password` | Client emits `POST /api/register` | Fields non-empty, valid format |
| **1. Middleware** | Rate limiter | IP: `192.168.1.1` | Counter incremented in Redis | Request under limit |
| **2. Validation** | Zod schema parse | Raw JSON body | Sanitized `{ email, password }` | Password meets strength requirements |
| **3. Duplicate Check**| Query DB for email | Sanitized email | DB returns `null` (not found) | Email unique |
| **4. Hash Password** | Argon2/Bcrypt hash | Raw password | `passwordHash` | Non-reversible, salt attached |
| **5. Insert User** | DB Transaction | `email`, `passwordHash` | New user record `id: 42` | Transaction open |
| **6. Create Session** | Session table insert | `userId: 42` | Session token `sess_xyz` | Session active |
| **7. Send Verification**| Queue email job | `userId: 42`, email | Message on queue | Async, does not block response |
| **8. Commit DB** | Commit transaction | Open transaction | Permanent record saved | Transaction closed |
| **9. HTTP Response**| Set Cookie & Return | `sess_xyz` | `Set-Cookie`, `{ ok: true }` | `HttpOnly`, `SameSite=Lax` |
| **10. UI Redirect** | Router push | `{ ok: true }` | Navigate to `/dashboard` | Protected route allows entry |

---

## 2. Failure Path Injection & Point of Breakdown

To isolate the bug, simulate the exact failure condition and observe where the state table diverges from the invariant:

1. **Simulate with Invalid / Problematic Data**:
   - What happens if Step 3 finds an existing user?
   - What happens if Step 7 fails because the email provider rejects the API key?
   - Does Step 8 still commit, leaving a user who can never verify their email?
2. **Identify Silent Failures**:
   - Is an unhandled promise rejection occurring?
   - Does a catch block return `{ success: false }` while the client expects `{ error: "message" }`?

---

## 3. Asynchronous & Race Condition Simulation

Many bugs only appear when actions occur concurrently or out of order. Simulate these timing profiles:

### Double-Click / Rapid Repeat
- User taps "Complete Purchase" twice in 100ms.
- **Simulation**:
  - Request A starts -> locks inventory -> processes payment.
  - Request B starts before Request A finishes -> checks inventory (still looks available) -> processes duplicate payment!
  - **Remedy Identified**: Add database row-level locking or idempotent payment keys (`Idempotency-Key` header).

### Slow Network / Out-of-Order Responses
- In a search typeahead:
  - User types "re" (Request 1 sent, takes 400ms).
  - User types "react" (Request 2 sent, takes 150ms).
  - Request 2 returns first and populates UI with "react" results.
  - Request 1 returns later and overwrites UI with outdated "re" results!
  - **Remedy Identified**: AbortController or sequence ID tracking.

---

## 4. Blast Radius Analysis Checklist

Before writing code:
1. **Direct Callers**: Grep for every file importing or referencing the modified function.
2. **Subclasses / Implementations**: If modifying an interface or base class, check every implementation.
3. **Database Migrations / Schema Changes**: If modifying a column type, check every query, ORM model, and DTO that touches that column.
4. **Third-Party Contracts**: If altering a webhook handler, confirm that the external provider's payload structure is strictly respected.
