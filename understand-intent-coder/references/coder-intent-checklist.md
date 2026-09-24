# Coder Intent Checklist

Use this checklist whenever creating, updating, or refactoring application components, forms, endpoints, and full-stack features.

---

## 1. UI/UX Design & Styling Checklist
- [ ] **Design Token Consistency**: Does the component use project color tokens (e.g. `bg-background`, `text-primary`, `border-border`) rather than hardcoded hex colors?
- [ ] **Spacing & Radii**: Does border rounding match the project standard (`rounded-xl` vs `rounded-md`)?
- [ ] **Responsive Breakpoints**: Does the layout adapt gracefully across mobile (`sm:`), tablet (`md:`), and desktop (`lg:`, `xl:`) screens?
- [ ] **Component Reuse**: Did we reuse existing primitives from `components/ui/` (`<Button>`, `<Input>`, `<Modal>`, `<Card>`)?
- [ ] **Icon Set Consistency**: Did we stick with the existing icon library (e.g. Lucide, Heroicons, Radix Icons)?

---

## 2. Complete UX State Coverage
- [ ] **Idle State**: Clean, initial presentation with intuitive call-to-actions.
- [ ] **Loading State**: Skeleton loaders, pulsing placeholders, or disabled buttons with spinner (`<Loader2 className="animate-spin" />`).
- [ ] **Empty State**: Friendly illustration/icon, clear copy explaining why it's empty, and an action to create or get started.
- [ ] **Error State**: Non-technical, actionable error message with a "Try Again" button.
- [ ] **Accessibility (a11y)**:
  - [ ] Semantic HTML (`<button>` instead of `<div onClick>`)
  - [ ] Accessible form inputs with linked `<label htmlFor="...">`
  - [ ] ARIA tags (`aria-expanded`, `aria-busy`, `aria-label`)
  - [ ] Keyboard navigation (`Tab`, `Enter`, `Escape`)

---

## 3. Defensive Security Checklist (OWASP)
- [ ] **Authentication & Session**: Is the endpoint or route guarded by auth middleware?
- [ ] **Authorization / Ownership**: Does the code verify that `req.user.id === resource.ownerId` before permitting mutations or deletes?
- [ ] **File Upload Protections**:
  - [ ] File size limit enforced (e.g. max 5MB for images)
  - [ ] Magic byte inspection (MIME sniffing) instead of relying on file extension
  - [ ] Randomized filenames (UUID) to prevent path traversal
  - [ ] Storage outside public webroot or in private cloud buckets
- [ ] **Input Sanitization & Injection Prevention**:
  - [ ] Parameterized database queries (ORM or SQL parameters) to block SQLi
  - [ ] HTML/Markdown sanitization (e.g. DOMPurify) to prevent Stored XSS
- [ ] **Rate Limiting & Abuse Prevention**: Are public or expensive endpoints protected by rate limiters (e.g. Redis sliding window or in-memory token bucket)?
- [ ] **Data Leakage**: Ensure sensitive fields (passwords, salts, API keys, internal IDs) are omitted from JSON API responses.
