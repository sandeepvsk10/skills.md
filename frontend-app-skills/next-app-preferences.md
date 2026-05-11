# Next.js app preferences — general portfolio

Conventions for this repository’s App Router layout, folders, and where code belongs. Agents and contributors should follow this alongside `AGENTS.md`.

---

## Stack (reference)

- **Next.js** (App Router), **React**, **TypeScript**
- **Tailwind CSS v4** — theme tokens in `src/app/globals.css` (`@theme inline`, shadcn imports)
- **shadcn/ui** (`components.json`, Base UI–style components under `src/components/ui/`)
- **Fonts:** Inter + Geist Mono via `next/font/google` in `src/app/layout.tsx`

---

## Top-level `src/` layout

| Area | Path | Purpose |
|------|------|---------|
| App routes | `src/app/` | Routes, nested layouts, route-level `components/` and `constants/` |
| Shared UI | `src/components/` | Site-wide components (`site-nav`, `optional-site-nav`, `ui/`, `core/`) |
| Shared constants | `src/constants/` | Cross-route constants (e.g. `site-nav-links.ts`) |
| Utilities | `src/lib/` | Helpers (e.g. `utils.ts` for `cn()`) |

---

## App Router structure (`src/app/`)

### Root

- **`layout.tsx`** — Root layout: fonts, `globals.css`, `<body>`, **`OptionalSiteNav`** (main nav hidden on `/` so the home route can use a custom header).
- **`globals.css`** — Tailwind entry + design tokens; do not duplicate theme in random CSS files without reason.

### Route group: `(home)` — URL `/`

- **`(home)`** is a **route group** (parentheses = **no URL segment**).
- **`(home)/layout.tsx`** — Home-only shell: max width **1280px**, horizontal padding, **`HomeHeader`** then **`{children}`** then **`HomeFooter`**.
- **`(home)/page.tsx`** — Home page.
- **`(home)/components/`** — Components used only by the home route (e.g. `HomeHeader`, `HomeFooter`, `HomeFooterNav`, `home-social-nav`, `home-content-split`).
- **`(home)/constants/`** — Home-only constants (e.g. `social-links.ts` for X / GitHub / LinkedIn).

### Section routes (no group)

Each top-level section is its own segment with a **nested layout** for that section’s chrome (header band, accent, copy):

| Route | Folder | Notes |
|-------|--------|--------|
| `/open-source` | `src/app/open-source/` | `layout.tsx` + `page.tsx` |
| `/neo4j` | `src/app/neo4j/` | same |
| `/python` | `src/app/python/` | same |

Per section, keep:

- **`components/`** — UI scoped to that route (colocated components).
- **`constants/`** — Constants scoped to that route (add modules as needed).

Use **relative imports** from the route folder (e.g. `./components/...`, `./constants/...`) or **`@/`** for shared code.

### Shared navigation data

- **`src/constants/site-nav-links.ts`** — Single source for primary nav links (**Home**, **Socials**, **Links**, **Others**). **`SiteNav`** and **`HomeFooterNav`** both consume this file.

---

## Component placement rules

1. **Route-specific** UI → under that route’s `components/` (e.g. `(home)/components/HomeHeader.tsx`).
2. **Reusable primitives / design system** → `src/components/ui/` (shadcn) or `src/components/core/` (e.g. `text-scramble`).
3. **Used on multiple routes but not global layout** → prefer a small module under `src/components/` with a clear name.
4. **Client vs server** — Use **`"use client"`** only where needed (hooks, browser APIs, interactive widgets). Keep route **`page.tsx` / `layout.tsx`** as Server Components when possible.

---

## URLs and layouts (mental model)

- **`/`** → `(home)` layout + page (custom header/footer; no default sticky **`SiteNav`**).
- **`/open-source`**, **`/neo4j`**, **`/python`** → each segment’s **`layout.tsx`** wraps **`page.tsx`**; global **`SiteNav`** still renders from root layout (except behavior tied to pathname).

---

## Files not listed exhaustively

Add new routes by mirroring the pattern: **`layout.tsx`** (optional), **`page.tsx`**, and local **`components/`** / **`constants/`** as the route grows.
