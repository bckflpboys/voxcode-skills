# Security and Design Signals Guide

When working in an existing codebase, developers leave clear architectural patterns that define their design system and security model.

---

## 1. Design & UI/UX Signals

| Code Signal | Underlying Intent | Recommended Action |
| :--- | :--- | :--- |
| `cn("base-classes", className)` utility | Project uses `tailwind-merge` + `clsx` (Shadcn / Radix pattern). | Always wrap component class strings with `cn(...)` to allow parent overrides cleanly. |
| CSS variables like `hsl(var(--primary))` | Theme-driven design system with dark mode support. | Use semantic tokens (`bg-primary`, `text-muted-foreground`) instead of static colors (`bg-blue-600`). |
| Radix/Headless UI imports (`@radix-ui/react-dialog`) | Accessible, unstyled primitive architecture. | Use the project's wrapped components in `components/ui/` rather than adding raw HTML dialogs. |
| Skeletons in sibling components (`<Skeleton className="..." />`) | App values perceived performance and smooth loading transitions. | Always build a loading skeleton state for newly created data-fetching components. |
| Framer Motion / Tailwind animate (`motion.div`, `transition-all duration-200`) | Polished micro-interactions and smooth layout transitions. | Add subtle hover and exit animations matching the duration/easing of adjacent components. |

---

## 2. Defensive Security Signals

| Code Signal | Underlying Intent | Recommended Action |
| :--- | :--- | :--- |
| Zod/Yup schemas at route boundaries (`const schema = z.object(...)`) | Strict type-safe input validation. | Never access `req.body` or `params` directly; always validate and infer types via the existing schema library. |
| `getServerSession(authOptions)` / `authMiddleware` | Session-based authorization. | Protect all new mutation and data endpoints with the established session guard. |
| `dangerouslySetInnerHTML` in existing files | Potential XSS risk surface. | Ensure user-generated markup is passed through a sanitizer (e.g. `DOMPurify.sanitize(content)`) before rendering. |
| Cloud storage uploads (`@aws-sdk/client-s3`, `@google-cloud/storage`) | Scalable private asset storage. | Generate short-lived presigned URLs for client uploads rather than buffering entire large files in Node.js server memory. |
| Rate-limit headers (`X-RateLimit-Limit`, Redis upstash) | DDOS and brute-force defenses active. | Attach the rate limiter to newly added endpoints, especially auth, submission, and export routes. |
