# seanmalone.com — Portfolio Rebuild Plan

> **Status:** v0.3 — living document. This is the source of truth for the project vision and decisions.
> Because Claude Code sessions are ephemeral (no memory between sessions), this file is how we preserve
> alignment. Update it as decisions are made. Anyone — or any future session — should be able to read this
> file and know exactly where the project stands.

_Last updated: 2026-09-14_

---

## 0. Next session — start here

Claude Code sessions have no memory of each other. If you are a new session, do this first:

1. **Read this whole file.** It is the source of truth for the project.
2. **Check §4 (Open questions).** Those are the blockers for starting design/build.
3. §3 holds a full inventory of the **current Format site**, captured from screenshots — including
   the end-to-end user flow in §3.10. Read it before proposing any structure.
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

Captured 2026-09-14 from screenshots Sean provided. **Pages captured:** Home, category/portfolio
page, booking page, Contact, Kudos, About, FAQs — i.e. the complete public site.

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

### 3.5 Services / categories implied by current content
From the FAQ copy: families · weddings · portraits/headshots · children · **senior portraits** ·
**events** · **commercial/corporate** (product and non-product).
From the homepage grid: **Portrait**, **Wedding**, **interiors/real-estate**, plus landscape and
detail tiles below the fold. Corporate clients shown: Yelp, Cibo, Unity, Paper Culture.
**Full category list still needed, and Sean is pruning some (§4.2).**

### 3.6 Design observations (current Format site)
- Essentially a **default Format template**: white ground, dark-gray humanist sans, generous whitespace.
- **Inconsistent hierarchy across pages** — Contact is small and left-aligned; FAQs are large and centered;
  About is a two-column image/text split. No unifying type scale or rhythm.
- **Third-party chrome is visible**: repeated "Using Format" badges, Wufoo/SurveyMonkey branding.
- Footer: *"© Sean Malone / Photographer"*.
- **Takeaway:** the *writing* is genuinely good and personal; the *presentation* is generic and
  inconsistent. A clean redesign has a lot of room to add craft without touching the voice.

### 3.7 Homepage — category grid

- **Nav (only 3 items):** `Portfolios` · `Pricing & Info` · `About/Contact`, left-aligned.
  Centered serif wordmark **"Sean Malone, Photographer"**. Right: **Instagram** + **share** icons.
- **Body:** a full-bleed **3-column image grid** of category tiles, edge to edge, no page gutter.
- **Interaction:** hovering a tile reveals the **category name** overlaid in white italic serif
  (e.g. *Portrait*). Clicking opens that category's portfolio page.
- Visible tiles (row 1): **Portrait**, an **interior / real-estate** shot, a **wedding** couple.
  Row 2 (partially visible): a landscape/hillside, a dark bokeh frame, a garment/detail shot.
- Footer: "Using Format" badge.

### 3.8 Category page — e.g. `/portrait`

Two-column split: **text rail on the left, horizontally-scrolling gallery on the right.**

- Large sans heading (**"Portraits"**), then:
  - *"Swipe right for gallery —>"*
  - *"**Check dates & times** to book a session, and read 'how it works'."* (links to booking page)
  - *"Session are typically 60-90 minutes. We shoot on location in the San Francisco Bay Area
    (**contact me** for travel options)."*  ← typo: "Session are" → "Sessions are"
  - *"**Pricing:** $200 for the session + $300 per image you choose, or $2,000 for the entire
    gallery (**full detail**)."*
  - *"You'll get 50+ images to choose from. Print anywhere or order archival quality right from
    your gallery."*
- **Gallery:** full-height images that scroll **horizontally**; many more images off-screen right.
- **Rebuild note:** pricing is stated **per category**, and again on the booking page — a single
  source of truth in frontmatter/data would keep them in sync (§6).
- **Design fork:** the page has to *tell* users "Swipe right for gallery —>", which is evidence the
  affordance isn't self-evident. Horizontal scroll is natural on touch, often confusing on desktop.
  Decide: keep horizontal, switch to vertical justified/masonry + lightbox, or hybrid (§4.4).

### 3.9 Booking page — "Book a Session"

- Subtitle: *"On-Location San Francisco Bay Area Portraits"*.
- **Embedded SavvyCal widget**: Sean's avatar, **"Portrait Session"**, **1 hr 30 min**, location **TBD**,
  **$200.00**, blurb *"Order processed, high-res images a la carte, or the whole session, directly
  through your online gallery ($300ea or $2,000 for all)."* Calendar + Pacific Time zone selector.
  → **Payment is collected at booking.**
- **"How it works"** — a three-column Q&A block:
  - **Pricing?** — $200/person session fee. Images (high-res, processed/edited) **$300 ea**, or
    **$2,000** for the whole collection of selects (**30–60** of the ~hundred shot).
  - **How does it work?** — book via the form; give address/area or TBD. $200 confirms date & time;
    confirmation email is replyable. Sean arrives on/before time and scouts nearby settings.
    Hair/makeup ready.
  - **How long is a session?** — reserves **90 minutes**; can run longer or as short as 20 min.
  - **How many outfits?** — a "look" = outfit + setting. No limit; most plan **2–3**.
  - **After the session?** — edited and posted to the client's **online gallery within a week**;
    order digitals or prints directly.
  - **Schedule changes?** — reschedule via the confirmation-email link, **24+ hours** ahead.
    Cancelling inside 24 hours forfeits the session fee.
  - **Next step / Questions?** — points back to the calendar and the portrait portfolio.
    ← typo: "brows my portfolio" → "browse"; "A cancellations forfeits" → "A cancellation forfeits"
- **Rebuild note:** this "How it works" block **substantially duplicates** the separate
  *FAQs about Sessions* page (§3.4) — session length, what's included, digitals, prints. Consolidate
  to one source (§4.5).

### 3.10 User flow (as built today)

```
Homepage grid  →  hover reveals category  →  click
      ↓
Category page (/portrait)  →  text rail + horizontal gallery  →  click "Check dates & times"
      ↓
Book a Session  →  SavvyCal embed (schedule + $200 payment)  →  confirmation email
      ↓
Client online gallery (Pixieset)  →  order digitals / prints
```

## 4. Open questions (need Sean's input)

1. **Full category list** — what are all the homepage grid categories, in order?
2. **Service pruning** — which categories/services are being **dropped** from the new site?
   (Cascades into pricing, FAQ, and the homepage grid.)
3. **Scale** — how many categories, total image count, and size of the largest gallery?
   (Drives the image pipeline and whether pagination/lazy-loading is needed.)
4. **Gallery interaction** — keep the **horizontal-scroll** gallery, move to a vertical
   justified/masonry grid with lightbox, or a hybrid? (See §3.8.)
5. **FAQ consolidation** — "How it works" (booking page) and "FAQs about Sessions" overlap heavily.
   Merge into one source? What stays on-site vs. links out to Pixieset?
6. **Pricing structure** — is pricing uniform across categories, or per-category? (Portraits is
   $200 + $300/image or $2,000. Do weddings/commercial differ?)
7. **Social proof placement** — testimonials and the Yelp/Cibo/Unity/Paper Culture logos are strong
   but currently buried off-nav. Promote them (homepage strip? per-category?)?
8. **Newsletter** — keep the opt-in? If so, which provider?
9. **Design vision** — desired feeling (minimal / editorial / dramatic / warm / moody) + 1–3
   reference sites. _(Sean to gather inspiration.)_
10. **Brand elements** — the current serif wordmark and serif/sans pairing: carry over or redesign?

## 5. Information architecture

Current nav is deliberately lean — **Portfolios · Pricing & Info · About/Contact** — and that
restraint is worth preserving. Draft structure for the rebuild:

- **Home** — full-bleed category grid with hover labels (keep the concept; §3.7).
- **Portfolios** → one page per category — gallery + per-category session info and pricing (§3.8).
- **Pricing & Info** — packages, "how it works", session FAQ (consolidated per §4.5).
- **Book** — SavvyCal embed, reachable from every category page.
- **About/Contact** — bio (strong existing copy) + native form, phone, email.
- **Kudos** — testimonials + corporate logos. *Consider promoting into Home and/or category pages
  rather than leaving it off-nav (§4.7).*

## 6. Content model (draft)

- Each **gallery** = a Hugo **page bundle**: a folder containing its images + an `index.md` with title,
  description, cover image, and ordering. Adding a shoot = drop images in a folder, edit a few frontmatter
  lines, commit.
- **Testimonials** and **corporate client logos** = data files (repeatable, reorderable).
- **Pricing tiers** = data file or structured frontmatter. Critically, **each category owns its
  pricing once** and it renders in every place that quotes it (category page, Pricing & Info,
  booking blurb) — today those are hand-synced copies that can drift.
- **Session FAQ / "how it works"** = one data file rendered wherever needed, not duplicated prose.
- **About / FAQ** = simple markdown pages.
- Hugo generates all responsive/optimized image variants at build time from originals.

## 7. Migration notes

- **Images** come from Sean's **original exports (Lightroom / Capture One / archive drives)** — NOT from
  Format's compressed copies. Cleaner source; Hugo does the optimizing.
- **Text** (About, Pricing, Kudos, FAQ copy) — transcribe from the current site; §3 already captures much
  of it.
- **Domain** — `seanmalone.com` registered at **GoDaddy**. Plan: external DNS pointing at the host; keep
  all existing records; change only apex A + `www` CNAME.
- **Third-party dependencies to retire:** Wufoo (contact form), Format (hosting/branding).
- **Third-party dependencies to keep:** **SavvyCal** (booking + payment, embedded),
  **Pixieset** (client galleries, ordering, fulfilment), **Instagram** (header link).
- **Copy fixes to make during transcription:** "Session are typically" → "Sessions are";
  "brows my portfolio" → "browse"; "A cancellations forfeits" → "A cancellation forfeits";
  "extending into a high school a business" (missing word); "a family photos on a wall".

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

- **2026-09-14 — v0.3** — Added the core user flow from screenshots: homepage category grid (§3.7),
  category page with horizontal gallery and inline pricing (§3.8), SavvyCal booking page and
  "How it works" (§3.9), and the end-to-end flow diagram (§3.10). Identified duplication between the
  booking page and the FAQ page, and pricing stated in multiple places — both become single-source
  in the content model. Recorded real pricing ($200 session + $300/image or $2,000). Rewrote IA
  around the actual three-item nav.
- **2026-09-14 — v0.2** — Added §3 current-site inventory from screenshots (Contact, Kudos, About, FAQs):
  services, testimonials, corporate clients, locations, design observations. Expanded open questions
  (service pruning, prints/ordering scope, newsletter). Added FAQ to the IA.
- **2026-09-13 — v0.1** — Initial plan. Locked: Hugo, git-native editing, no on-site client auth (Pixieset),
  Netlify + external DNS recommended. Open: galleries/genres, scale, pricing structure, design vision, brand.
