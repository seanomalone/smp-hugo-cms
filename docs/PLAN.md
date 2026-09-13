# seanmalone.com — Portfolio Rebuild Plan

> **Status:** v0.1 — living document. This is the source of truth for the project vision and decisions.
> Because Claude Code sessions are ephemeral (no memory between sessions), this file is how we preserve
> alignment. Update it as decisions are made. Anyone — or any future session — should be able to read this
> file and know exactly where the project stands.

_Last updated: 2026-09-13_

---

## 0. Next session — start here

Claude Code sessions have no memory of each other. If you are a new session, do this first:

1. **Read this whole file.** It is the source of truth for the project.
2. **Review the current site.** Fetch `https://seanmalone.com` and build a **content inventory**:
   navigation structure, gallery names and groupings, services/pricing, testimonials, About copy.
   Write the findings into §3 (Open questions) and §4 (Information architecture) and commit.
3. **If the fetch is blocked** by the network egress proxy, the environment's allowlist has not been
   updated yet — see §6.1. Ask Sean rather than guessing.

**Immediate goal:** answer the open questions in §3 so the design/build phase can start.

---

## 1. Goal

Replace Sean Malone's photography portfolio site — currently on **Format** (format.com), served at
**seanmalone.com** — with a self-hosted, image-forward site built on **Hugo**, managed git-natively,
and deployed to a static host. This is a **clean redesign**, not a replica of the current Format site.

The site is a **working photographer's business site**, not just an art gallery: portfolio + services
+ social proof + inquiry funnel.

## 2. Decisions locked

| Area | Decision | Notes |
|------|----------|-------|
| **Static site generator** | **Hugo** (keep the existing repo's engine) | Best-in-class built-in image pipeline (responsive `srcset`, webp/avif, build-time resizing), fast builds for image-heavy sites, no JS runtime. |
| **Base template** | **Discard** the "Kaldi" business-template layouts | Keep Hugo as the engine; build image-forward gallery/lightbox layouts from scratch. |
| **Content editing** | **Git-native** (folders + markdown), Claude Code as the "CMS" | Sean is technical and lives in Claude Code. A gallery = a folder of images + small metadata file (Hugo page bundles). |
| **Visual CMS** | **Deferred** — add **Sveltia CMS** later if wanted | Modern Decap successor with better image UX. Only if mobile/non-technical posting is ever needed. NOT Decap. |
| **Client galleries / proofing** | **Out of scope** | Sean uses **Pixieset** for client delivery. The new site links out to it; no auth on this site. |
| **Hosting (recommended)** | **Netlify**, using **external DNS** (keep GoDaddy) | Change only 2 records at GoDaddy (apex A + `www` CNAME); leave email/MX/TXT untouched. No nameserver move. |

### Hosting rationale (pending final confirmation)
- **Netlify + external GoDaddy DNS** avoids the nameserver move Sean wants to avoid. Free tier (100GB/mo
  bandwidth) is typically ample for a starting portfolio. Keeps the door open for Sveltia/Decap CMS auth.
- **Cloudflare Pages** (better long-term bandwidth/CDN/image tooling) requires pointing the apex domain's
  **nameservers** to Cloudflare. Cloudflare *auto-imports* existing DNS records (review before switching).
  Revisit only if bandwidth cost becomes a real problem — migration is easy later.

## 3. Open questions (need Sean's input)

1. **Galleries / genres** — what work does the portfolio show, and what are the natural gallery groupings?
   (portraits, weddings, landscape, travel, commercial, editorial, …?)
2. **Scale** — roughly how many galleries, total image count, and size of the largest gallery? (Drives
   image pipeline + whether pagination/lazy-loading is needed.)
3. **Services & pricing** — what services are sold, and is "Pricing" one page or several (per service type)?
4. **Design vision** — desired feeling (minimal / editorial / dramatic / warm / moody) + 1–3 reference
   sites Sean admires. _(Sean to gather inspiration.)_
5. **Brand elements** — existing logo, typeface, colors to carry over? Or design fresh?

## 4. Information architecture (draft)

- **Home** — hero image(s), a curated selection, clear entry into galleries + a CTA to inquire.
- **Portfolio / Work** — the galleries (structure TBD pending Q1).
- **Pricing** — services & packages (one page or several, TBD pending Q3).
- **Kudos** — testimonials / client praise.
- **About** — bio + artist statement.
- **Contact / Inquiry** — inquiry form (static-friendly: Netlify Forms or a form service).

## 5. Content model (draft)

- Each **gallery** = a Hugo **page bundle**: a folder containing its images + an `index.md` with title,
  description, cover image, and ordering. Adding a shoot = drop images in a folder, edit a few frontmatter
  lines, commit.
- **Pricing / Kudos / About** = simple markdown pages (or data files for repeatable items like testimonials
  and pricing tiers).
- Hugo generates all responsive/optimized image variants at build time from originals.

## 6. Migration notes

- **Images** come from Sean's **original exports (Lightroom / Capture One / archive drives)** — NOT from
  Format's compressed copies. Cleaner source; Hugo does the optimizing.
- **Text** (About, Pricing, Kudos copy) — copy-paste from the current Format site.
- **Domain** — `seanmalone.com` registered at **GoDaddy**. Plan: external DNS pointing at the host; keep
  all existing records; change only apex A + `www` CNAME.
### 6.1 Network egress / allowlist

The Claude Code environment's **network access policy** blocks general web traffic by default (only package
registries + Anthropic APIs). It is an **environment-level setting**, configured in the Claude Code web UI,
read once at container startup — it cannot be changed from inside a running session, and requires starting a
**new session** to take effect.

To let Claude review the current site, set **Network access → custom allowlist** (not full access) with:

```
seanmalone.com
www.seanmalone.com
*.format.com
```

- `seanmalone.com` — page structure, nav, gallery names, copy (enough for a content inventory).
- `*.format.com` — the CDN serving the Format site's CSS/JS/images; needed for rendered **screenshots**.
  Exact Format asset hostnames are unconfirmed; if requests are still blocked, check the proxy failures
  (`curl -sS "$HTTPS_PROXY/__agentproxy/status"`) and add the missing domains.

Later, during the build phase, also consider allowlisting `fonts.googleapis.com` and `fonts.gstatic.com`
for web fonts.

**Note:** even with egress opened, Claude **cannot reach the Format admin account** — that requires Sean's
login, which lives in his browser, not the container. Anything behind the Format login must be exported
by Sean.

## 7. Explicitly out of scope / separate projects

- **Zenfolio → Pixieset client-image & records migration** — a *separate* effort with a pre-**March**
  deadline (account renewal). Tracked separately; not part of this portfolio rebuild.

## 8. Milestones (draft — to be refined)

1. **Align** — finish this plan (answer §3), lock design direction. ← _we are here_
2. **Scaffold** — strip Kaldi layouts; set up base Hugo structure, image pipeline, and one sample gallery.
3. **Design system** — typography, color, spacing, gallery + lightbox components (image-forward).
4. **Build pages** — Home, Portfolio, Pricing, Kudos, About, Contact.
5. **Content migration** — real galleries + images + copy.
6. **Deploy** — Netlify (external GoDaddy DNS), staging on a `*.netlify.app` URL first.
7. **Cutover** — point `seanmalone.com` at the new host once approved.
8. **(Optional, later)** — add Sveltia CMS.

## 9. Changelog

- **2026-09-13 — v0.1** — Initial plan. Locked: Hugo, git-native editing, no on-site client auth (Pixieset),
  Netlify + external DNS recommended. Open: galleries/genres, scale, pricing structure, design vision, brand.
