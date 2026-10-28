# davidiandata.com

The directory component of the former AI David Koresh site
([aidavidkoresh](https://github.com/hack-r/aidavidkoresh), aikoresh.com),
rebuilt as a plain data directory. The AI/chat novelty is gone; what remains
is the curated Waco / Branch Davidian resource directory, ready to be
supplemented with additional open data that looks like it wants to be
reposted.

## What's here

- `index.html` — the site. Static, no build step, no framework, no AI.
- `data/directory.json` — the directory as structured data. This is the
  source of truth; the page renders from it. Adding an entry means adding it
  here.
- Icons and web manifest at the repo root.

## Directory categories

Featured · Government Reports & Documents · Archives & Collections ·
Encyclopedias & Reference · News & Journalism · Films & Media · Books ·
Community · Related Sites

Entries carry `title`, `url`, and `tags`, so the same JSON can be loaded into
Notion or MongoDB without transformation (see "Export targets" below).

## Deploy (Cloudflare)

The domain's zone is on Cloudflare. Two options:

**Cloudflare Pages** (simplest): create a Pages project connected to this
repo, or `npx wrangler pages deploy .` directly.

- Build command: none
- Output directory: /

The site is fully static; `data/directory.json` is fetched client-side.

## 301 redirect from aikoresh.com

Once both zones are on the same Cloudflare account, add a Redirect Rule (or
Bulk Redirect) on the aikoresh.com zone:

- If incoming requests match: `Hostname eq aikoresh.com`
- Then: Dynamic redirect to
  `concat("https://davidiandata.com", url.path)` with status `301`,
  preserving query strings.

## Export targets

The directory JSON is deliberately schema-friendly for:

- **Notion** — one database, properties: `Title` (title), `URL` (url),
  `Category` (select), `Tags` (multi-select).
- **MongoDB** — one collection (`directory`), one document per entry, plus a
  category collection, or just insert `data/directory.json` as-is.

## Knowledge base note

The original ChatKit workflow's knowledge base (file search) can be kept
without any LLM chat, two ways:

1. Call the OpenAI vector store search endpoint
   (`POST /v1/vector_stores/{id}/search`) behind a small `/api/search`
   worker — returns matching source chunks, no generation, minimal cost.
2. Export the knowledge base files to this repo (e.g. `data/kb/`) and index
   them statically — zero recurring cost, fully open data.

## Supplementing

The repo's standing mission: add any additional open source data that looks
like it wants to be reposted. Add entries to `data/directory.json`, or add
whole new datasets under `data/`.
