---
name: dot-matrix-animation
description: "Use this skill when adding Dot Matrix animated loaders to a React, Next.js, Tailwind, or shadcn/ui app. Covers registry setup, installing loaders, CSS import checks, component usage, and UX/accessibility rules for loading states."
---

# Dot Matrix Animation Loaders

Use this skill when a UI needs small animated loading indicators from the Dot Matrix shadcn registry.

Before implementing, check the latest Dot Matrix docs for current registry URLs, component names, install commands, and CSS import requirements.

## When To Use

- Add a Dot Matrix loader to a React/Next.js app.
- Configure `components.json` so `npx shadcn` can install `@dotmatrix/*` components.
- Replace a generic spinner with a local, stylable dot-matrix loader.
- Improve pending, saving, uploading, or partial-loading UI states.

## Prerequisites

Before installing Dot Matrix loaders, confirm the app has:

- A working React app.
- Tailwind CSS configured.
- shadcn/ui initialized with `components.json`.
- Path aliases aligned with the app's component structure.

If `components.json` is missing, initialize shadcn first:

```bash
npx shadcn@latest init
```

## Registry Setup

Add the Dot Matrix registry to `components.json`:

```json
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "default",
  "tailwind": {
    "css": "app/globals.css",
    "baseColor": "neutral",
    "cssVariables": true
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils",
    "ui": "@/components/ui",
    "lib": "@/lib",
    "hooks": "@/hooks"
  },
  "registries": {
    "@dotmatrix": "https://dotmatrix.zzzzshawn.cloud/r/{name}.json"
  }
}
```

Preserve the existing local `components.json` values. Add or merge only the `registries.@dotmatrix` entry unless the project needs shadcn initialization.

## Install Loaders

Install one loader:

```bash
npx shadcn@latest add @dotmatrix/dotm-square-3
```

Install every loader:

```bash
npx shadcn@latest add @dotmatrix/all
```

Known registry items include:

- `@dotmatrix/dotm-square-3`
- `@dotmatrix/dotm-circular-5`
- `@dotmatrix/dotm-triangle-2`
- `@dotmatrix/all`

Installed components are local project files. Rename, restyle, and retime them when the product design calls for it.

## CSS Import Check

Dot Matrix loaders depend on `dotmatrix-loader.css`. If the install flow does not import it automatically, add the import to the app's global CSS file, adjusting the relative path to match the project:

```css
@import "tailwindcss";
@import "tw-animate-css";
@import "shadcn/tailwind.css";
@import "../components/dotmatrix-loader.css";
@custom-variant dark (&:is(.dark *));
```

For Next.js App Router projects, this is usually `src/app/globals.css` or `app/globals.css`.

## Usage Pattern

Keep the loader close to the pending action. For example, use an inline loader inside a save button:

```tsx
import { DotmSquare3 } from "@/components/ui/dotm-square-3";

export function SaveButton({ isSaving }: { isSaving: boolean }) {
  return (
    <button
      type="button"
      disabled={isSaving}
      className="inline-flex items-center gap-2 rounded-md border px-3 py-2"
    >
      {isSaving ? <DotmSquare3 size={18} dotSize={3} aria-label="Saving" /> : null}
      <span>{isSaving ? "Saving..." : "Save changes"}</span>
    </button>
  );
}
```

## UX Rules

- Use inline loaders when only a local control or region is pending.
- Prefer skeletons when preserving content shape matters more than showing activity.
- Pair motion with accessible text when the loading context is not obvious.
- Avoid multiple competing loaders in a single viewport.
- Keep loader size proportional to surrounding UI; buttons usually need small loaders.
- Do not use decorative loading motion where no async state exists.

## Implementation Checklist

1. Inspect the app's existing Tailwind, shadcn, and component alias setup.
2. Add the `@dotmatrix` registry without overwriting unrelated `components.json` settings.
3. Install only the loader needed unless the user asks for the full set.
4. Confirm `dotmatrix-loader.css` is imported by global CSS.
5. Use the loader near the pending action with `aria-label` or visible status text.
6. Run the app and verify the loader renders, animates, and does not shift layout.
