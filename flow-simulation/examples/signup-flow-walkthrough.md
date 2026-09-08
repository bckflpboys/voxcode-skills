# Example Walkthrough: Signup & Authentication Flow

This example demonstrates how an AI agent uses the **Flow Simulation** skill when instructed to fix a real-world issue in a user registration flow.

---

## The User Prompt
> *"Users are reporting that when they complete the signup form, the page spins for 10 seconds and then displays 'Network Timeout', but when they refresh and try to log in, it says their email is already registered."*

---

## ❌ How an Untrained AI Handles This (Shallow Fix)
1. Opens `SignupForm.tsx`.
2. Sees `timeout: 10000` in the `fetch()` call.
3. Changes `timeout: 10000` to `timeout: 30000` or catches the error and displays a generic toast.
4. Marks the issue as "Fixed".
5. **Result**: The underlying backend stall remains; users now wait 30 seconds instead of 10; duplicate accounts accumulate in limbo; users still cannot verify their accounts.

---

##  How the AI Handles This with Flow Simulation

### Phase 1: Flow Identification & Route Discovery
The AI traces the entire signup route:

```
[SignupForm.tsx]
       │ (User submits email, password, name)
       ▼
[POST /api/v1/auth/signup] (Next.js / Express Route Handler)
       │
       ├─► [validateSignupInput] (Zod Schema Validation)
       │
       ├─► [UserService.createPendingUser] (Inserts user with status='PENDING')
       │
       ├─► [TokenService.generateVerificationToken] (Creates 6-digit OTP in Redis)
       │
       ├─► [EmailService.sendVerificationEmail] (Calls third-party SMTP / Resend API)
       │
       └─► [Response 201 Created] -> [Client navigates to /auth/verify-otp]
```

---

### Phase 2: Interaction & Blast Radius Mapping
The AI analyzes upstream and downstream dependencies:
- **Upstream Callers**:
  - `SignupForm.tsx` (web client)
  - `MobileAuthService.ts` (mobile app client)
- **Downstream Services**:
  - `UserRepository` (Postgres database)
  - `RedisCache` (OTP storage)
  - `EmailService` (SMTP transport)
- **Shared Utilities**:
  - `EmailService.send()` is also used by `PasswordResetService` and `BillingReceiptNotifier`.
  - Database table `users` has a unique constraint on `email`.

---

### Phase 3: Pre-Fix Flow Simulation

The AI mentally steps through the failure scenario:
1. `validateSignupInput`: Valid input passes cleanly.
2. `UserService.createPendingUser`: Database row created successfully in Postgres. Email is now taken.
3. `TokenService.generateVerificationToken`: OTP saved to Redis with a 15-minute TTL.
4. `EmailService.sendVerificationEmail`: **CRITICAL BOTTLENECK DISCOVERED**
   - The code does `await emailClient.sendMail(...)` synchronously inside the HTTP handler.
   - The SMTP server has an unhandled connection retry loop that hangs for 10 seconds before throwing an unhandled socket timeout.
5. The HTTP route never returns a response before the client-side 10-second timeout fires.
6. The client cancels the request and shows "Network Timeout".
7. Because the user row was already committed to the database without a transaction rollback, the email remains locked as `'PENDING'`, blocking any subsequent registration attempt.

---

### Phase 4: Contract-Preserving Implementation

The AI applies a surgical, architecture-compliant fix:
1. **Asynchronous Email Offloading**:
   - Instead of awaiting synchronous SMTP network requests inside the critical HTTP request-response cycle, push the email delivery task to the background job queue (e.g., BullMQ / background worker).
   - Return HTTP 201 immediately once the user record and verification token are persisted.
2. **Database Transaction & Rollback Safety**:
   - Wrap the user creation and token generation in an atomic transaction:
   ```typescript
   // Atomic operation
   await db.transaction(async (tx) => {
     const user = await tx.users.insert({ ... });
     await tx.verificationTokens.insert({ ... });
   });
   ```
3. **Handle Pending Duplicate Registrations**:
   - If a user with status `'PENDING'` re-submits the form before verifying, regenerate the token and resend the email rather than throwing a duplicate key constraint violation.

---

### Phase 5: Post-Fix Flow Simulation & Verification

The AI simulates the complete workflow again:
1. **Happy Path Simulation**:
   - User submits form -> DB transaction commits in 15ms -> Background email job enqueued -> HTTP 201 returned immediately -> Client redirects to `/auth/verify-otp` in under 100ms.
2. **SMTP Down Simulation**:
   - Even if SMTP provider experiences downtime, the signup request succeeds; the worker retries the email gracefully with exponential backoff; user is not blocked.
3. **Blast Radius Check**:
   - Verified that `PasswordResetService` and `LoginService` were not negatively impacted.
   - Ran `npm run test:auth` - 14 tests passing.
4. **Agent Reports Back to User**:
   - Provides a clear summary flow diagram showing what happened and how the fix resolved both the latency and the duplicate registration lock.
