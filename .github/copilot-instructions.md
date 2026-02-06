<!-- .github/copilot-instructions.md - guidance for AI coding agents working on this repo -->
# Astro News — AI Assistant Instructions

Purpose: Help AI coding agents make precise, low-risk changes in this Astro + Keystatic site.

- Project type: Static site built with Astro (v5.x) with optional Keystatic CMS integration.
- Primary runtimes/tools referenced in docs: Bun (README) and Node/npm scripts in package.json.

Quick pointers
- Start dev server: use the package scripts in package.json (`dev`, `build`, `preview`); e.g. `npm run dev` or `bun dev` per README.
- Keystatic is opt-in at runtime via environment variable `RUN_KEYSTATIC=true`. See `astro.config.mjs`.
- Content is stored under `src/content/*` and loaded via `src/content.config.ts` (Astro Content Collections + MDX).

Architecture & data flow
- Content authoring: files in `src/content/articles`, `src/content/authors`, `src/content/categories` → collected by `src/content.config.ts` → consumed by pages under `src/pages/articles` and layouts in `src/layouts`.
- Keystatic CMS: configured in `keystatic.config.ts` and the project's Keystatic collections are defined in `src/lib/keystatic` (imported by `keystatic.config.ts`). Keystatic is added to Astro integrations only when `RUN_KEYSTATIC` is truthy.
- Build integrations: `astro.config.mjs` registers MDX, sitemap, pagefind (search), and conditionally `@keystatic/astro` + React when Keystatic is enabled.

Key files to inspect before edits
- Config & scripts: package.json, astro.config.mjs, keystatic.config.ts
- Content schema & loaders: src/content.config.ts, src/lib/schema (schema definitions)
- Keystatic helpers: src/lib/keystatic (collection builders and mappings)
- Utilities: src/lib/utils (e.g. remark plugins: readingTime, modifiedTime)
- Pages & components: src/pages, src/layouts, src/components

Project-specific conventions
- MDX article files: `src/content/articles/**/*.mdx` (loader pattern in `src/content.config.ts`).
- Author pages expect `index.mdx` under `src/content/authors/*` (loader uses `**/index.mdx`).
- Categories use `index.json` under `src/content/categories/*` (loader uses `**/index.json`).
- Keystatic only runs in dev when `RUN_KEYSTATIC=true`; guards appear in `astro.config.mjs` — follow that pattern when adding optional integrations.

Editing guidance & examples
- To add a new article: create an `.mdx` file anywhere under `src/content/articles/` — metadata validated against the schema in `src/lib/schema`.
- To add a new author: add `src/content/authors/<slug>/index.mdx` and ensure the schema fields match `authorSchema`.
- To add a category: create `src/content/categories/<slug>/index.json` matching `categorySchema`.
- When adding a Keystatic collection, mirror keys between `src/lib/keystatic` and `keystatic.config.ts` and keep `collections` consistent with `src/content.config.ts`.

Build & dev notes
- Use the package scripts from `package.json` to run `astro dev`, `astro build`, and `astro preview`.
- README references Bun (`bun install` / `bun dev`) — confirm locally which runtime team prefers before making environment-specific changes.

Search, sitemap, and RSS
- Search is provided by `astro-pagefind`/`pagefind` and configured in `astro.config.mjs` and `src/pages/search`.
- RSS and sitemap integrations exist (see `@astrojs/rss` and `@astrojs/sitemap` in package.json and `src/pages/rss.xml.js`).

Safe-change checklist for PRs
- Run `npm run dev` and spot-check core pages: home, article list, article, author, category.
- If changing content schema: update `src/lib/schema` and `src/content.config.ts` loaders; run type-check/build.
- When touching Keystatic: ensure `RUN_KEYSTATIC` gating remains and verify `http://localhost:4321/keystatic` in dev (README note).

If unsure
- Open these files first: package.json, astro.config.mjs, keystatic.config.ts, src/content.config.ts, src/lib/schema, src/pages.
- Ask for clarification if README and package.json disagree about the preferred runtime (Bun vs npm/pnpm).

Questions or missing details? Ask the repo owner for preferred local runtime and any deployment hooks.
