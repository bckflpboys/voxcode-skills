# Understand Intent Coder Skill 💻🎨🔒

> **Universal AI Skill for Decoding Developer Intent in Application Coding, UI/UX Design Coherence, and Defensive Security by Default.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Format: Universal](https://img.shields.io/badge/Format-Universal%20(Markdown%20%7C%20JSON%20%7C%20YAML)-green.svg)](#universal-compatibility--importing)
[![Skill: Product Coder Standard](https://img.shields.io/badge/Skill-Senior%20Product%20Coder-purple.svg)](#the-core-coder-protocol)

---

## What is Understand Intent Coder?

While [Understand Intent](../understand-intent) is for general human work (teaching, office, writing) and [Understand Intent Engineer](../understand-intent-engineer) is for deep systems and mathematics, **Understand Intent Coder** is built specifically for **application developers, product engineers, frontend/backend builders, and designers**.

When developers build applications, they often ask for things tersely:
- *"Add a user avatar upload to the settings page"*
- *"Make this pricing table look cleaner and responsive"*
- *"Hook up the comment form to our database"*
- *"Add a dark mode toggle"*

Standard AI coding assistants routinely fail in two major ways:
1. **Design Blindness**: They inject raw, unstyled HTML or conflicting inline CSS that completely breaks the visual rhythm of the app, ignoring the existing Tailwind theme, typography, component library (Shadcn, Radix), responsive breakpoints, and accessibility.
2. **Security Vulnerability**: They generate insecure starter code: missing authentication checks, accepting arbitrary file uploads with `.php`/`.svg` scripts (leading to RCE or Stored XSS), omitting CSRF tokens, neglecting input sanitization, and ignoring rate limiting.

The **Understand Intent Coder** skill directs the AI to act like a **Senior Full-Stack Product Engineer**:
- **Bake in Security by Default**: Automatically apply OWASP best practices (MIME sniffing with magic bytes, strict file size limits, parameterized queries, authorization checks, path traversal defenses) without needing to be reminded.
- **Respect Design Coherence**: Reuse existing UI building blocks, color tokens, spacing scales, and micro-interactions.
- **Complete the Experience**: Proactively supply all UX states (loading skeletons, empty states, error retry handling, accessible keyboard focus).

---

## Universal Compatibility & Importing

This skill works across all environments:

### 1. Antigravity & Agentic IDEs (Native Skill)
```bash
# In your target project root
mkdir -p .agents/skills/
cp -r understand-intent-coder .agents/skills/

# Or user global level
cp -r understand-intent-coder ~/.gemini/config/skills/
```

### 2. Cursor, Windsurf, & VS Code AI Assistants
Add to your `.cursorrules` or `.windsurfrules`:
```markdown
# Understand Intent Coder Rule
Read and adhere to: ./skills/understand-intent-coder/README.md
Always respect existing design tokens, Tailwind rules, and component libraries. Apply OWASP defensive security defaults (sanitization, auth checks, file validation) and provide complete UX states (loading, empty, error) for all UI components.
```

### 3. Claude Projects & ChatGPT (Custom GPTs / System Prompts)
Directly import or copy this `README.md` (or [prompt.md](./prompt.md)) into:
- **Claude Projects Knowledge**: Upload `understand-intent-coder/README.md` as project knowledge.
- **ChatGPT Custom GPT Instructions**: Copy the contents of [prompt.md](./prompt.md) into the Instructions box.
- **System Prompt**: Append the core protocol section below into your system prompt.

### 4. Custom Agents, Developer Portals & CI Workflows
Read the JSON manifest [skill.json](./skill.json) or ingest this markdown file:
```python
from pathlib import Path

skill_path = Path("skills/understand-intent-coder/README.md")
coder_protocol = skill_path.read_text(encoding="utf-8")

agent_system_prompt = f"""
You are a Senior Full-Stack Product Engineer and Security Reviewer.
Always adhere to the Understand Intent Coder protocol:

{coder_protocol}
"""
```

---

## The Core Coder Protocol

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
  * File uploads: Validate magic bytes (not just file extension), generate secure random UUID names, enforce maximum file sizes, and store outside webroot.
  * Inputs: Sanitize HTML/Markdown inputs to block XSS, use parameterized queries to block SQLi.
  * Auth: Ensure resource modification endpoints check user ownership (`req.user.id === resource.ownerId`).
  * Rate Limiting: Apply request throttles on sensitive endpoints.
- **Complete UX State Coverage**:
  * Include Loading skeletons, Empty states (when list is empty), and Error states (with friendly message and retry).
  * Ensure Accessibility: semantic HTML, ARIA tags, and keyboard navigation.

### Pillar 4: Cohesive, Non-Destructive Implementation
- **Zero Incompatible Boilerplate**: Do not introduce alien styling libraries or unrequested dependencies.
- **Transparent Alignment**: Briefly highlight design and security decisions:
  > *"I implemented the avatar uploader using your existing Tailwind theme and Radix Dialog. I added client-side image preview with aspect-ratio cropping, validated file magic bytes and 5MB size limits on the API route, and updated your UserContext state immediately upon success."*

---

## Real-World Comparison: Coder Intent

### Scenario: User Avatar Upload Request
**User Prompt**: *"Can you add an avatar upload to the user profile settings?"*

---

### ❌ Shallow AI Response (Vulnerable & Unstyled)
```html
<!-- Untrained AI injects raw HTML with zero styling and insecure upload -->
<form action="/api/upload" method="POST" enctype="multipart/form-data">
  <input type="file" name="avatar" />
  <button type="submit">Upload</button>
</form>
```
```javascript
// Naive backend handler
app.post('/api/upload', upload.single('avatar'), (req, res) => {
  // Saves file directly using user-supplied filename! (Path traversal hazard)
  // No file type validation (attacker uploads .php or .svg with XSS payload)
  // No file size limits (DoS hazard)
  // No auth check: Anyone can overwrite anyone's avatar!
  fs.renameSync(req.file.path, `./public/avatars/${req.file.originalname}`);
  res.json({ success: true });
});
```

---

###  AI with "Understand Intent Coder" Skill
The AI performs Code Archaeology:
1. **Design System**: Detects Tailwind CSS + Lucide icons + Shadcn UI primitives.
2. **UX Intent**: The developer wants a polished, modern profile setting with drag-and-drop, instantaneous image preview, a loading spinner during upload, and a toast confirmation.
3. **Security Intent**:
   - Generates randomized UUID filename (`crypto.randomUUID() + ".webp"`).
   - Validates magic bytes (JPEG/PNG/WebP only; rejects `.svg` with inline scripts).
   - Enforces 5MB max payload limit.
   - Verifies session token and ensures the user can only update their own avatar.
   - Resizes/optimizes image with `sharp` to strip malicious EXIF metadata.

```typescript
// Frontend Component (Cohesive with Tailwind & Shadcn)
import React, { useState } from 'react';
import { Camera, Loader2 } from 'lucide-react';
import { Button } from '@/components/ui/button';
import { useToast } from '@/components/ui/use-toast';

export function AvatarUploader({ currentAvatarUrl, onUploadSuccess }: { currentAvatarUrl?: string; onUploadSuccess: (url: string) => void }) {
  const [preview, setPreview] = useState<string | null>(currentAvatarUrl || null);
  const [isUploading, setIsUploading] = useState(false);
  const { toast } = useToast();

  const handleFileChange = async (e: React.ChangeEvent<HTMLInputElement>) => {
    const file = e.target.files?.[0];
    if (!file) return;

    if (file.size > 5 * 1024 * 1024) {
      toast({ title: "File too large", description: "Avatar must be under 5MB", variant: "destructive" });
      return;
    }

    // Client-side preview
    setPreview(URL.createObjectURL(file));
    setIsUploading(true);

    const formData = new FormData();
    formData.append("avatar", file);

    try {
      const res = await fetch("/api/user/avatar", { method: "POST", body: formData });
      if (!res.ok) throw new Error((await res.json()).error || "Upload failed");
      const data = await res.json();
      toast({ title: "Avatar updated", description: "Your new profile picture is live." });
      onUploadSuccess(data.avatarUrl);
    } catch (err: any) {
      toast({ title: "Upload error", description: err.message, variant: "destructive" });
      setPreview(currentAvatarUrl || null);
    } finally {
      setIsUploading(false);
    }
  };

  return (
    <div className="flex items-center gap-4">
      <div className="relative h-20 w-20 overflow-hidden rounded-full border-2 border-border bg-muted shadow-sm">
        {preview ? (
          <img src={preview} alt="Avatar Preview" className="h-full w-full object-cover" />
        ) : (
          <div className="flex h-full w-full items-center justify-center text-muted-foreground text-sm font-semibold">User</div>
        )}
        {isUploading && (
          <div className="absolute inset-0 flex items-center justify-center bg-black/40 text-white">
            <Loader2 className="h-5 w-5 animate-spin" />
          </div>
        )}
      </div>
      <div>
        <label htmlFor="avatar-input">
          <Button variant="outline" size="sm" asChild disabled={isUploading} className="cursor-pointer gap-2">
            <span>
              <Camera className="h-4 w-4" /> Change Avatar
            </span>
          </Button>
        </label>
        <input id="avatar-input" type="file" accept="image/png, image/jpeg, image/webp" className="sr-only" onChange={handleFileChange} />
        <p className="mt-1 text-xs text-muted-foreground">PNG, JPG, or WebP. Max 5MB.</p>
      </div>
    </div>
  );
}
```

---

## Skill Directory Structure

```text
understand-intent-coder/
├── SKILL.md                                 # Agentic standard skill file with frontmatter
├── README.md                                # Universal documentation & self-contained prompt
├── prompt.md                                # Raw system prompt for direct copy-pasting
├── skill.json                               # Machine-readable JSON manifest
├── references/
│   ├── coder-intent-checklist.md            # Tactical checklist for UI/UX & defensive security
│   └── security-and-design-signals.md       # Signals in design systems & security perimeters
├── examples/
│   ├── ui-design-intent.md                  # Responsive dashboard card & design token case study
│   └── security-defensive-intent.md         # Bulletproof secure file upload case study
└── resources/
    └── coder-alignment-templates.md         # High-signal framing templates for developers
```

---

## License

This skill is open source under the [MIT License](../../LICENSE).
