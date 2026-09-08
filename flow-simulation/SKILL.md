---
name: flow-simulation
description: >-
  Enforces systematic end-to-end execution flow tracing and simulation for any code modification,
  bug fix, or feature addition. Use whenever modifying or debugging a workflow, authentication/signup
  flow, API endpoint, checkout funnel, state transition, or connected multi-step operation. Ensures the
  AI traces upstream triggers, downstream consumers, data contracts, and runs pre- and post-fix simulations
  to prevent regressions and shallow fixes.
---

# Flow Simulation Skill

The **Flow Simulation** skill prohibits isolated, "band-aid" patches. When an issue occurs within any user journey, API route, background worker, or state machine (e.g., *"fix the validation error on the signup form"* or *"fix the payment status webhook"*), the agent must **never** edit isolated lines in a single file and immediately claim completion.

Instead, the agent must treat the code as an interconnected pipeline. The agent will trace the full route from initial entry to final settlement, map all upstream and downstream dependencies, mentally simulate the dynamic execution paths, implement a contract-safe solution, and simulate the complete flow again to ensure zero regressions.

---

## The 5-Phase Flow Simulation Protocol

Whenever a task touches any interconnected logic or workflow, follow this protocol sequentially:

```
[Phase 1: Flow Identification & Route Discovery]
                     │
                     ▼
[Phase 2: Interaction & Blast Radius Mapping]
                     │
                     ▼
[Phase 3: Pre-Fix Flow Simulation]
                     │
                     ▼
[Phase 4: Contract-Preserving Implementation]
                     │
                     ▼
[Phase 5: Post-Fix Flow Simulation & Verification]
```

---

### Phase 1: Flow Identification & Route Discovery

Before modifying any file, map the entire journey from entry point to terminal response:

1. **Identify the Trigger Entry Point**:
   - Client/UI event (e.g., form submit button, URL router, hook, form action).
   - Network API route (e.g., HTTP POST `/api/v1/auth/signup`, GraphQL mutation, gRPC method).
   - Event trigger (e.g., message queue topic, webhook listener, cron schedule).
2. **Trace the Ingestion & Pipeline Route**:
   - Route handlers & controllers.
   - Middlewares (auth guards, rate limiters, validation schemas, session parsers).
   - Business services and domain logic.
   - External dependencies (databases, cache layers, third-party APIs, email providers, payment gateways).
3. **Trace the Settlement & Response Route**:
   - Return payloads, status codes, and headers.
   - Client state updates, cookies, session storage, and route redirects.
   - Background side effects (async dispatch, analytics events, notification queues).

---

### Phase 2: Interaction & Blast Radius Mapping

Examine every component that interacts with the target code:

1. **Upstream Invariants**:
   - Who calls this function or route?
   - What data shapes, types, headers, or parameters does upstream code guarantee or assume?
2. **Downstream Dependencies**:
   - What other services or consumers rely on the output of this function?
   - If the return format or error shape changes, what downstream clients or jobs fail?
3. **State & Storage Mutability**:
   - What database tables, cache keys, or local state tokens are read, written, or locked during this flow?
   - Can this flow fail midway leaving orphaned or corrupted state (lack of transaction/rollback)?
4. **Shared Utilities & Cross-Cutting Concerns**:
   - If a shared utility or helper is changed, what unrelated flows also invoke it?

Consult the [Flow Tracing Checklist](./references/flow-tracing-checklist.md) for full auditing guidance.

---

### Phase 3: Pre-Fix Flow Simulation

Perform a mental dry-run and step-by-step execution analysis before authoring code:

1. **Simulate the Happy Path**:
   - Step through execution with valid, ideal inputs.
   - Track variable states, schema validations, DB queries, session states, and redirect destinations.
2. **Simulate the Failure Path (Recreate the Bug)**:
   - Inject the specific inputs or conditions that cause the user's reported problem.
   - Pinpoint the exact point in the pipeline where the contract, invariant, or expectation breaks.
3. **Simulate Edge Cases & Failure Modes**:
   - Empty, malformed, or hostile inputs (e.g., null, undefined, unexpected strings).
   - Network timeout, database unique constraint collisions (e.g., duplicate email during signup).
   - Race conditions (e.g., double submit, simultaneous token consumption).

Review [Simulation Techniques](./references/simulation-techniques.md) for modeling complex state machines and async steps.

---

### Phase 4: Contract-Preserving Implementation

Write the code fix with strict preservation of existing contracts:

1. **Surgical Precision**: Fix the root cause at the correct abstraction layer—not through superficial workarounds in downstream consumers.
2. **Maintain Data Contracts**:
   - Do not alter existing return types, API response schemas, or exception types unless explicitly agreed upon.
   - If introducing new parameters or return fields, ensure backward compatibility with all upstream callers.
3. **Atomic Error Handling & Rollbacks**:
   - Ensure exceptions at any step in the flow cleanly rollback partial changes (e.g., database transactions, cleanup of temporary tokens).
   - Return informative, standardized error responses consistent with the surrounding architecture.

---

### Phase 5: Post-Fix Flow Simulation & Verification

Never finish without validating the complete chain:

1. **Simulate the Resolved Flow**:
   - Step through the exact scenario that previously failed.
   - Confirm that the issue is resolved cleanly at each layer.
2. **Simulate Surrounding Flows (Regression Prevention)**:
   - Walk adjacent flows that touch the same services, helpers, or database models.
   - For an authentication fix: verify login, password reset, session refresh, and token verification still function properly.
3. **Execute Automated Tests & End-to-End Verification**:
   - Run relevant unit, integration, and E2E test suites:
     ```bash
     npm test # or pytest, cargo test, go test
     ```
   - If automated tests do not exist for the flow, author tests or provide the user with clear manual verification instructions.
4. **Communicate Clearly with a Flow Map**:
   - Summarize the traced flow, the pinpointed issue, and the verified resolution using a concise diagram or structured step summary (see [Flow Diagram Templates](./resources/flow-diagram-templates.md)).

---

## Concrete Example: Signup Flow Walkthrough

For an in-depth walkthrough illustrating how this skill is applied to a broken user registration and session initiation flow, read the [Signup Flow Walkthrough](./examples/signup-flow-walkthrough.md).
