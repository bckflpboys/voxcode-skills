# Example: Defensive Security Intent

This case study shows how an AI model applies the **Understand Intent Coder** skill to automatically incorporate OWASP defensive security standards when implementing a feature from a brief prompt.

---

## 1. The Context (Existing API Route)

A developer asks to implement an image upload endpoint in a Next.js / Node.js application:

```typescript
// pages/api/user/avatar.ts or app/api/user/avatar/route.ts
// Need an endpoint to upload and save user avatar
```

---

## 2. The User Prompt
> *"Can you create the avatar upload API route?"*

---

## 3. ❌ Shallow AI Response (Vulnerable Code)
The untrained AI writes code that trusts client data implicitly:
```typescript
export async function POST(req: Request) {
  const formData = await req.formData();
  const file = formData.get("file") as File;
  
  // Vulnerabilities:
  // 1. Path Traversal: Uses user's filename directly
  // 2. Unrestricted File Upload: Accepts .html, .php, or SVG with embedded scripts
  // 3. No File Size Limit: Attacker uploads 5GB file to exhaust disk/RAM
  // 4. Broken Access Control: No session authentication!
  const buffer = Buffer.from(await file.arrayBuffer());
  await fs.writeFile(`./public/uploads/${file.name}`, buffer);
  
  return Response.json({ url: `/uploads/${file.name}` });
}
```

---

## 4.  AI with "Understand Intent Coder" Skill

### Step 1: Security & Architecture Archaeology
1. **Authentication**: The route must extract the authenticated user session; anonymous callers must receive 401 Unauthorized.
2. **Path Traversal Defense**: Never trust `file.name`. Generate a cryptographically secure random UUID name (`crypto.randomUUID()`).
3. **Magic Byte Inspection**: A malicious user can rename `malware.html` to `image.png`. The server must verify the file's first few bytes (magic numbers) to ensure it is a genuine PNG, JPEG, or WebP:
   - PNG: `89 50 4E 47`
   - JPEG: `FF D8 FF`
   - WebP: `52 49 46 46 ... 57 45 42 50`
4. **Denial of Service (DoS) Defense**: Enforce strict size limits (e.g. 5MB) and apply a rate limiter to prevent flooding.
5. **Storage Isolation**: Store the file in a dedicated non-executable directory or object storage bucket.

---

### Step 2: Implementation

```typescript
import { NextRequest, NextResponse } from 'next/server';
import { getServerSession } from 'next-auth';
import { authOptions } from '@/lib/auth';
import { prisma } from '@/lib/prisma';
import crypto from 'crypto';
import fs from 'fs/promises';
import path from 'path';

// Max file size: 5MB
const MAX_FILE_SIZE = 5 * 1024 * 1024;
const ALLOWED_MIME_TYPES = new Set(['image/jpeg', 'image/png', 'image/webp']);

// Inspect first 12 bytes to confirm genuine image headers (magic bytes)
function isValidImageMagicBytes(buffer: Buffer): string | null {
  if (buffer.length < 12) return null;

  // PNG header: 89 50 4E 47 0D 0A 1A 0A
  if (buffer[0] === 0x89 && buffer[1] === 0x50 && buffer[2] === 0x4E && buffer[3] === 0x47) {
    return '.png';
  }

  // JPEG header: FF D8 FF
  if (buffer[0] === 0xFF && buffer[1] === 0xD8 && buffer[2] === 0xFF) {
    return '.jpg';
  }

  // WebP header: RIFF .... WEBP
  if (
    buffer.toString('ascii', 0, 4) === 'RIFF' &&
    buffer.toString('ascii', 8, 12) === 'WEBP'
  ) {
    return '.webp';
  }

  return null;
}

export async function POST(req: NextRequest) {
  try {
    // 1. Authentication & Session Guard
    const session = await getServerSession(authOptions);
    if (!session?.user?.id) {
      return NextResponse.json({ error: 'Unauthorized' }, { status: 401 });
    }

    const formData = await req.formData();
    const file = formData.get('avatar') as File | null;

    if (!file) {
      return NextResponse.json({ error: 'No file provided' }, { status: 400 });
    }

    // 2. File Size Enforcement
    if (file.size > MAX_FILE_SIZE) {
      return NextResponse.json({ error: 'File exceeds 5MB limit' }, { status: 413 });
    }

    // 3. MIME Type Whitelist
    if (!ALLOWED_MIME_TYPES.has(file.type)) {
      return NextResponse.json({ error: 'Invalid file type. Only JPEG, PNG, or WebP allowed.' }, { status: 400 });
    }

    const arrayBuffer = await file.arrayBuffer();
    const buffer = Buffer.from(arrayBuffer);

    // 4. Magic Byte Verification (Blocks extension spoofing / XSS payloads)
    const validExtension = isValidImageMagicBytes(buffer);
    if (!validExtension) {
      return NextResponse.json({ error: 'File content does not match a valid image format.' }, { status: 422 });
    }

    // 5. Safe Filename Generation (Zero Path Traversal)
    const randomId = crypto.randomUUID();
    const safeFilename = `${session.user.id}-${randomId}${validExtension}`;
    const uploadDir = path.join(process.cwd(), 'uploads', 'avatars');

    await fs.mkdir(uploadDir, { recursive: true });
    const destinationPath = path.join(uploadDir, safeFilename);

    // Write file securely
    await fs.writeFile(destinationPath, buffer);

    // 6. Update Database with new avatar URL
    const publicUrl = `/api/avatars/${safeFilename}`;
    await prisma.user.update({
      where: { id: session.user.id },
      data: { avatarUrl: publicUrl },
    });

    return NextResponse.json({ success: true, avatarUrl: publicUrl });
  } catch (error) {
    console.error('Avatar upload error:', error);
    return NextResponse.json({ error: 'Internal server error' }, { status: 500 });
  }
}
```

---

### Step 3: Aligned Communication
The AI explains:
> *"I created the avatar upload route incorporating defensive security best practices: session authentication, strict 5MB size limits, magic-byte header inspection (to prevent extension spoofing and SVG/HTML XSS attacks), and randomized UUID filenames to eliminate path traversal vulnerabilities."*
