# Flow Simulation Skill 🔄

> **Universal AI Agent & Model Skill for End-to-End Route Tracing, Blast Radius Analysis, and Regression Prevention.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Format: Universal](https://img.shields.io/badge/Format-Universal%20(Markdown%20%7C%20JSON%20%7C%20YAML)-green.svg)](#universal-compatibility--importing)
[![Skill: Agentic Standard](https://img.shields.io/badge/Skill-Agentic%20Coding%20Standard-purple.svg)](#the-core-skill-protocol)

---

## What is the Flow Simulation Skill?

A notorious failure mode of AI coding models is **"shallow patching"**:
- When a user asks: *"Fix the bug on the signup screen where the submit button hangs,"* a standard AI model often inspects only the frontend component, removes a loading state check or comments out a validation, and marks the task complete.
- In reality, the button hung because an upstream API route returned an unexpected error format, which failed a token parser, which broke session creation, which caused an unhandled promise rejection. The shallow fix leaves the user's data corrupted and breaks adjacent flows (such as login or OAuth).

The **Flow Simulation** skill trains the AI model to behave like a senior systems engineer. It instructs the AI to:
1. **Trace the entire route** from user input through middleware, business logic, databases, third-party APIs, and final client state.
2. **Map the blast radius** to understand every upstream caller and downstream consumer that touches that flow.
3. **Simulate the execution paths mentally** (happy path, failure path, edge cases) before modifying code.
4. **Implement surgical, contract-preserving fixes**.
5. **Re-simulate the entire flow end-to-end** and verify that no connected routes or adjacent features have regressed.

---

## Universal Compatibility & Importing

This skill is designed to work in any environment. Choose the integration method that fits your app or tool:

### 1. Antigravity & Agentic IDEs (Native Skill)
Place this directory in your project's agent skills folder:
```bash
# Workspace level
mkdir -p .agents/skills/
cp -r flow-simulation .agents/skills/

# Or user global level
cp -r flow-simulation ~/.gemini/config/skills/
```
Antigravity automatically discovers `flow-simulation/SKILL.md` and activates it when working on flows, routes, or bug fixes.

---

### 2. Cursor, Windsurf, & VS Code AI Assistants
Add the rule to your `.cursorrules` or `.windsurfrules`:
```markdown
# Include Flow Simulation Rule
Read and adhere to: ./skills/flow-simulation/README.md
Always simulate full execution flows, trace upstream/downstream dependencies, and verify blast radius before and after fixing bugs.
```

---

### 3. Claude Projects & ChatGPT (Custom GPTs / System Prompts)
You can directly import or copy this `README.md` (or [prompt.md](./prompt.md)) into:
- **Claude Projects Knowledge**: Upload `flow-simulation/README.md` as project knowledge.
- **ChatGPT Custom GPT Instructions**: Copy the contents of [prompt.md](./prompt.md) into the GPT's Instructions box.
- **System Prompt**: Append the core protocol section below into your system prompt.

---

### 4. LangChain, LlamaIndex, CrewAI, AutoGen, & Custom Apps
Read the JSON manifest [skill.json](./skill.json) or ingest this markdown file:
```python
# Example: Ingesting into a Python agent
from pathlib import Path

skill_path = Path("skills/flow-simulation/README.md")
flow_simulation_instructions = skill_path.read_text(encoding="utf-8")

agent_system_prompt = f"""
You are an expert software engineering agent.
Always adhere to the following workflow instructions:

{flow_simulation_instructions}
"""
```

---

## The Core Skill Protocol (For AI Models & Agents)

When this skill is active, the AI model must strictly follow these **5 sequential phases**:

```
┌──────────────────────────────────────────────────────────┐
│ Phase 1: Flow Identification & Route Discovery           │
│ Map trigger entry point -> handlers -> services -> store │
└────────────────────────────┬─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│ Phase 2: Interaction & Blast Radius Mapping              │
│ Upstream callers, downstream consumers, data contracts   │
└────────────────────────────┬─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│ Phase 3: Pre-Fix Flow Simulation                         │
│ Step-through happy path, failure condition, edge cases   │
└────────────────────────────┬─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│ Phase 4: Contract-Preserving Implementation              │
│ Surgical root-cause fix; atomic rollback on failures     │
└────────────────────────────┬─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│ Phase 5: Post-Fix Flow Simulation & Verification         │
│ Re-simulate full path, verify adjacent flows, run tests  │
└──────────────────────────────────────────────────────────┘
```

### Phase 1: Flow Identification & Route Discovery
Before editing any file, map the entire journey:
- **Entry Point**: UI event handler, HTTP route handler, webhook receiver, or queue consumer.
- **Ingestion Pipeline**: Middlewares (CORS, auth tokens, rate limits, request schema validation).
- **Domain Services**: Business logic, transactional steps, data models, entity relationships.
- **External Dependencies**: Database queries, Redis cache, third-party APIs (Stripe, Twilio, SendGrid), cloud services.
- **Settlement & Response**: Response schema, HTTP status, session cookies, client state updates, browser navigation/redirects, analytics events.

### Phase 2: Interaction & Blast Radius Mapping
Analyze everything linked to the target code:
- **Upstream Callers**: Who calls this route, method, or component? What arguments and types do they expect?
- **Downstream Consumers**: Who depends on the output, return object, emitted event, or modified database row?
- **Shared Utilities**: If editing a shared validator, helper function, or base class, what other features share it?
- **State Integrity**: Could a failure in this step leave orphaned database records or inconsistent session state?

### Phase 3: Pre-Fix Flow Simulation
Conduct a step-by-step mental simulation:
1. **Nominal Path**: Walk step-by-step through execution with valid inputs. Trace the state transitions at each boundary.
2. **Failure Path**: Inject the exact parameters or edge conditions that produce the bug. Trace where the expected contract breaks.
3. **Boundary & Edge Conditions**: Evaluate null inputs, duplicate records, expired tokens, slow network responses, and concurrent requests.

### Phase 4: Contract-Preserving Implementation
Write the code fix with strict preservation of existing contracts:
- **Root-Cause Resolution**: Fix the core issue at the right abstraction layer instead of masking it with downstream band-aids.
- **Contract Stability**: Retain return types, parameter signatures, and error formats so upstream and downstream callers remain unbroken.
- **Atomic Operations**: Ensure failure conditions trigger appropriate cleanups or database rollbacks.

### Phase 5: Post-Fix Flow Simulation & Verification
Never finish without validating the full route:
1. **Target Flow Re-Simulation**: Mentally step through the previously broken flow and verify that every step succeeds with valid state.
2. **Adjacent Flow Re-Simulation**: Check related routes (e.g., if fixing *Signup*, verify *Login*, *Password Reset*, and *Session Refresh*).
3. **Automated & Manual Verification**: Run test suites (`npm test`, `pytest`, `cargo test`, etc.) or construct targeted assertions.
4. **Summary Flow Map**: Provide the user with a concise flow map showing the traced path, the root cause, and how the fix preserves system integrity.

---

## Real-World Example: Signup Flow Simulation

### Scenario
**User Request**: *"When signing up with Google OAuth or email, users get stuck on the verification screen with error 'Invalid session state'."*

### Shallow AI Fix (BAD)
The shallow AI opens the verification page, sees `if (!session.token) showError()`, and changes it to `if (false) showError()` or creates a dummy token. Result: The user enters the app without a valid database session, causing all subsequent API requests to crash with 401 Unauthorized.

### Flow Simulation Fix (GOOD)
The AI activates the **Flow Simulation** skill and executes:

1. **Phase 1 (Route Tracing)**:
   ```
   [User Submits Signup]
          │
          ▼
   POST /api/auth/register (Route Handler)
          │ (Validates body schema with Zod)
          ▼
   UserService.createUser() -> Writes to DB
          │
          ▼
   TokenService.generateVerificationToken()
          │ (Generates JWT verification token)
          ▼
   EmailService.sendVerificationEmail()
          │
          ▼
   SessionService.createPendingSession() -> Sets HttpOnly Cookie 'auth_pending'
          │
          ▼
   Client redirects to /auth/verify?email=...
          │
          ▼
   GET /api/auth/session/status (Verification Screen Checks Status)
   ```
2. **Phase 2 (Interaction & Blast Radius)**:
   - Discovers that `SessionService.createPendingSession()` sets a cookie with `SameSite=Strict`.
   - When OAuth redirects from an external identity provider, cross-site redirect causes the browser to omit `SameSite=Strict` cookies on the initial landing request!
   - Upstream impact: Affects both OAuth signups and magic-link email clicks.
   - Downstream impact: Verification page route guard reads missing cookie and throws `'Invalid session state'`.
3. **Phase 3 (Pre-Fix Simulation)**:
   - OAuth redirect: External URL -> `/api/auth/callback` -> Cookie missing -> Session fails.
   - Email click: Mail client -> Browser new tab -> Cross-site request -> Cookie missing.
4. **Phase 4 (Contract-Preserving Implementation)**:
   - Updates cookie configuration to `SameSite=Lax` with `Secure=true` for intermediate auth handshakes.
   - Adds fallback session lookup using an encrypted short-lived state query parameter if cookie is omitted during cross-site transitions.
5. **Phase 5 (Post-Fix Simulation & Verification)**:
   - Re-simulates OAuth redirect: external callback correctly attaches `SameSite=Lax` cookie.
   - Re-simulates standard email/password signup: untouched and functioning identically.
   - Re-simulates login flow: verified to ensure no regression on existing user logins.
   - Runs `npm test auth` and confirms all suites pass.

---

## Skill Directory Structure

```text
flow-simulation/
├── SKILL.md                              # Antigravity/Agentic standard instruction file
├── README.md                             # Universal documentation & self-contained prompt
├── prompt.md                             # Raw system prompt for direct copy-pasting
├── skill.json                            # Universal metadata manifest (JSON Schema)
├── references/
│   ├── flow-tracing-checklist.md         # Comprehensive end-to-end tracing checklist
│   └── simulation-techniques.md          # Guide to simulating state machines, async, & DBs
├── examples/
│   ├── signup-flow-walkthrough.md        # Detailed auth & signup flow walk-through
│   └── payment-checkout-flow.md          # Multi-step checkout & webhook flow walk-through
└── resources/
    └── flow-diagram-templates.md         # ASCII & Mermaid templates for reporting flows
```

---

## License

This skill is open source and available under the [MIT License](../../LICENSE).
Feel free to use it in your personal projects, internal agent setups, or commercial applications.
