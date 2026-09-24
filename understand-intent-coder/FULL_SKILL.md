---
name: understand-intent-coder
description: >-
  All-in-one specialized skill for decoding developer intent in application coding, UI/UX design coherence,
  and defensive security by default. Contains the complete 4-pillar coder protocol, design token checklists,
  OWASP security audits, UI and security case studies, and developer alignment templates.
---

# Understand Intent Coder Skill (All-in-One Edition) 💻🎨🔒

> **Single-File Complete Edition**: Contains the entire Understand Intent Coder protocol, design system and UI/UX checklists, OWASP defensive security guidelines, two full real-world case studies (responsive dashboard metric card with loading skeletons, and secure magic-byte file upload route), and developer alignment templates in one copy-pasteable document.

---

## Table of Contents
1. [Core Product & Security Mindset](#1-core-product--security-mindset)
2. [The 4-Pillar Coder Intent Protocol](#2-the-4-pillar-coder-intent-protocol)
3. [Coder Intent Audit Checklist (Design & Security)](#3-coder-intent-audit-checklist-design--security)
4. [Security & Design Signals Guide](#4-security--design-signals-guide)
5. [Case Study 1: UI/UX Design Coherence & Loading States](#5-case-study-1-uiux-design-coherence--loading-states)
6. [Case Study 2: Defensive Security by Default (Secure Uploads)](#6-case-study-2-defensive-security-by-default-secure-uploads)
7. [Developer Alignment Framing Templates](#7-developer-alignment-framing-templates)

---

## 1. Core Product & Security Mindset

When developers build applications, they often ask for features in shorthand:
- *"Add a user avatar upload to the settings page"*
- *"Make this pricing table look cleaner and responsive"*
- *"Hook up the comment form to our database"*
- *"Add a dark mode toggle"*

Standard AI coding assistants routinely fail in two major ways:
1. **Design Blindness**: Injecting raw, unstyled HTML or conflicting inline CSS that breaks the visual rhythm of the app, ignoring the existing Tailwind theme, typography, component library (Shadcn, Radix), responsive breakpoints, and accessibility.
2. **Security Vulnerability**: Generating insecure starter code: missing authentication checks, accepting arbitrary file uploads with `.php`/`.svg` scripts (leading to RCE or Stored XSS), omitting CSRF tokens, neglecting input sanitization, and ignoring rate limiting.

The **Understand Intent Coder** skill directs the AI to act like a **Senior Full-Stack Product Engineer**:
- **Bake in Security by Default**: Automatically apply OWASP best practices (MIME sniffing with magic bytes, strict file size limits, parameterized queries, authorization checks, path traversal defenses) without needing to be reminded.
- **Respect Design Coherence**: Reuse existing UI building blocks, color tokens, spacing scales, and micro-interactions.
- **Complete the Experience**: Proactively supply all UX states (loading skeletons, empty states, error retry handling, accessible keyboard focus).

---

## 2. The 4-Pillar Coder Intent Protocol

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

### Pillar 1: Code & Design Archaeology
- **Styling Architecture**: Check `tailwind.config.js`, CSS Modules, or theme tokens. Identify brand primary/secondary colors, border radius conventions (`rounded-xl` vs `rounded-md`), and typography scales.
- **Component Primitives**: Inspect `components/ui/` (e.g. Shadcn/Radix/HeadlessUI) to reuse `<Button>`, `<Dialog>`, `<Input>`, and `<Card>` rather than reinventing raw elements.
- **Auth & Session Context**: Identify where the user identity is stored (`useSession()`, `useAuth()`, JWT in cookies) and enforce authorization on mutations.

### Pillar 2: Developer Intent & Workflow Triangulation
- **Look Beyond the Literal Request**: When asked for a "form", the developer also needs validation schemas (Zod/Yup), inline field error messages, disable-on-submitting states, and post-submit toasts/redirects.
- **Read Between the Lines**: If the user asks to "hook this up to our API", trace the existing API routes and data-fetching hooks (React Query, SWR, Server Actions) and wire them up consistently.

### Pillar 3: Outside-the-Box Security & UX Anticipation
- **Security Defaults (Never Skip)**:
  * File uploads: Validate magic bytes, generate secure random UUID names, enforce maximum file sizes, and store outside webroot.
  * Inputs: Sanitize HTML/Markdown inputs to block XSS, use parameterized queries to block SQLi.
  * Auth: Ensure resource modification endpoints check user ownership (`req.user.id === resource.ownerId`).
  * Rate Limiting: Apply request throttles on sensitive endpoints.
- **Complete UX State Coverage**:
  * Include Loading skeletons, Empty states (when list is empty), and Error states (with friendly message and retry).
  * Ensure Accessibility: semantic HTML, ARIA tags, and keyboard navigation.

### Pillar 4: Cohesive, Non-Destructive Implementation
- **Zero Incompatible Boilerplate**: Do not introduce alien styling libraries or unrequested dependencies.
- **Transparent Alignment**: Briefly highlight design and security decisions.

---

## 3. Coder Intent Audit Checklist (Design & Security)

### A. UI/UX Design & Styling
- [ ] **Design Token Consistency**: Uses semantic project tokens (`bg-card`, `text-primary`, `border-border`) rather than hardcoded hex colors.
- [ ] **Spacing & Radii**: Border rounding matches project standard (`rounded-xl` vs `rounded-md`).
- [ ] **Responsive Breakpoints**: Layout adapts gracefully across mobile (`sm:`), tablet (`md:`), and desktop (`lg:`, `xl:`).
- [ ] **Component Reuse**: Reuses primitives from `components/ui/` (`<Button>`, `<Input>`, `<Modal>`, `<Card>`).
- [ ] **Icon Set Consistency**: Retains the existing icon library (Lucide, Heroicons, Radix Icons).

### B. Complete UX State Coverage
- [ ] **Idle State**: Clean, initial presentation with intuitive call-to-actions.
- [ ] **Loading State**: Skeleton loaders, pulsing placeholders, or disabled buttons with spinner (`<Loader2 className="animate-spin" />`).
- [ ] **Empty State**: Friendly illustration/icon, clear copy explaining why it's empty, and an action to create or get started.
- [ ] **Error State**: Actionable error message with a "Try Again" button.
- [ ] **Accessibility (a11y)**: Semantic HTML (`<button>`), linked `<label htmlFor="...">`, ARIA tags, and keyboard navigation (`Tab`, `Enter`, `Escape`).

### C. Defensive Security (OWASP)
- [ ] **Authentication**: Endpoint guarded by auth session middleware.
- [ ] **Authorization / Ownership**: Verifies `req.user.id === resource.ownerId` before permitting mutations or deletes.
- [ ] **File Upload Protections**:
  - [ ] Strict file size limit enforced (e.g. 5MB)
  - [ ] Magic byte inspection (MIME sniffing) instead of relying on file extension
  - [ ] Randomized UUID filenames to prevent path traversal
  - [ ] Storage outside public webroot or in private cloud buckets
- [ ] **Injection Prevention**: Parameterized queries (ORM) and HTML sanitization (DOMPurify).
- [ ] **Rate Limiting**: Request throttles on sensitive endpoints.
- [ ] **Data Leakage**: Sensitive fields (passwords, salts, API keys) omitted from API responses.

---

## 4. Security & Design Signals Guide

### Design & UI/UX Signals
| Code Signal | Underlying Intent | Recommended Action |
| :--- | :--- | :--- |
| `cn("base-classes", className)` utility | Project uses `tailwind-merge` + `clsx`. | Always wrap component class strings with `cn(...)` to allow parent overrides. |
| CSS variables like `hsl(var(--primary))` | Theme-driven design system with dark mode. | Use semantic tokens (`bg-primary`, `text-muted-foreground`) instead of static colors. |
| Radix/Headless UI imports | Accessible primitive architecture. | Use the project's wrapped components in `components/ui/`. |
| Skeletons in sibling components | App values perceived performance. | Always build a loading skeleton state for newly created components. |

### Defensive Security Signals
| Code Signal | Underlying Intent | Recommended Action |
| :--- | :--- | :--- |
| Zod/Yup schemas at route boundaries | Strict type-safe input validation. | Never access `req.body` directly; always validate and infer types via schema. |
| `getServerSession(authOptions)` | Session-based authorization. | Protect all new mutation and data endpoints with the established session guard. |
| `dangerouslySetInnerHTML` | Potential XSS risk surface. | Ensure user-generated markup is passed through `DOMPurify.sanitize(content)`. |
| Cloud storage uploads | Scalable asset storage. | Generate short-lived presigned URLs for client uploads rather than buffering on server. |

---

## 5. Case Study 1: UI/UX Design Coherence & Loading States

### Existing Bare-Bones Component
```tsx
export function MetricCard({ title, value, change }: { title: string; value: string; change: string }) {
  return (
    <div style={{ border: '1px solid #ccc', padding: '16px' }}>
      <h3>{title}</h3>
      <p>{value}</p>
      <span>{change}</span>
    </div>
  );
}
```
**User Prompt**: *"Can you make this metric card look modern and fit with our dashboard?"*

### AI Response with Coder Intent:
1. Replaces inline styles with semantic Tailwind design tokens (`bg-card`, `border-border`, `text-card-foreground`).
2. Adds dynamic trend pills (`text-emerald-500` / `bg-emerald-500/10` for positive, `text-rose-500` / `bg-rose-500/10` for negative) with Lucide `TrendingUp` / `TrendingDown` icons.
3. Provides an exportable `<MetricCardSkeleton />` component for perceived loading performance.

---

## 6. Case Study 2: Defensive Security by Default (Secure Uploads)

### User Prompt
> *"Can you create the avatar upload API route?"*

### AI Implementation with Defensive Security Defaults:
1. **Session Auth Guard**: Verifies caller is logged in; returns 401 if unauthenticated.
2. **File Size Enforcement**: Rejects payloads over 5MB with 413 Payload Too Large.
3. **Magic Byte Verification**: Inspects the first 12 bytes of the buffer to verify genuine image headers (PNG: `89 50 4E 47`, JPEG: `FF D8 FF`, WebP: `RIFF...WEBP`), blocking extension spoofing and HTML/SVG XSS scripts.
4. **Randomized UUID Filename**: Generates `${session.user.id}-${crypto.randomUUID()}${extension}` to eliminate path traversal vulnerabilities.
5. **Database Update**: Links public URL to the authenticated user's record.

---

## 7. Developer Alignment Framing Templates

### UI/UX Design Coherence
> *"I built the `ProjectCard` component matching your Tailwind tokens (`bg-card`, `border-border`, `text-card-foreground`). It uses your existing `<Button>` and `<Badge>` primitives, adapts smoothly from single-column on mobile to a 3-column grid on desktop, and includes a matching `ProjectCardSkeleton` for loading states."*

### Defensive Security
> *"I implemented the avatar upload handler with defensive security defaults: session auth validation, magic-byte inspection (verifying genuine PNG/JPEG headers to block XSS), strict 5MB size limits, and randomized UUID filenames to eliminate path traversal hazards."*

### Full-Stack Form & Mutation
> *"I connected the profile edit form to `/api/user/profile` using your shared Zod schema. The form disables during submission with an inline spinner, updates your client-side React Query cache optimistically, and displays your project's toast notification on error or success."*
