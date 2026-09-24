# Coder Alignment Templates

When delivering full-stack code, developers appreciate knowing that their design tokens, component architecture, and security boundaries were respected. Use these framing templates.

---

## 1. UI/UX Design Coherence Framing

### Pattern
> *"I built `[Component]` following your existing [Tailwind / CSS module / theme] tokens (`[specific classes/tokens used]`). I integrated [primitives: e.g. Radix / Shadcn] with responsive breakpoints and included [UX states: e.g. loading skeletons / empty states / micro-interactions]."*

### Example
> *"I built the `ProjectCard` component matching your Tailwind tokens (`bg-card`, `border-border`, `text-card-foreground`). It uses your existing `<Button>` and `<Badge>` primitives, adapts smoothly from single-column on mobile to a 3-column grid on desktop, and includes a matching `ProjectCardSkeleton` for loading states."*

---

## 2. Defensive Security Framing

### Pattern
> *"I implemented `[Endpoint / Feature]` incorporating OWASP defensive defaults: [security measures: e.g. magic byte inspection / auth session checks / randomized UUID names / parameterized queries], preventing [vulnerabilities: e.g. path traversal / stored XSS / unauthorized mutations]."*

### Example
> *"I implemented the avatar upload handler with defensive security defaults: session auth validation, magic-byte inspection (verifying genuine PNG/JPEG headers to block XSS), strict 5MB size limits, and randomized UUID filenames to eliminate path traversal hazards."*

---

## 3. Full-Stack Form & Mutation Framing

### Pattern
> *"I connected `[Form]` to `[API Endpoint]` using [validation library: e.g. Zod] schemas for both client and server validation. The submit button handles loading spinners, disables during in-flight requests, and triggers a [toast / notification] with automatic error rollback."*

### Example
> *"I connected the profile edit form to `/api/user/profile` using your shared Zod schema. The form disables during submission with an inline spinner, updates your client-side React Query cache optimistically, and displays your project's toast notification on error or success."*
