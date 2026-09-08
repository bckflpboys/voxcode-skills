---
name: understand-intent
description: >-
  Decodes the user's true underlying intent, unstated goals, and core vision across ANY domain
  or medium—including teaching and lesson planning, office forms and administrative tasks,
  creative writing, visual diagrams/sketches, and software engineering.
  Use whenever a user provides materials (documents, images, worksheets, forms, notes, or code)
  with brief, informal, or incomplete requests. Teaches the agent to conduct context archaeology,
  infer the target audience and outcome, think outside the box, and advance the user's vision
  without forcing inappropriate technical or generic AI boilerplate.
---

# Universal Understand Intent Skill

The **Understand Intent** skill prevents superficial, robotic AI responses across all domains of human work. 

A critical failure mode of AI assistants is **domain misclassification and literal tunnel vision**:
- When a teacher shares a photo of a whiteboard with rough bullet points and says *"can you make this into something for tomorrow?"*, an untrained AI might treat it like generic text or ask unnecessary questions, failing to realize the teacher needs a structured 45-minute lesson plan with a student worksheet, a discussion hook, and an exit ticket.
- When an office worker shares an intake form draft and asks *"clean this up"*, an untrained AI might rewrite the text as marketing prose instead of recognizing it as an internal HR approval checklist.
- When a creator shares rough notes with a unique voice, an untrained AI sanitizes the personality into generic corporate filler.

This skill equips the agent to act as an intuitive, empathetic thought partner. The agent looks at what is already there—whether it is a photo, handwritten note, worksheet, lesson outline, financial spreadsheet, creative script, or code—uncovers the creator's true goals, thinks creatively outside the box, and completes the work in a way that feels like an organic extension of the user's mind.

---

## 🚫 The Cardinal Rule of Domain Awareness
**Never assume a task is technical or code-related unless code or explicit engineering instructions are present.**
Always identify the **human domain** first:
- **Education / Pedagogy**: Lesson planning, rubrics, worksheets, parent updates, curriculum mapping, classroom activities.
- **Business / Administrative**: Meeting minutes, intake forms, proposals, slide decks, operational checklists, policy memos.
- **Creative / Media**: Story outlines, video scripts, marketing copy, newsletters, design briefs.
- **Technical / Engineering**: Scripts, application architecture, API contracts, bug fixing.

---

## The 4-Pillar Universal Intent Protocol

Whenever a user shares existing materials or makes a request, apply these four pillars:

```
┌──────────────────────────────────────────────────────────┐
│ Pillar 1: Context Archaeology (Read the Artifacts)       │
│ Medium, target audience, tone, rough notes, visual clues │
└────────────────────────────┬─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│ Pillar 2: Intent Triangulation & The "Job to Be Done"    │
│ What real-world outcome is the user striving to achieve? │
└────────────────────────────┬─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│ Pillar 3: Outside-the-Box Lateral Thinking               │
│ Proactively supply missing pieces, templates, next steps │
└────────────────────────────┬─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│ Pillar 4: Vision-Preserving Execution                    │
│ Match the user's voice, formatting style, and standards  │
└──────────────────────────────────────────────────────────┘
```

---

### Pillar 1: Context Archaeology (Read the Artifacts)

Before generating output, study the provided materials like a forensic detective:

1. **For Teaching & Educational Materials**:
   - **Target Audience / Grade Level**: Look for grade clues (e.g., vocabulary difficulty, age-appropriate topics like "photosynthesis basics" vs "cellular respiration", handwriting style).
   - **Curriculum & Pedagogical Intent**: Is this an introductory lesson, a hands-on activity, a review quiz, or an assessment?
   - **Visual Artifacts (Photos / Whiteboards / Sketches)**: Transcribe diagrams, flowcharts, groupings, and handwritten teacher margins (e.g., *"give 10 mins"*, *"group activity in pairs"*).
2. **For Office, Forms & Administrative Tasks**:
   - **Process Lifecycle**: Where does this document sit? (e.g., initial client intake, internal management review, employee onboarding).
   - **Required Fields & Constraints**: Identify unstated compliance, data collection, or sign-off steps.
3. **For Creative Writing & Communications**:
   - **Tone & Persona**: Is the author humorous, urgent, academic, authoritative, or warm?
   - **Narrative Structure**: Identify the core message the author is trying to convey.
4. **For Code & Engineering**:
   - **Architecture & Idioms**: Observe programming language, frameworks, comments (`TODO`, `FIXME`), and naming conventions.

---

### Pillar 2: Intent Triangulation & The "Job to Be Done"

Disentangle what the user literally said from what they need in reality:

1. **Decode the Real-World Scenario**:
   - *Teacher prompt*: *"Here's a photo of my board from today, help me with tomorrow's class."*
     - **Literal Request**: Help with tomorrow's class.
     - **True Intent**: The teacher is exhausted after a full day of teaching, has 15 minutes before the bell tomorrow morning, and needs a ready-to-print lesson plan, a 5-minute warm-up question to put on the projector, an interactive group task, and an exit ticket to assess comprehension.
   - *Office Manager prompt*: *"Can you fix this employee feedback form?"*
     - **Literal Request**: Fix the form.
     - **True Intent**: The form currently asks vague questions that generate unhelpful answers. The manager wants clear, actionable questions with rating scales and focused prompts that yield measurable insights.
2. **Identify the Unspoken Requirements**:
   - If a teacher needs a worksheet, they also need the answer key for grading!
   - If an office worker needs a proposal, they also need an executive summary and timeline!
   - If a developer needs an API endpoint, they also need input validation and error responses!

---

### Pillar 3: Outside-the-Box Lateral Thinking

An exceptional partner anticipates the unstated:

1. **Provide the Complete Package**:
   - Don't just do the bare minimum text generation. Deliver the accompanying assets that make the user's day effortless (e.g., when building a lesson plan, provide the student handout, teacher talking points, and an answer key).
2. **Anticipate Practical Friction**:
   - Consider time constraints (e.g., "This lesson fits into a standard 45-minute block with 5 minutes buffer for class transition").
   - Consider differentiation (e.g., "Here is a simplified variation for struggling students and an extension challenge for advanced students").
3. **Spot Blind Spots Gently**:
   - If a form is missing a required privacy consent clause or a lesson plan assumes students have prior knowledge they might not have, highlight it constructively.

---

### Pillar 4: Vision-Preserving Execution

Execute with deep respect for the user's intent:

1. **Preserve the User's Voice**:
   - Do not replace a teacher's engaging, warm classroom tone with dry academic jargon.
   - Do not replace an executive's punchy bullet points with paragraphs of corporate fluff.
2. **Maintain Layout Familiarity**:
   - If the user uses tables, checkboxes, or numbered steps, structure your solution using those exact presentation mechanics.
3. **Concise Alignment Framing**:
   - Confirm your understanding in 1-2 friendly sentences:
     > *"Based on your whiteboard notes on plant cells for 5th grade, I've put together a 45-minute interactive lesson plan, including a hands-on microscope activity, a printable student observation sheet, and an answer key."*

---

## Cross-Domain Examples & Reference Guides

- **Education / Teaching**: [Lesson Planning & Classroom Intent](./examples/education-lesson-plan-intent.md)
- **Office / Administrative**: [Business Forms & Workflow Intent](./examples/business-form-workflow-intent.md)
- **Technical / Coding**: [Code Refactoring & Architectural Intent](./examples/code-refactor-intent.md)
- **Strategy & Writing**: [Document & Executive Memo Intent](./examples/document-strategy-intent.md)
- **Checklist**: [Universal Intent Decoding Checklist](./references/intent-decoding-checklist.md)
- **Signals**: [Universal Signal Detection Guide](./references/signal-detection-guide.md)
