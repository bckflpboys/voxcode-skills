# Example: UI/UX Design Intent

This case study demonstrates how an AI model applies the **Understand Intent Coder** skill to decode a developer's visual intent, match their design system, and implement full UX states from a terse request.

---

## 1. The Context (Existing Raw Component)

A developer working on a SaaS analytics app has a bare-bones metric card in `src/components/MetricCard.tsx`:

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

The rest of the app uses **Tailwind CSS**, **Lucide React icons**, and **Shadcn UI** conventions with dark mode support.

---

## 2. The User Prompt
> *"Can you make this metric card look modern and fit with our dashboard?"*

---

## 3. ❌ Shallow AI Response (Failed Intent)
The untrained AI replaces inline styles with basic Tailwind classes, but ignores the app's tokens and fails to handle trend indicators or loading states:

```tsx
export function MetricCard({ title, value, change }: any) {
  return (
    <div className="bg-white p-4 rounded shadow border">
      <h3 className="text-gray-500 font-bold">{title}</h3>
      <p className="text-2xl font-bold">{value}</p>
      <span className="text-green-500">{change}</span>
    </div>
  );
}
```
**Why this fails**:
- Uses hardcoded `bg-white` and `text-gray-500` (breaks dark mode!).
- Hardcodes `text-green-500` without checking whether `change` is positive or negative.
- Ignores icons, trend indicators, tooltips, and loading skeleton states.

---

## 4.  AI with "Understand Intent Coder" Skill

### Step 1: Design System Archaeology
1. **Design Tokens**: The project uses semantic HSL tokens: `bg-card`, `text-card-foreground`, `border-border`, `text-muted-foreground`.
2. **Typography & Hierarchy**: Primary numbers should be `text-2xl font-bold tracking-tight`, headers should be `text-sm font-medium text-muted-foreground`.
3. **Data Polish**: Trend changes need dynamic color styling (`text-emerald-500` vs `text-rose-500`) and appropriate trend arrow icons (`TrendingUp`, `TrendingDown`).
4. **UX States**: Provide a companion `<MetricCardSkeleton />` for seamless React Suspense or loading states.

---

### Step 2: Implementation

```tsx
import React from 'react';
import { TrendingUp, TrendingDown, Minus } from 'lucide-react';
import { cn } from '@/lib/utils';

export interface MetricCardProps {
  title: string;
  value: string | number;
  change?: number; // e.g. +12.4 or -3.2 (percentage)
  periodLabel?: string; // e.g. "vs last month"
  icon?: React.ReactNode;
  className?: string;
}

export function MetricCard({
  title,
  value,
  change,
  periodLabel = "from last month",
  icon,
  className,
}: MetricCardProps) {
  const isPositive = change !== undefined && change > 0;
  const isNegative = change !== undefined && change < 0;

  return (
    <div
      className={cn(
        "group relative overflow-hidden rounded-xl border border-border bg-card p-6 text-card-foreground shadow-sm transition-all duration-200 hover:shadow-md hover:border-border/80",
        className
      )}
    >
      <div className="flex items-center justify-between space-y-0 pb-2">
        <span className="text-sm font-medium text-muted-foreground tracking-tight">{title}</span>
        {icon && (
          <div className="rounded-lg bg-muted p-2 text-muted-foreground group-hover:text-primary transition-colors">
            {icon}
          </div>
        )}
      </div>

      <div className="mt-2 flex items-baseline justify-between">
        <div className="text-2xl font-bold tracking-tight">{value}</div>

        {change !== undefined && (
          <div
            className={cn(
              "inline-flex items-center gap-1 rounded-full px-2 py-0.5 text-xs font-semibold",
              isPositive && "bg-emerald-500/10 text-emerald-600 dark:text-emerald-400",
              isNegative && "bg-rose-500/10 text-rose-600 dark:text-rose-400",
              !isPositive && !isNegative && "bg-muted text-muted-foreground"
            )}
          >
            {isPositive && <TrendingUp className="h-3 w-3 stroke-[2.5]" />}
            {isNegative && <TrendingDown className="h-3 w-3 stroke-[2.5]" />}
            {!isPositive && !isNegative && <Minus className="h-3 w-3" />}
            <span>
              {isPositive ? `+${change}%` : `${change}%`}
            </span>
          </div>
        )}
      </div>

      {periodLabel && (
        <p className="mt-2 text-xs text-muted-foreground">
          {periodLabel}
        </p>
      )}
    </div>
  );
}

// Built-in loading skeleton for seamless perceived performance
export function MetricCardSkeleton({ className }: { className?: string }) {
  return (
    <div className={cn("rounded-xl border border-border bg-card p-6 shadow-sm", className)}>
      <div className="flex items-center justify-between pb-2">
        <div className="h-4 w-24 animate-pulse rounded bg-muted" />
        <div className="h-8 w-8 animate-pulse rounded-lg bg-muted" />
      </div>
      <div className="mt-3 flex items-baseline justify-between">
        <div className="h-7 w-32 animate-pulse rounded bg-muted" />
        <div className="h-5 w-14 animate-pulse rounded-full bg-muted" />
      </div>
      <div className="mt-3 h-3 w-28 animate-pulse rounded bg-muted" />
    </div>
  );
}
```

---

### Step 3: Aligned Communication
The AI explains:
> *"I updated your `MetricCard` to match your Tailwind design tokens and dark mode palette (`bg-card`, `border-border`, `text-card-foreground`). I added dynamic positive/negative trend pills with Lucide icons, subtle hover elevation, and an exportable `MetricCardSkeleton` for smooth loading states."*
