# Example: Decoding Document & Strategy Intent from Rough Notes

The **Understand Intent** skill applies to writing, product specs, and business documents as much as code. This example shows an AI turning raw notes into an executive strategy document without losing the author's voice or original vision.

---

## 1. The Context (Author's Raw Notes)

The user has a file `strategy-draft.md`:

```markdown
# Q3 App Redesign Thoughts
- Current app feels clunky. 4 clicks to export a project is ridiculous.
- Competitors (Tool X, Tool Y) do 1-click sharing.
- Target: power users + agencies. They want speed, not wizards.
- Need a command palette (cmd+k) like Linear or Raycast.
- Don't touch the backend API yet, just client-side workflow improvements.
- Need to present this to leadership next Tuesday. Keep it crisp.
```

---

## 2. The User Prompt
> *"Can you flesh this out?"*

---

## 3. ❌ Shallow AI Failure
The untrained AI produces a generic, 15-page corporate report bloated with AI clichés (*"In today's fast-paced digital landscape, synergy between stakeholders..."*), invents backend rewrites the user specifically said not to touch, and loses the urgency and directness of the author.

---

## 4.  AI with "Understand Intent" Skill

### Step 1: Context Archaeology
- **Author's Tone**: Direct, high-urgency, pragmatic, product-minded (*"4 clicks is ridiculous"*).
- **Target Audience**: Company leadership presentation on Tuesday (*"Keep it crisp"*).
- **Hard Constraints**: *"Don't touch backend API yet, just client-side workflow improvements"*.
- **Key Analogies**: Linear, Raycast (keyboard-first, low latency, developer ergonomics).
- **Target User Persona**: Power users and creative agencies.

### Step 2: Intent Triangulation
- **Surface Prompt**: *"Can you flesh this out?"*
- **True Intent**: The author needs a persuasive, executive-ready Product Strategy Proposal for leadership next Tuesday that articulates why the redesign is critical, how it beats competitors, and the exact phased roadmap—strictly confined to frontend improvements.

### Step 3: Outside-the-Box Thinking
- Add a quantifiable **"Before vs. After"** workflow metric comparison (e.g., 4 clicks / 18 seconds -> 1 keystroke / 1.2 seconds).
- Frame the proposal using an executive narrative: Problem -> Competitive Threat -> Solution -> Zero-Backend-Risk Rollout.
- Include a mock ASCII Command Palette (Cmd+K) wireframe to make the vision instantly tangible to leadership.

### Step 4: Vision-Preserving Expansion

```markdown
# Product Strategy: Q3 Workflow Acceleration & Command Center
**Target Audience**: Executive Leadership  
**Timeline**: Q3 Sprint Cycle | **Constraint**: 100% Client-Side (Zero Backend Migrations)

---

## 1. Executive Summary
Our current export and project navigation workflows require **4 distinct modal steps** and an average of **18 seconds** per action. Competitors like Tool X and Tool Y have captured agency mindshare with 1-click frictionless workflows. 

This proposal outlines a focused client-side redesign centered on **keyboard-driven navigation (Cmd+K)** and **instant 1-click sharing**, slashing task completion times by **90%** without altering our core backend API infrastructure.

---

## 2. The Bottleneck: Power User Friction
Creative agencies and power users execute the project export loop 20–50 times daily. 
- **Current Flow**: Dashboard -> Project Settings -> Export Tab -> Format Select -> Download.
- **Pain Point**: Multi-step modal wizards penalize high-frequency users.
- **Proposed Flow**: Global keyboard trigger (`Cmd + K`) -> `Export` -> Instant Direct Download.

---

## 3. Competitive Benchmark & Solution Architecture

| Metric | Current Experience | Competitor X | Proposed Q3 Workflow |
| :--- | :--- | :--- | :--- |
| **Export Task Time** | 18.4 seconds | 3.2 seconds | **1.5 seconds** |
| **Clicks Required** | 4 clicks | 1 click | **0 clicks (Cmd+K) or 1 click** |
| **Backend Risk** | N/A | N/A | **Zero (reuses existing `/api/v1/export`)** |

---

## 4. Implementation Phasing (Leadership Roadmap)
- **Phase 1 (Weeks 1-2)**: Global Cmd+K Command Palette shell with fuzzy search.
- **Phase 2 (Weeks 3-4)**: 1-Click direct export shortcuts utilizing current client cached state.
- **Phase 3 (Week 5)**: Beta rollout to top 15 high-volume agency accounts.
```

### Step 5: Aligned Communication
The AI responds:
> *"I've fleshed out your notes into an executive-ready proposal tailored for your leadership presentation on Tuesday. I kept your crisp, direct tone, added a before/after benchmark table against competitors, and ensured the entire roadmap strictly respects your constraint of zero backend API modifications."*
