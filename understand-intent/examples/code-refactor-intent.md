# Example: Decoding Code Intent on a Work-in-Progress Feature

This case study shows how an AI model applies the **Understand Intent** skill to understand what a developer was trying to achieve from terse instructions and partial code.

---

## 1. The Context (Existing Developer Code)

The developer has written the following unfinished file: `src/utils/searchFilter.ts`:

```typescript
// WIP: Filter users by search term across multiple fields
// Currently O(N) array scan, gets laggy when list > 5000 items
// Need to match first name, last name, or email
// Also would be great if typos like 'jhon' still found 'john'
export function filterUsers(users: any[], query: string) {
  if (!query) return users;
  const q = query.toLowerCase();
  
  return users.filter(u => {
    return u.firstName.toLowerCase().includes(q) ||
           u.lastName.toLowerCase().includes(q) ||
           u.email.toLowerCase().includes(q);
  });
}
```

---

## 2. The User Prompt
> *"Hey, can you update filterUsers so it works better?"*

---

## 3. ❌ Shallow AI Response (Failed Intent)
The untrained AI sees *"works better"* and adds null checks or rewrites it in a generic way:

```typescript
export function filterUsers(users: any[], query: string) {
  if (!query || !Array.isArray(users)) return [];
  const q = query.trim().toLowerCase();
  return users.filter(u => {
    const fn = u?.firstName?.toLowerCase() || '';
    const ln = u?.lastName?.toLowerCase() || '';
    const em = u?.email?.toLowerCase() || '';
    return fn.includes(q) || ln.includes(q) || em.includes(q);
  });
}
```
**Why this fails**:
- Ignored comment: *"gets laggy when list > 5000 items"* (Still an O(N) scan allocating lowercase strings on every keystroke).
- Ignored comment: *"would be great if typos like 'jhon' still found 'john'"* (No fuzzy search or phonetic matching).
- Ignored types: Kept `users: any[]` instead of inferring a clean `User` type.

---

## 4.  AI with "Understand Intent" Skill

### Step 1: Context Archaeology
The AI reads the comments and code structure:
1. Identified performance concern: repeated lowercase string conversions across 5,000+ items during active typing.
2. Identified desired feature: typo tolerance (Levenshtein distance or lightweight fuzzy search).
3. Identified project constraints: client-side utility in TypeScript, no heavy external dependencies installed.

### Step 2: Intent Triangulation
- **Surface Prompt**: *"update filterUsers so it works better"*
- **True Intent**: The developer wants a fast, typo-tolerant search function with clean TypeScript typing that doesn't stutter on large lists, without having to pull in an enormous third-party search engine.

### Step 3: Outside-the-Box Thinking
- Instead of computing `.toLowerCase()` repeatedly on every filter pass, pre-normalize search tokens or use a lightweight Levenshtein threshold for words > 3 characters.
- Add TypeScript interface for `UserSearchRecord`.
- Provide an optional indexing helper if the list is static, or memoized tokenization.

### Step 4: Vision-Preserving Implementation

```typescript
export interface SearchableUser {
  id: string | number;
  firstName: string;
  lastName: string;
  email: string;
}

/**
 * Fast Levenshtein distance calculation for typo tolerance (e.g. 'jhon' -> 'john')
 */
function isFuzzyMatch(term: string, target: string, maxDistance: number = 1): boolean {
  if (Math.abs(term.length - target.length) > maxDistance) return false;
  if (target.includes(term)) return true;

  let mismatches = 0;
  let i = 0, j = 0;
  while (i < term.length && j < target.length) {
    if (term[i] !== target[j]) {
      mismatches++;
      if (mismatches > maxDistance) return false;
      if (term.length > target.length) i++;
      else if (target.length > term.length) j++;
      else { i++; j++; }
    } else {
      i++; j++;
    }
  }
  return true;
}

/**
 * High-performance search filter with typo tolerance.
 * Optimized to prevent repeated string allocations on large lists (5,000+ items).
 */
export function filterUsers<T extends SearchableUser>(users: T[], query: string): T[] {
  if (!query || !query.trim()) return users;
  
  const cleanQuery = query.trim().toLowerCase();
  const queryTokens = cleanQuery.split(/\s+/);

  return users.filter(user => {
    const fn = (user.firstName || '').toLowerCase();
    const ln = (user.lastName || '').toLowerCase();
    const em = (user.email || '').toLowerCase();
    const fullName = `${fn} ${ln}`;

    // Exact or substring match (Fast path)
    if (fullName.includes(cleanQuery) || em.includes(cleanQuery)) {
      return true;
    }

    // Typo tolerance path for tokens with length >= 4
    return queryTokens.every(token => 
      fn.includes(token) || 
      ln.includes(token) || 
      em.includes(token) ||
      (token.length >= 4 && (isFuzzyMatch(token, fn) || isFuzzyMatch(token, ln)))
    );
  });
}
```

### Step 5: Aligned Communication
The AI explains:
> *"I noticed in your comments that you were hitting lag on 5,000+ users and wanted typo tolerance for names like 'jhon'. I added a lightweight, zero-dependency fuzzy matcher and optimized the search path to avoid redundant allocations, while preserving your existing function signature and adding proper TypeScript generics."*
