# seanmalone.com — Portfolio Rebuild Plan

> **Status:** v0.2 — living document. This is the source of truth for the project vision and decisions.
> Because Claude Code sessions are ephemeral (no memory between sessions), this file is how we preserve
> alignment. Update it as decisions are made. Anyone — or any future session — should be able to read this
> file and know exactly where the project stands.

_Last updated: 2026-09-14_

---

## 0. Next session — start here

Claude Code sessions have no memory of each other. If you are a new session, do this first:

1. **Read this whole file.** It is the source of truth for the project.
2. **Check §4 (Open questions).** Those are the blockers for starting design/build.
3. §3 holds the inventory of the **current Format site**, captured from screenshots Sean provided.
   Pages still uncaptured: **Home, Portfolio/galleries, Pricing.**
4. If you need to fetch `https://seanmalone.com` directly and it is blocked by the network egress proxy,
   the environment allowlist has not been updated — see §7.1. Ask Sean rather than guessing.

**Immediate goal:** answer §4 so the design/build phase can start.

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

## 3. Current site inventory

Captured 2026-09-14 from screenshots Sean provided. **Pages captured:** Contact, Kudos, About, FAQs.
**Still needed:** Home, Portfolio/galleries, Pricing.

> ⚠️ Sean has said **some current services will NOT carry over** to the new site. Do not assume the
> service list below is the target list — confirm first (§4.2).

### 3.1 Contact — "Get in touch"
- Phone **415-843-1311**; email link.
- Embedded **Wufoo** form (SurveyMonkey): First/Last name, Email (required), free-text
  "Questions, comments, whatever.", **newsletter opt-in checkbox**, "Send it!" submit.
- Protected by **reCAPTCHA Enterprise**.
- **Rebuild note:** replace Wufoo with a static-friendly native form (Netlify Forms) + spam protection.
  Removes third-party branding and a dependency. Decide whether to keep the newsletter opt-in (§4.6).

### 3.2 Kudos — testimonials
- Heading: *"Kind words from some of my clients…"*
- Layout: two-column pairs of **client photo + quote**.
- Named testimonials: **Nicole, Jennifer & Steve, Scott, Monique, Lynnea, Kelly, Rebecca**.
- Footer block: *"…a few corporate clients…"* with logo row: **Yelp, Cibo, Unity, Paper Culture**.
- **Rebuild note:** model testimonials as a data file (quote, attribution, image) and corporate logos as a
  separate data list — both become trivially reorderable/extendable.

### 3.3 About — "Thanks, Mom and Dad!"
- Personal origin story: dad's **Canon AE-1**, an enlarger and bulk film; makeshift darkroom in the laundry
  room; family farm in the **Sierra Nevada Foothills**; early childhood in **Zaire**. Mom's creativity,
  home-building, painting, gardening, farm animals — framed as the source of wonder/optimism.
- *"Beyond photography"* — Sean works in **web software / product design & management** at startups and
  public companies; links to **LinkedIn**.
- Contact repeated: 415-843-1311, email.
- Two personal photos: family on a Land Rover (vintage); Sean with daughter on his shoulders at a
  Christmas tree farm.
- **Rebuild note:** this copy is strong and personal — a real asset. Worth keeping largely intact and
  giving it better typographic treatment.

### 3.4 FAQs about Sessions
- **What's included in a typical session:** pre-session discussion (locations, looks, what to wear);
  post-session editing & processing on all images; online gallery for viewing/sharing/ordering; simple
  online ordering; everything à la carte; package pricing on digital downloads also available.
- **Digitals:** varies by plan. **Event, Senior Portraits, and commercial (non-product)** typically
  include digitals. Otherwise priced individually or as a discounted whole-session package.
- **Locations:** pricing applies to the **San Francisco Bay Area**; frequently also **Sacramento**,
  **Sierra Nevada Foothills**, and **Tahoe**; occasionally **Seattle, WA**; shoots abroad on request.
- **Prints/framing:** ordering through the proofing gallery (pro lab, 75+ years); ready-framed prints with
  frame/matting/glass options and a "Frame It" cart flow; a short argument for printing at all.
- **Rebuild note:** much of this presumes the **proofing-gallery ordering flow**, which now lives in
  Pixieset. Decide what stays as on-site copy vs. what links out (§4.5).

### 3.5 Services implied by current content
Families · weddings · portraits/headshots · children · **senior portraits** · **events** ·
**commercial/corporate** (product and non-product). Corporate clients shown: Yelp, Cibo, Unity,
Paper Culture. — **To be pruned by Sean (§4.2).**

### 3.6 Design observations (current Format site)
- Essentially a **default Format template**: white ground, dark-gray humanist sans, generous whitespace.
- **Inconsistent hierarchy across pages** — Contact is small and left-aligned; FAQs are large and centered;
  About is a two-column image/text split. No unifying type scale or rhythm.
- **Third-party chrome is visible**: repeated "Using Format" badges, Wufoo/SurveyMonkey branding.
- Footer: *"© Sean Malone / Photographer"*.
- **Takeaway:** the *writing* is genuinely good and personal; the *presentation* is generic and
  inconsistent. A clean redesign has a lot of room to add craft without touching the voice.

## 4. Open questions (need Sean's input)

1. **Portfolio structure** — screenshots of **Home, Portfolio/galleries, and Pricing** are still needed.
   What are the gallery groupings and their names?
2. **Service pruning** — *which current services are being dropped* from the new site? (See §3.5.)
3. **Scale** — roughly how many galleries, total image count, and size of the largest gallery?
   (Drives image pipeline + whether pagination/lazy-loading is needed.)
4. **Design vision** — desired feeling (minimal / editorial / dramatic / warm / moody) + 1–3 reference
   sites Sean admires. _(Sean to gather inspiration.)_
5. **Prints & ordering** — how much of the FAQ print/framing/ordering content stays on-site vs. links
   out to Pixieset?
6. **Newsletter** — keep the opt-in? If so, which provider (drives the form integration)?
7. **Brand elements** — existing logo, typeface, colors to carry over? Or design fresh?

## 5. Information architecture (draft)

- **Home** — hero image(s), a curated selection, clear entry into galleries + a CTA to inquire.
- **Portfolio / Work** — the galleries (structure TBD pending §4.1).
- **Pricing** — services & packages (TBD pending §4.1–4.2).
- **FAQ** — session expectations (trimmed per §4.5).
- **Kudos** — testimonials + corporate client logos.
- **About** — bio + artist statement (strong existing copy).
- **Contact / Inquiry** — native form (Netlify Forms), phone, email.

## 6. Content model (draft)

- Each **gallery** = a Hugo **page bundle**: a folder containing its images + an `index.md` with title,
  description, cover image, and ordering. Adding a shoot = drop images in a folder, edit a few frontmatter
  lines, commit.
- **Testimonials** and **corporate client logos** = data files (repeatable, reorderable).
- **Pricing tiers** = data file or structured frontmatter, so packages render consistently.
- **About / FAQ** = simple markdown pages.
- Hugo generates all responsive/optimized image variants at build time from originals.

## 7. Migration notes

- **Images** come from Sean's **original exports (Lightroom / Capture One / archive drives)** — NOT from
  Format's compressed copies. Cleaner source; Hugo does the optimizing.
- **Text** (About, Pricing, Kudos, FAQ copy) — transcribe from the current site; §3 already captures much
  of it.
- **Domain** — `seanmalone.com` registered at **GoDaddy**. Plan: external DNS pointing at the host; keep
  all existing records; change only apex A + `www` CNAME.
- **Third-party dependencies to retire:** Wufoo (form), Format (hosting/branding).

### 7.1 Network egress / allowlist

The Claude Code environment's **network access policy** blocks general web traffic by default (only package
registries + Anthropic APIs). It is an **environment-level setting**, configured in the Claude Code web UI,
read once at container startup — it cannot be changed from inside a running session, and requires starting a
**new session** to take effect.

To let Claude review the current site directly, set **Network access → custom allowlist** (not full access):

```
seanmalone.com
www.seanmalone.com
*.format.com
```

- `seanmalone.com` — page structure, nav, gallery names, copy.
- `*.format.com` — the CDN serving the Format site's CSS/JS/images; needed for rendered **screenshots**.
  Exact Format asset hostnames are unconfirmed; if requests are still blocked, check the proxy failures
  (`curl -sS "$HTTPS_PROXY/__agentproxy/status"`) and add the missing domains.

Later, during the build phase, also consider allowlisting `fonts.googleapis.com` and `fonts.gstatic.com`
for web fonts.

**Note:** even with egress opened, Claude **cannot reach the Format admin account** — that requires Sean's
login, which lives in his browser, not the container. Anything behind the Format login must be exported
by Sean. Screenshots pasted into a session work fine as a substitute.

## 8. Explicitly out of scope / separate projects

- **Zenfolio → Pixieset client-image & records migration** — a *separate* effort with a pre-**March**
  deadline (account renewal). Tracked separately; not part of this portfolio rebuild.

## 9. Milestones (draft — to be refined)

1. **Align** — finish this plan (answer §4), lock design direction. ← _we are here_
2. **Scaffold** — strip Kaldi layouts; set up base Hugo structure, image pipeline, and one sample gallery.
3. **Design system** — typography, color, spacing, gallery + lightbox components (image-forward).
4. **Build pages** — Home, Portfolio, Pricing, FAQ, Kudos, About, Contact.
5. **Content migration** — real galleries + images + copy.
6. **Deploy** — Netlify (external GoDaddy DNS), staging on a `*.netlify.app` URL first.
7. **Cutover** — point `seanmalone.com` at the new host once approved.
8. **(Optional, later)** — add Sveltia CMS.

## 10. Changelog

- **2026-09-14 — v0.2** — Added §3 current-site inventory from screenshots (Contact, Kudos, About, FAQs):
  services, testimonials, corporate clients, locations, design observations. Expanded open questions
  (service pruning, prints/ordering scope, newsletter). Added FAQ to the IA.
- **2026-09-13 — v0.1** — Initial plan. Locked: Hugo, git-native editing, no on-site client auth (Pixieset),
  Netlify + external DNS recommended. Open: galleries/genres, scale, pricing structure, design vision, brand.
