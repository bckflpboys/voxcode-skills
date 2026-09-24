---
name: understand-intent-coder
description: >-
  Decodes developer intent with deep focus on application coding, UI/UX design coherence,
  and defensive security best practices. Use when building or updating frontend components,
  backend routes, forms, file handlers, or full-stack features. Instructs the agent to
  perform code archaeology on existing styling tokens, component hierarchies, and auth patterns,
  proactively apply OWASP security defaults and UX polish, and build features that integrate
  seamlessly without breaking the author's vision or architecture.
---

# Understand Intent Coder Skill

The **Understand Intent Coder** skill embodies the mindset of a senior product engineer and security-conscious full-stack developer. When developers or product builders ask an AI to create or update features, they often communicate in shorthand:
- *"Add a profile picture upload"*
- *"Make this card component look cleaner and responsive"*
- *"Add a comment box below the article"*
- *"Hook this form up to our API"*

Standard AI models frequently commit two critical sins:
1. **Design Blindness**: They inject generic, unstyled HTML or jarring inline styles that clash with the app's established design system, ignoring color tokens, typography, padding scales, dark mode, loading states, and mobile responsiveness.
2. **Security Negligence**: They write naive, vulnerable code—storing unvalidated files directly to disk (path traversal / RCE), reflecting raw user input into DOM (XSS), missing authentication middleware, omitting CSRF tokens, and ignoring rate limits.

The **Understand Intent Coder** skill directs the agent to decode the author's true product goals, honor their design system, bake in defensive security by default, and deliver clean, idiomatic code that feels like it was written by the project's lead developer.

---

## The 4-Pillar Coder Intent Protocol

```
┌──────────────────────────────────────────────────────────┐
│ Pillar 1: Code & Design Archaeology                      │
│ Design tokens, CSS/Tailwind rules, auth guards, schemas  │
└────────────────────────────┬─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│ Pillar 2: Developer Intent & Workflow Triangulation      │
│ Decode the unstated user experience & architectural goal │
└────────────────────────────┬─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│ Pillar 3: Outside-the-Box Security & UX Anticipation     │
│ OWASP defaults, rate limiting, sanitization, ARIA, states│
└────────────────────────────┬─────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────┐
│ Pillar 4: Cohesive, Non-Destructive Implementation       │
│ Match existing styling, state hooks, and idioms cleanly  │
└──────────────────────────────────────────────────────────┘
```

---

### Pillar 1: Code & Design Archaeology

Before writing code, inspect the existing project assets to learn its patterns:

1. **Design System & Styling Patterns**:
   - Inspect styling configurations: `tailwind.config.js`, CSS variables, Theme providers, or styled-components.
   - What color palette, border radiuses (`rounded-xl` vs `rounded-md`), shadow scales, and spacing units does the app use?
   - How are components structured? (Atomic design, compound components, headless UI with Radix/Shadcn, Lucide/Heroicons).
2. **Security & Authentication Patterns**:
   - What authentication mechanism is in place (NextAuth, Supabase, JWT in HttpOnly cookies, session tokens)?
   - Where are route guards or middleware layers located?
   - How are inputs validated (Zod, Yup, Joi, class-validator)?
3. **State Management & Data Fetching**:
   - How does the app fetch and cache data (React Query / TanStack Query, SWR, Zustand, Redux Toolkit, Vue Pinia)?
   - Match the exact conventions already adopted.

---

### Pillar 2: Developer Intent & Workflow Triangulation

Separate the brief prompt from the complete product feature:

1. **Reconstruct the "Job to Be Done"**:
   - *Prompt*: *"Add an avatar upload"*
     - *Literal Request*: A file input that uploads an image.
     - *True Intent*: An interactive user experience (drag-and-drop or file pick, client-side preview, cropping/aspect ratio validation, progress indicator) backed by a secure API endpoint that saves the image safely, updates the user's session state, and handles errors gracefully.
2. **Audit What's Missing**:
   - Developers frequently omit the edge cases: What happens if the image is 50MB? What happens if someone uploads a `.php` or `.svg` script? What happens when the network fails mid-upload?
   - Address these proactively without needing to be asked.

---

### Pillar 3: Outside-the-Box Security & UX Anticipation

A great developer bakes in security and UX polish automatically:

1. **Defensive Security by Default (OWASP Mindset)**:
   - **File Uploads**: Check magic bytes (MIME sniffing), enforce strict file size limits (e.g. 5MB for avatars), generate randomized UUID filenames (prevent directory traversal), and store outside the webroot or in private S3 buckets.
   - **Input Sanitization**: Sanitize rich text against XSS (DOMPurify, sanitize-html); use parameterized queries against SQLi.
   - **Authentication & Authorization**: Verify that the logged-in user actually owns the resource they are modifying (`req.user.id === resource.ownerId`).
   - **Rate Limiting & Abuse Prevention**: Protect mutation endpoints with IP/user-based rate limiters to stop spam and DoS.
2. **Complete UX State Coverage**:
   - Never provide a component with only a success state. Always include:
     * **Idle / Default State**
     * **Loading / Skeleton State**
     * **Empty State** (friendly message and call-to-action)
     * **Error State** (human-readable error message and retry button)
3. **Accessibility (a11y)**:
   - Include proper semantic tags (`<button>` instead of `<div onClick>`), `aria-expanded`, `aria-label`, and ensure full keyboard navigation (`Enter`, `Space`, `Escape`).

---

### Pillar 4: Cohesive, Non-Destructive Implementation

Deliver code that fits naturally into the codebase:

1. **Zero Foreign Clutter**:
   - If the codebase uses Tailwind CSS, do not inject inline style tags or write custom CSS files unless requested.
   - If the project uses Lucide icons, do not import FontAwesome or raw SVGs.
2. **Component Reusability**:
   - Reuse existing UI building blocks (e.g. `<Button>`, `<Modal>`, `<Card>`, `<Input>`) from the project's `components/ui/` folder instead of creating duplicate primitives.
3. **Communicate Value Transparently**:
   - Briefly summarize the design and security choices made:
     > *"I implemented the avatar uploader matching your existing modal styles and Tailwind theme. I added drag-and-drop with client preview, enforced 5MB limit with magic-byte image validation on the server, and updated your UserContext to reflect the new image instantly."*

---

## Detailed References & Case Studies

- **[Coder Intent Checklist](./references/coder-intent-checklist.md)**: Audit checklist for UI/UX harmony and security.
- **[Security & Design Signals Guide](./references/security-and-design-signals.md)**: Latent signals in styling systems and auth boundaries.
- **[UI/UX Design Case Study](./examples/ui-design-intent.md)**: Elevating an unstyled card into a responsive, accessible dashboard component.
- **[Defensive Security Case Study](./examples/security-defensive-intent.md)**: Implementing a bulletproof, secure file upload flow.
- **[Coder Alignment Templates](./resources/coder-alignment-templates.md)**: Conversational framing templates for developers.
