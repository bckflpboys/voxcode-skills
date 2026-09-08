# Universal Signal Detection Guide: Reading Context Clues

Across every profession and medium, creators leave subtle clues in their artifacts. This guide teaches AI models how to detect and interpret these signals across education, office work, creative writing, and engineering.

---

## 1. Education & Teaching Signals

| Signal / Clue | What It Tells You | Recommended Action |
| :--- | :--- | :--- |
| Hand-drawn boxes with arrows between animals/plants | Food web or lifecycle concept. | Build a visual, interactive student diagram with clear directionality. |
| Time notations: *"10 min"*, *"20 min"* | Rigid classroom period (e.g. 45-minute bell schedule). | Respect the exact pacing; provide timed sections with a 5-minute transition buffer. |
| Mention of *"tables of 4"* or *"pairs"* | Teacher favors collaborative/kinesthetic learning. | Design group instructions with assigned peer roles (e.g. Reader, Recorder, Presenter). |
| Grade indicator (*"4th Grade"*, *"Middle School"*) | Lexile / reading level and developmental age. | Calibrate vocabulary, instructions, and complexity strictly to that age group. |
| Note about an *"exit slip"* or *"check"* | Formative assessment needed before class dismissal. | Provide a 2–3 question half-sheet exit ticket with a teacher answer key. |

---

## 2. Office & Business Administration Signals

| Signal / Clue | What It Tells You | Recommended Action |
| :--- | :--- | :--- |
| Open-ended blank lines on forms (e.g. *"Reason: _____"*) | High administrative friction; users write messy, non-standard answers. | Replace with standardized checkboxes, drop-downs, or rating scales. |
| Frustration notes (e.g. *"people keep losing X"*) | Process lacks accountability or closure. | Add dual-stage check-in/check-out fields and signed agreements. |
| Mention of dates, approval, or department codes | Multi-stakeholder compliance workflow. | Include manager sign-off blocks, audit timestamps, and routing tags. |
| Raw tabular figures or budget notes | Executive or finance audience. | Structure into summary KPI cards, before/after comparisons, and timeline milestones. |

---

## 3. Creative & Communications Signals

| Signal / Clue | What It Tells You | Recommended Action |
| :--- | :--- | :--- |
| Punchy, energetic sentence fragments | Author prefers conversational, modern copy. | Never sanitize into formal academic/corporate prose. Preserve the punch. |
| Outlines with empty sub-bullets | Author has established the narrative arc. | Expand directly within their existing headings rather than rearranging the structure. |
| Bolded words or highlighted themes | The author's core thesis or unique angle. | Ensure the expanded content centers squarely around those highlighted concepts. |

---

## 4. Technical & Engineering Signals

| Signal / Clue | What It Tells You | Recommended Action |
| :--- | :--- | :--- |
| `// TODO:` / `// FIXME:` comments | Known technical debt or unfinished aspirations. | Address the root cause highlighted in the comment rather than touching only surface syntax. |
| Specific variable names (`tenantId`, `currencyCode`) | Architectural invariants (multi-tenancy, multi-currency). | Ensure all new queries and mutations uphold these domain boundaries. |
| Flat, single-file scripts | Author prefers minimal, lightweight simplicity. | Do not introduce heavy OOP class hierarchies or alien third-party libraries. |
