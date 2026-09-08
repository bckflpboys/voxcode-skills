# Flow Tracing Checklist

Use this checklist whenever evaluating a bug or modification that spans multiple layers, components, or services.

---

## 1. Trigger & Ingestion Checklist
- [ ] **Trigger Source**: Is the entry point user-driven (button, form, navigation), system-driven (cron, webhook), or message-driven (queue, event bus)?
- [ ] **Input Parsing**: Where is the incoming payload parsed or deserialized? Are types coerced or validated?
- [ ] **Middlewares & Interceptors**:
  - [ ] Authentication / Session validation
  - [ ] Authorization / Permissions / Roles
  - [ ] Request body validation (e.g., Zod, Joi, Pydantic)
  - [ ] Rate limiting / Throttling
  - [ ] CORS / CSRF headers

---

## 2. Business Logic & Service Checklist
- [ ] **Domain Entity State**: What business rules govern the transition? Are all preconditions checked?
- [ ] **Concurrency & Re-entrancy**: Can this flow be triggered twice simultaneously (e.g., double-clicking a submit button)? Is there idempotency?
- [ ] **Branching Logic**: Are all condition branches (`if / else`, `switch`, `match`) accounted for, including unhandled default cases?
- [ ] **Error Propagation**: Are errors thrown as typed domain exceptions, or are they swallowed / caught silently?

---

## 3. Persistence & State Checklist
- [ ] **Database Reads/Writes**: Which tables, collections, or records are touched?
- [ ] **Transactions**: Are multi-step writes wrapped in an atomic database transaction?
- [ ] **Rollback Mechanisms**: If step 3 of 4 fails, is step 1 and step 2 cleaned up or rolled back?
- [ ] **Caching Layer**: Does this flow invalidate, update, or read from Redis/Memcached? Could stale cache break subsequent reads?

---

## 4. External Integrations Checklist
- [ ] **Third-Party APIs**: Calls to Stripe, SendGrid, Twilio, OAuth providers, etc.
- [ ] **Timeout & Retry Policy**: How does the system react if the external service times out or returns HTTP 500/503?
- [ ] **Webhooks & Asynchronous Handshakes**: Does this action trigger an asynchronous callback? Will the system be ready to receive it?

---

## 5. Settlement & Client State Checklist
- [ ] **Response Envelope**: Does the return payload match the expected format `{ success: true, data: ... }`?
- [ ] **HTTP Status**: Is the status code accurate (200, 201, 400, 401, 403, 404, 409, 422, 500)?
- [ ] **Session & Cookie Headers**: Are `Set-Cookie` attributes correct (`SameSite`, `Secure`, `HttpOnly`, `Path`, `Domain`)?
- [ ] **Client State Mutations**: How does the frontend store (Redux, Zustand, Pinia, React Query) react?
- [ ] **Client Navigation**: Is the user redirected to the correct route with the required query parameters or route state?

---

## 6. Regression & Blast Radius Checklist
- [ ] **Adjacent Workflows**: What other flows invoke the modified service/function/component?
  - Example: Modifying `generateToken()` affects Signup, Login, Password Reset, and OAuth callback.
- [ ] **Schema Changes**: Did any database column, API response property, or function argument signature change?
- [ ] **Existing Automated Tests**: Did all unit, integration, and E2E tests pass?
