# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
pnpm dev          # dev server at http://localhost:3000
pnpm build        # production build (Cloudflare Pages)
pnpm preview      # preview production build locally
pnpm lint         # ESLint
pnpm typecheck    # vue-tsc type checking
```

CI runs `lint` then `typecheck` on every push — both must pass before merging.

## Architecture

This is a **Nuxt 4** SPA (`ssr: false`). All application code lives under the `app/` directory (not the project root). Nuxt auto-imports from `app/components/`, `app/composables/`, `app/utils/`, and `app/pages/`.

**Key modules and their roles:**
- **@nuxt/ui v4** — component library built on Tailwind CSS v4. Colors configured in `app/app.config.ts` (primary: blue, neutral: slate); global component size default set to `lg` in `nuxt.config.ts`.
- **@pinia/nuxt + @pinia/colada-nuxt** — Pinia for state, Pinia Colada (`useQuery`/`useMutation`) for async data fetching.
- **@vueuse/nuxt** — VueUse composables, auto-imported.
- **nitro-cloudflare-dev** — enables Cloudflare bindings (KV, D1, etc.) during local dev.
- **Nitro preset: cloudflare-pages** — deploys as a Cloudflare Pages SPA. The home route (`/`) is prerendered.

**Page title template:** `%s - Nuxt SPA` (set in `nuxt.config.ts` `app.head`). Use `useHead({ title: '...' })` or `useSeoMeta` in pages; `app.vue` sets the default SEO meta.

**Layout structure** (`app/app.vue`):
```
UApp
  UHeader      ← logo link (left), color mode toggle + GitHub link (right)
  UMain
    NuxtPage
  USeparator   ← icon="i-simple-icons-nuxtdotjs"
  UFooter      ← copyright text (left), GitHub link (right)
```

**Icons:** Two Iconify sets — `lucide` and `simple-icons`. Use via `<UIcon name="i-lucide-..." />` or `<UIcon name="i-simple-icons-..." />`.

## CSS & Theming

`app/assets/css/main.css` extends Tailwind v4 and Nuxt UI with:
- Font: `--font-sans: "Google Sans"` (must be loaded externally if needed)
- Container width: `--ui-container: 90rem`
- Custom size token: `--text-2xs: 0.625rem`
- Body background: `var(--ui-bg-muted)`
- Thin scrollbars (4px) styled with neutral palette tokens
- Heading utility classes: `.heading-1` → `text-4xl`, `.heading-2` → `text-3xl`, `.heading-3` → `text-2xl`, `.heading-4` → `text-xl` (all bold)

## Nuxt Reference

- https://nuxt.com/llms.txt — Nuxt condensed LLM reference

## Nuxt UI Reference

- https://ui.nuxt.com/llms.txt — Nuxt UI condensed LLM reference
- https://ui.nuxt.com/llms-full.txt — Nuxt UI full LLM reference

## ESLint Style Rules

The project uses `@nuxt/eslint` with stylistic rules configured in `nuxt.config.ts`:
- No trailing commas (`commaDangle: 'never'`)
- 1TBS brace style (`braceStyle: '1tbs'`)
- 2-space indentation, LF line endings (via `.editorconfig`)
