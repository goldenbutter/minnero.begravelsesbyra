# BUILD_PROGRESS — Minnero Begravelsesbyrå demo

> **Append-only build log.** Every commit gets one entry. Newest entries at the **top** of the build log — easier to scan when scrolling in. Lives at `_docs/BUILD_PROGRESS.md` (TRACKED in git, family-wide convention as of 2026-05-02). Originally at `.claude/BUILD_PROGRESS.md` (gitignored, local-only) — migrated to `_docs/` so the log is recoverable from any clone and visible on GitHub. The `.claude/` directory stays gitignored.
>
> **Brand history:** project was first delivered as "Lysning Begravelsesbyrå" but renamed to "Minnero Begravelsesbyrå" on 2026-05-01 (commit `6c4a340`). Reason: Bithun's interior-design business already uses Lysning. Older entries in this log reference `demo-lysning-*` paths — those were accurate at the time of writing; the current paths are `demo-minnero-classic/` and `demo-minnero-premium/`.

## Format per entry

```
## <commit short hash> — <one-line title>
**Date:** <YYYY-MM-DD>
**Tier(s) touched:** classic | premium | both | scaffolding
**Reason:** <why this change was made — usually quotes the user's review point>
**Files changed:** <bulleted list>
**Notes for future agents:** <gotchas, follow-ups, things deliberately left undone>
```

---

## Project context (read once)

- **Business:** Minnero Begravelsesbyrå (fictional Norwegian funeral home, *minne* + *ro* = peaceful memory)
- **Two tiers:** [demo-minnero-classic/](../demo-minnero-classic/) and [demo-minnero-premium/](../demo-minnero-premium/)
- **Stack:** vanilla HTML + CSS + JS, no build step
- **Architecture rules:** [_docs/_skill/ARCHITECTURE-DECISION-GUIDE.md](../../_docs/_skill/ARCHITECTURE-DECISION-GUIDE.md)
- **Image prompts:** [Nano-Banana-prompt.md](Nano-Banana-prompt.md)
- **Dev server:** `python -m http.server 5500` from project root
- **Public README:** [../README.md](../README.md) — customer-facing two-tier overview
- **GitHub repo:** `goldenbutter/minnero.begravelsesbyra` (pushed 2026-05-01)
- **Productised skill:** `C:\Project\skills\begravelsesbyra-skill\` — fork procedure for next funeral-home customer

## Tier-defining rules (locked, do not silently break)

These are the rules the user has corrected me on — DO NOT undo them without an explicit instruction. The full parity contract is captured in `C:\Project\skills\begravelsesbyra-skill\skills\begravelsesbyra-skill\SKILL.md` §5c, §10, §11.

| Rule | Tier | Why |
|---|---|---|
| Norwegian-only, no language toggle (HTML + JS init commented out, EN strings in JS untouched on classic) | classic | Customer pays extra for EN — i18n is upsell-locked |
| Static, no hover/fade/scroll animations | classic | Classic = static brochure feel |
| No visitor-badge view counter | classic | Premium-only feature; markup stays in HTML but commented |
| Same candle hero on both tiers (still on classic, animated `hero-candle.mp4` on premium) | both | Visual continuity |
| Mobile menu must NOT cover the whole page | both | The full-screen overlay was rejected |
| Classic mobile menu = 260px compact dropdown anchored top-right | classic | Classic signature |
| Premium mobile menu = 320px liquid-glass slide-in from right edge with `backdrop-filter: blur(28px)` | premium | Premium signature — never apply to classic |
| Hero candle positioning is empirically locked per device — never re-derive | both | Classic mobile `74% 30% scale(1.04)`; premium desktop `center 8%`; premium mobile `77% 65% scale(1.22)`. AI-generated mp4 has the candle at a slightly different X% than the still poster |
| Service cards (home) + memorial cards (minneside) float continuously with `card-float` keyframe; on hover, `animation-play-state: paused` + `border-color: var(--color-primary)` + deeper shadow. **No transform on hover.** | premium | Inspired by rameezscripts.com — the "alive but holding still while you interact" pattern |
| `.service-icon` SVGs bounce inside service cards with `icon-bounce` keyframe (3s, -5px) — staggered per nth-child | premium | Layered motion — card bobs at one rhythm, icon at another |
| `prefers-reduced-motion` fallback turns off `card-float`, `icon-bounce`, `.reveal`, `.fade-in`, `card-float-paused` states for both card types | premium | Accessibility |
| Photo strips (classic) use `.photo-strip` class with caption overlay; interstitials (premium) use `.interstitial` (with caption blockquote) or `.interstitial--photo` (without caption, brighter overlay) | both | Visual breath sections share content but diverge in animation: classic = always-visible; premium = IntersectionObserver fade-in |
| Premium-only minneside page exists; classic has no minneside | premium | Memorial wall + graveyard interstitial after wall are premium-tier features |
| Minneside graveyard interstitial sits AFTER the memorial wall, before the footer (not between intro and wall) | premium | User explicitly placed it there on 2026-05-01 review |
| Vipps modal locked phrase: "Dette er en demo — Vipps-betaling aktiveres når din konto er satt opp." | both | Family-wide rule |
| Footer credit "Utviklet av Bithun" | both | Family-wide rule |
| `lysning.no` must never appear anywhere — replaced with `minnero.no` on 2026-05-01 (commit `9791be5`) | both | Brand-rename leftovers |

## File tree map

```
begravelsesbyrå-demo/
├── README.md                     (customer-facing two-tier overview)
├── .gitignore
├── .claude/                      (gitignored, agent-only)
│   ├── BUILD_PROGRESS.md         (this file)
│   └── Nano-Banana-prompt.md
├── _public/                      (gitignored, source images from Nano Banana — Bithun drops .jpeg exports here)
├── demo-minnero-classic/
│   ├── index.html · tjenester.html · om-oss.html · kontakt.html
│   ├── favicon.svg
│   ├── css/style.css
│   ├── js/main.js
│   └── assets/images/            (12 jpgs)
└── demo-minnero-premium/
    ├── index.html · tjenester.html · om-oss.html · kontakt.html · minneside.html
    ├── favicon.svg
    ├── css/style.css
    ├── js/main.js
    ├── data/memorials.json       (memorial-wall data — 8 fictional names, replace per customer)
    └── assets/
        ├── images/               (13 jpgs — 12 shared + minneside-graveyard.jpg)
        └── videos/hero-candle.mp4
```

---

# Build log

(Newest entries at the **top**. Scrolling in shows the most recent state first.)

## 2026-05-02 — OG/Twitter Card requirement promoted to skill + family-wide
**Date:** 2026-05-02
**Tier(s) touched:** none (skill + architecture guide updates only — no prototype code changes, so no commit on this repo)
**Reason:** After the WhatsApp link-preview fix in `21635d1`, the customer-fork skill at `C:\Project\skills\begravelsesbyra-skill\` and the family-wide architecture guide both still treated OG tags as a one-line afterthought. Bithun asked for the lesson to be propagated so future agents — both for funeral-home customer forks and for entirely new business prototypes (the other 16 in the family) — don't reintroduce the same gap.
**Files changed (outside this repo):**
- `C:\Project\skills\begravelsesbyra-skill\skills\begravelsesbyra-skill\SKILL.md` — Step 5b's brief `og:image` paragraph now points to the canonical Step 8; **Step 8 fully rewritten** with the complete OG + Twitter Card template, per-page image-pick table (index→hero-candle, tjenester→banner-services, om-oss→banner-about, kontakt→banner-contact, premium minneside→memorial-banner), per-tier values (premium adds `og:locale:alternate en_US`), and the three rules — absolute URLs, every page, image:width/height matter; new verification step **L** in the A–K checklist with three concrete grep checks
- `C:\Project\prototypes\_docs\_skill\ARCHITECTURE-DECISION-GUIDE.md` — new bullet in the "Cross-cutting rules" section codifying OG/Twitter Card as family-wide policy with the same three rules and required-tags list (so when the next business prototype — bakeri, hårstudio, dyrlege etc. — gets built, the agent doesn't have to relearn this)
**Notes for future agents:**
- The lesson is now codified in three places: this BUILD_PROGRESS entry (project-historical), SKILL.md Step 8 (per-fork procedure), and the architecture guide (family-wide). Don't drop the rule from any of them — the redundancy is on purpose.
- WhatsApp's crawler silently fails on relative `og:image` URLs (no error, just an empty preview). That's why this slipped through initial review — there's no console warning to catch it. The grep verification in SKILL.md Step L is the only reliable check.

## 2026-05-02 — Vercel two-project monorepo step added to family-wide arch guide
**Date:** 2026-05-02
**Tier(s) touched:** none (architecture guide update only — no prototype code changes, so no commit on this repo)
**Reason:** Bithun checked his Vercel config and discovered the `git diff HEAD^ HEAD --quiet ./` Ignored Build Step was never actually applied — only described retrospectively in the previous BUILD_PROGRESS entry. Confirmed it wasn't on any forward-looking checklist; the rule existed in build-history but not in any "do this when starting a new project" instruction. Easy gap for the next prototype to fall into.
**Scope clarification (important):** the two-project monorepo pattern is **prototype-build only**. Customer forks are single-tier single-project (the customer buys ONE tier; not a monorepo) — they don't need the Ignored Build Step. So this rule belongs in the architecture guide (which governs prototype builds) and NOT in the customer-fork SKILL.md punch-list.
**Files changed (outside this repo):**
- `C:\Project\prototypes\_docs\_skill\ARCHITECTURE-DECISION-GUIDE.md` — new **Step 4b "Deploy the prototype to Vercel"** between Step 4 (Scaffold) and Step 5 (Write SKILL.md), explicitly framed as prototype-only with a callout that customer forks don't follow this pattern. The 10-step setup walks through: push to GitHub → import to Vercel → Framework Preset Other → Root Directory `demo-<business>-<tier>` → empty Build settings → first deploy → **Ignored Build Step `git diff HEAD^ HEAD --quiet ./`** (with the `./` scoping note) → Web Analytics toggle → custom domain. Verification block at the end describes how to confirm the Ignored Build Step actually works (touch only one tier, push, watch the other tier's build skip).
- `C:\Project\skills\begravelsesbyra-skill\skills\begravelsesbyra-skill\SKILL.md` — left unchanged on this point; customer-fork punch-list keeps its simple "Create Vercel project, import from GitHub, deploy" because a customer fork really is just one project.
**Notes for future agents:**
- If you're building a NEW prototype for any of the 16 unbuilt businesses (bakeri, hårstudio, etc.), follow Step 4b verbatim — don't skip the Ignored Build Step "to set up later". After the second commit, "later" means every push triggers two redundant builds and the Vercel build minutes drain.
- If you're forking the SOURCE prototype for a customer using `begravelsesbyra-skill`, the customer fork is a single-tier single-project deploy — Vercel setup is the simple one-project flow, not the two-project monorepo flow.

## 21635d1 — Add Open Graph + Twitter Card tags to all 9 pages
**Date:** 2026-05-02
**Tier(s) touched:** both
**Reason:** Bithun shared the production URLs in WhatsApp; link preview rendered as plain text — no image, no description. Root cause was twofold: (a) `og:image` on the two index pages was a relative path (`assets/images/hero.jpg`), and WhatsApp's crawler needs an absolute URL; (b) the four sub-pages on classic and four on premium had no OG/Twitter tags at all. Bonus fix: the relative `hero.jpg` reference was also stale — that file was retired in `ef64f99` when the candle hero replaced the misty fjord.
**Files changed:**
- `demo-minnero-classic/{index,tjenester,om-oss,kontakt}.html` — full OG + Twitter Card block on each
- `demo-minnero-premium/{index,tjenester,om-oss,kontakt,minneside}.html` — same block, premium adds `og:locale:alternate en_US` since premium ships NO+EN
- All `og:image` URLs are now absolute against the per-tier `*.ibithun.com` domain. Per-page image picks match the on-page banner: `hero-candle.jpg` (index), `banner-services.jpg` (tjenester), `banner-about.jpg` (om-oss), `banner-contact.jpg` (kontakt), `memorial-banner.jpg` (premium minneside).
- `og:image:width 2400`, `og:image:height 1350` declared for each (matches the 16:9 source)
**Notes for future agents:**
- For all future prototypes in the family: never use a relative URL for `og:image`. WhatsApp's crawler doesn't resolve relative paths. The architecture guide should probably be updated with this rule (currently says nothing about OG tags).
- Every page deserves its own OG block — sub-pages aren't auto-inherited.
- WhatsApp caches link previews aggressively. After fixing OG tags, an already-shared link won't re-fetch automatically. Workarounds for testing: append `?v=1` to the URL, edit a path letter, or share to a new chat. For Facebook/LinkedIn, use `https://developers.facebook.com/tools/debug/` to force a re-scrape.
- `og:image:width/height` aren't strictly required, but WhatsApp uses them to allocate the preview box before the image loads — without them you sometimes see a flicker as the layout reflows.

## 2026-05-02 — Vercel deployment live, custom domains active, Web Analytics enabled
**Date:** 2026-05-02
**Tier(s) touched:** infra (no code changes)
**Reason:** Productionise the prototype on Vercel so future demos run from `*.ibithun.com` instead of localhost. Two separate Vercel projects (one per tier) sharing the same GitHub repo via per-project Root Directory + Ignored Build Step.
**What's now live:**
- **Classic project** in Vercel: `demo-minnero-classic` — Root Directory `demo-minnero-classic`, Ignored Build Step `git diff HEAD^ HEAD --quiet ./`
  - Production: `https://demo-minnero-classic.ibithun.com/`
  - Vercel fallback: `https://minnero-begravelsesbyra.vercel.app`
- **Premium project** in Vercel: `demo-minnero-premium` — Root Directory `demo-minnero-premium`, same Ignored Build Step
  - Production: `https://demo-minnero-premium.ibithun.com/`
  - Vercel fallback: `https://minnero-begravelsesbyra-jf2g.vercel.app`
- **Web Analytics enabled** on both projects via the dashboard toggle. The `_vercel/insights/script.js` snippet is already in every HTML page on both tiers (added during initial scaffold), so analytics started collecting the moment the toggle was flipped — no code change needed.
- **Visitor badge** (`visitor-badge.laobi.icu`) — `page_id` values were already set to `demo-minnero-classic.ibithun.com` (commented out, dormant on classic) and `demo-minnero-premium.ibithun.com` (live on premium). They lined up with the production custom domains by design, so the counter started attributing correctly the moment premium was deployed. **Zero code rewrite for the deployment.**
**Files changed:** none (this milestone is infra-only)
**Notes for future agents:**
- The Vercel `Ignored Build Step` setting on each project is **`git diff HEAD^ HEAD --quiet ./`** — Vercel scopes `./` to the project's Root Directory, so a commit that only edits files in `demo-minnero-classic/` will skip the premium build, and vice versa. Without this, every commit triggers two redundant builds.
- Vercel's Web Analytics dashboard shows a **Next.js setup wizard** by default (it's the most common framework). Ignore those instructions — our stack is vanilla HTML and the snippet pattern is already wired. The `Framework Preset` dropdown in the dashboard top-right is just a docs filter, not a project setting.
- Custom domains are managed at `goldenbutter`'s ibithun.com DNS. Subdomains: `demo-minnero-classic.ibithun.com` and `demo-minnero-premium.ibithun.com`. SSL certs auto-provisioned by Vercel via Let's Encrypt.
- Speed Insights remains **not enabled** — it requires a separate snippet (Core Web Vitals collector). Optional. If a customer wants it, that's a 10-line addition per HTML page on both tiers.
- DNS propagation can leave the local browser cache holding a failed lookup if the user tried the URL before propagation completed. `chrome://net-internals/#dns → Clear host cache` resolves it. Not a deployment problem.

## 62101d3 — Card float + icon bounce on premium service + memorial cards; hover-pause + accent-border glow
**Date:** 2026-05-01
**Tier(s) touched:** premium
**Reason:** User review point — "service cards on home should float like memorial cards, not just lift on hover. Memorial cards lost their hover effect. Want both card types floating + on hover the float pauses with a border glow." User cited rameezscripts.com as the visual reference.
**Files changed:**
- `demo-minnero-premium/css/style.css`
  - `.service-card` — added `animation: card-float 4s ease-in-out infinite`; replaced hover `transform: translateY(-3px)` + shadow with `animation-play-state: paused` + `border-color: var(--color-primary)` + `box-shadow: 0 12px 32px rgba(44, 53, 61, 0.18)`. Stagger delays for `nth-child(2/3/4)` (0.55s / 1.1s / 1.65s).
  - `.memorial-card` — same hover-pattern swap (was `translateY(-2px)` + shadow, now paused + accent border + deeper shadow). Stagger delays already existed.
  - New `@keyframes icon-bounce` (`translateY 0 → -5px`, 3s ease-in-out infinite). Applied to `.service-icon` SVG with stagger (0s / 0.4s / 0.8s / 1.2s).
  - `prefers-reduced-motion` block extended: `.service-card`, `.memorial-card`, `.service-icon` all `animation: none`.
**Notes for future agents:**
- The hover does **not** add a `transform: translateY` — the paused animation holds the card at whatever float position it was at when the mouse arrived. This is intentional ("alive but holding still"). If you add a transform on hover, the animation transform overrides it (CSS animation precedence) and you'll get visual glitches.
- The pattern is mirrored from rameezscripts.com expertise/service cards. They use the same 4s/-8px float keyframe, same 3s/-5px icon bounce, same hover `animation-play-state: paused`.
- DO NOT propagate to classic — classic stays static. The skill SKILL.md §11 calls this out as an anti-pattern.

## 9791be5 — Replace lysning.no leftovers with minnero.no
**Date:** 2026-05-01
**Tier(s) touched:** both
**Reason:** Bithun caught that `mailto:post@lysning.no` and link-text `post@lysning.no` survived the Lysning → Minnero brand rename in 14 spots across HTML/JS. Needed cleanup before productising the skill so the verification grep is clean.
**Files changed:**
- 11 files: 4 classic HTML + 5 premium HTML + 2 `js/main.js` (NO + EN translation blocks on both tiers). 52 string replacements total.
**Notes for future agents:**
- Verify with `grep "lysning"` over the prototype — should return zero matches.
- If you ever need to grep the prototype for source-brand literals (during a customer fork), only `Minnero / Sande / Berg / Kirkebakken / Trondheim / 73 00 00 00 / @minnero.no` should be in the rewrite list. `lysning.no` is permanently gone.

## f1e14b1 — Add README documenting the two-tier structure
**Date:** 2026-05-01
**Tier(s) touched:** scaffolding
**Reason:** Customer-facing README was missing — needed before pushing to the GitHub repo `goldenbutter/minnero.begravelsesbyra`.
**Files changed:**
- `README.md` (NEW) — covers brand etymology, tier comparison table, stack note, local preview command, link to per-prototype Nano-Banana-prompt.md
**Notes for future agents:**
- README is customer-facing; do not duplicate internal build-log content here. Skill-internal content lives in `.claude/`.

## 6c4a340 — Brand rename Lysning → Minnero + image audit + lock hero positioning + minneside graveyard
**Date:** 2026-05-01
**Tier(s) touched:** both
**Reason:** Bithun's interior-design business already uses Lysning. Renamed the funeral-home prototype to Minnero (*minne* + *ro* — peaceful memory) so the two businesses don't collide. While renaming, took the chance to: audit image usage (every image used at least once on each tier), lock hero candle positioning empirically per device (mobile + desktop differ between tiers due to AI-generated mp4 vs still poster), wire the minneside graveyard image (Nano Banana export) after the memorial wall.
**Files changed:**
- Folder rename: `demo-lysning-classic/` → `demo-minnero-classic/`, `demo-lysning-premium/` → `demo-minnero-premium/`. All file paths inside the rename are equivalent renames.
- `js/main.js` (both tiers) — translations object NO + EN keys updated where they referenced "Lysning" → "Minnero", "Om Lysning" → "Om Minnero", `og:image` updates etc.
- All HTML pages — `<title>`, `<meta>`, `data-i18n` text content, footer copyright text rewritten
- `demo-minnero-premium/css/style.css` — locked positioning values:
  - Base `.hero-video { object-position: center 8%; }` (desktop, low Y to keep flame in frame and reveal smoke space above)
  - Mobile override at end of file (after base rule due to source-order specificity): `.hero-video { object-position: 77% 65%; transform: scale(1.22); }` — scale 1.22 creates vertical overflow that 65% Y offsets to push candle down on portrait mobile
- `demo-minnero-classic/css/style.css` — mobile override: `.hero-candle .hero-bg { background-position: 74% 30%; transform: scale(1.04); }` — different X% from premium because still poster has candle at slightly different position than mp4
- `demo-minnero-premium/minneside.html` — added `interstitial--photo` section with `minneside-graveyard.jpg` between the `</main>` close and `<footer>` (before footer, after memorial wall — explicit user placement)
- `demo-minnero-premium/css/style.css` — added `.values-decor` CSS rule (88px circular wreath disc, copied from classic), added `.interstitial--photo` modifier class (lighter overlay + opacity:1 for non-captioned interstitials), lightened `.page-banner-overlay` from `0.65/0.20` to `0.45/0.08` so banner imagery reads brighter
- 4 new image files copied: `interstitial-1.jpg`, `interstitial-2.jpg`, `memorial-banner.jpg` into `demo-minnero-classic/assets/images/`; `hero.jpg` into `demo-minnero-premium/assets/images/`
- New: `demo-minnero-premium/assets/images/minneside-graveyard.jpg` (Nano Banana export, graveyard at dusk)
**Notes for future agents:**
- The hero positioning values are locked per device — **never re-derive**. Math is in the SKILL.md §10. The asymmetry (classic 74% vs premium 77%) is because the AI mp4 has the candle at a different X% than the JPG poster.
- Image audit confirmed all 12 shared images are referenced at least once on each tier. Premium has 1 extra (minneside-graveyard) + 1 video (hero-candle.mp4).
- The whole 3-hour review session that produced these locked values is captured as a parity report — that report became the basis for `begravelsesbyra-skill`.

## bf4f92f — Wire unused images to fill empty layouts
**Date:** 2026-05-01
**Tier(s) touched:** both
**Reason:** User review point #2 — "both sites looks very empty without images." Three of the four "reserved" images from Nano-Banana-prompt.md were already on disk; this commit places them in layouts.
**Files changed:**
- `demo-lysning-classic/assets/images/chapel-interior.jpg`, `morning-water.jpg` (NEW — copied from `_public/`)
- `demo-lysning-classic/index.html` — new `.photo-strip` between Services and Preparing (chapel-interior, no animation)
- `demo-lysning-classic/om-oss.html` — `.values-decor` 88px wreath disc above Values heading
- `demo-lysning-classic/css/style.css` — `.photo-strip` + `.values-decor` rules appended
- `demo-lysning-premium/index.html` — second `.interstitial` (interstitial-2.jpg + already-wired i18n string `interstitial2.quote/author`) between About Preview and Contact CTA
- `demo-lysning-premium/tjenester.html` — caption-less `.interstitial` with chapel-interior before the Contact CTA — pure visual breath
**Notes for future agents:**
- `morning-water.jpg` is still reserved — not yet wired into HTML on either tier. Use it if a future change wants a third interstitial on premium home, or to replace a banner.
- The classic `.photo-strip` is intentionally NOT a `.fade-in` element — classic stays static. If a customer pays for animation upsell, the IntersectionObserver could be re-enabled and `.photo-strip` could swap to `.fade-in`.
- `interstitial-2`'s i18n strings were already in `js/main.js` translations from the original premium build, so wiring it into HTML required only one new section block.

## 213aa87 — Premium mobile menu: 320px liquid-glass slide-in
**Date:** 2026-05-01
**Tier(s) touched:** premium
**Reason:** User review point #3 (premium half) — "320px, glass hover effect, liquid glass effect, will not be covering at all." Strict rules.
**Files changed:**
- `demo-lysning-premium/css/style.css` — `.mobile-menu` rewritten as a fixed 320px right-edge panel with `backdrop-filter: blur(28px) saturate(1.4)`, ivory-tinted translucent background, hairline link dividers, premium 550ms cubic-bezier slide easing
- `demo-lysning-premium/js/main.js` — body-scroll lock removed, closeMenu() + click-outside + Escape handlers added
**Notes for future agents:**
- Liquid-glass effect requires `backdrop-filter` support. iOS Safari < 14 falls through to the solid `rgba(245, 243, 238, 0.55)` background — still readable, just no blur. Don't add a `@supports` fallback that increases opacity; the 0.55 opacity is already legible without blur.
- DO NOT copy this design to classic — classic has a totally different compact-dropdown design (see commit 6c66f46).
- The `max-width: 88vw` clause prevents the 320px panel from edge-bleeding on smaller phones (e.g. iPhone SE at 320px viewport — panel would cover edge-to-edge otherwise, defeating the "not covering" requirement).

## 6c66f46 — Classic mobile menu: compact dropdown panel
**Date:** 2026-05-01
**Tier(s) touched:** classic
**Reason:** User review point #3 — "the mobile menu looks so ugly... covering the whole website, the whole page, and it is a small menu in the middle." Replaced full-page overlay with a 260px dropdown anchored just below the nav.
**Files changed:**
- `demo-lysning-classic/css/style.css` — `.mobile-menu` rules: `position: fixed; top: 72px; right: 1.25rem; width: 260px;` plus simple stacked-link styles with hairline dividers
- `demo-lysning-classic/js/main.js` — body-scroll lock removed, click-outside-closes + Escape-closes added, stop-propagation on hamburger so document-click handler doesn't re-close immediately
**Notes for future agents:**
- The menu is `position: fixed`, not absolute, because `.mobile-menu` sits OUTSIDE `<nav>` in the DOM. Switching to `position: absolute` requires moving the element inside `<nav>` first.
- Premium mobile menu is a separate, very different design (320px liquid glass slide-in) — see next commit.

## ef64f99 — Hero parity: classic gets candle still, premium keeps video
**Date:** 2026-05-01
**Tier(s) touched:** classic (hero swap) + classic CSS additions
**Reason:** User review point #4 — "If you use this candlelight as a hero image, put it on both sides. Use the static candle for classic and the animated candle for premium." Visual continuity across tiers.
**Files changed:**
- `demo-lysning-classic/assets/images/hero-candle.jpg` (NEW — copied from `_public/hero-candle.jpeg`)
- `demo-lysning-classic/index.html` — hero now uses `hero-candle.jpg` + adds `.hero-candle` class
- `demo-lysning-classic/css/style.css` — appended `.hero-candle .hero-overlay` radial-vignette rule (parity with premium CSS)
**Notes for future agents:**
- Old `demo-lysning-classic/assets/images/hero.jpg` (misty fjord) is on disk but unreferenced. Do NOT delete unless you check no other file links it. The `_public/hero.jpeg` source is preserved.
- Premium hero remains a `<video>` with `hero-candle.jpg` as poster fallback — this commit doesn't touch premium.

## ff26891 — Classic: strip i18n, hover/scroll animations, visitor badge
**Date:** 2026-05-01
**Tier(s) touched:** classic
**Reason:** User review point #1 — "classic site should have only Norwegian, the EN-NO toggle will be in premium" + "classic cards hovering, popping up — those features are only for premium" + "in classic, comment out [visitor badge]". User said comment out, NOT delete, so the upsell path stays open.
**Files changed:**
- `demo-lysning-classic/index.html`, `tjenester.html`, `om-oss.html`, `kontakt.html` — `lang-toggle` button commented out in nav + mobile menu, `<img>` visitor badge commented out in footer
- `demo-lysning-classic/js/main.js` — `applyTranslations()`, `initLangToggle()`, `initScrollAnimations()`, `.page-fade` apply commented at init (functions kept intact)
- `demo-lysning-classic/css/style.css` — `.reveal`, `.reveal-stagger`, `.page-fade` block commented out + safety fallback `.reveal { opacity: 1; transform: none; }` added; service-card hover transform/shadow commented out; btn-primary hover lift commented out; text-link gap arrow-slide commented out; modal entrance translateY removed (now fade-only)
**Notes for future agents:**
- If customer wants EN: uncomment the four init calls in [demo-lysning-classic/js/main.js](../demo-lysning-classic/js/main.js#L450) and the `.lang-toggle` buttons in all four HTML pages.
- If customer wants animations back: uncomment the `.reveal`/`.page-fade` block in [demo-lysning-classic/css/style.css](../demo-lysning-classic/css/style.css) AND remove the safety fallback line.
- DO NOT propagate this strip to premium — premium keeps i18n + animations + visitor badge.

## 2329366 — OG fix: dedicated 1200×630 sub-600 KB variants per page
**Date:** 2026-05-03
**Tier(s) touched:** both (classic + premium)
**Reason:** opengraph.xyz preview (screenshot) flagged the live site on every page: image was 1376×768 instead of 1200×630, and several source banners were 700 KB – 1 MB — above the WhatsApp 600 KB ceiling that silently drops link previews to text-only. The previous Open Graph commit (`21635d1`) had pointed every page at full-resolution in-page banners, which is the recurring family-wide failure mode documented in `_docs/_skill/CUSTOMER-FORK-FINISH-CHECKLIST.md` Section A.

**Files changed:** all 9 HTML pages (og:image, og:image:width, og:image:height, twitter:image) + 5 new OG variants mirrored to both tiers (10 new image files total).

**Generated variants (ffmpeg `scale=1200:630:force_original_aspect_ratio=increase,crop=1200:630 -q:v 5`):**
- `minnero-og-preview.jpg`  (11 KB)  ← from `hero-candle.jpg`     → index/landing
- `minnero-og-services.jpg` (127 KB) ← from `banner-services.jpg` → tjenester
- `minnero-og-about.jpg`    (143 KB) ← from `banner-about.jpg`    → om-oss
- `minnero-og-contact.jpg`  (69 KB)  ← from `banner-contact.jpg`  → kontakt
- `minnero-og-memorial.jpg` (14 KB)  ← from `memorial-banner.jpg` → minneside (premium-only)

All five well under 600 KB; alt text on every page preserved unchanged.

**Section B note (also applies retroactively to commits `1584a67` and `f07b99d`):** Those two prior docs/`.gitignore` commits silently CANCELED on both Vercel projects because `git diff HEAD^ HEAD --quiet ./` returned 0 (no files in either tier root). The live site stayed on `21635d1` until this commit. The OG fix touches files in BOTH `demo-minnero-classic/` and `demo-minnero-premium/` tier roots, so the head's diff comes back non-empty for both projects' Ignored Build Step → both rebuilt → both now serve fresh content. Bundling the entire fix in one commit avoided the multi-commit-push gotcha entirely.

### Customer-fork finish checklist — completed 2026-05-03

- [x] **Section A** — OG variants generated (filename pattern: `minnero-og-*.jpg`), all under 600 KB at 1200×630, every HTML page references them via absolute `https://demo-minnero-<tier>.ibithun.com/...` URL with matching `twitter:image`. Verified via grep: zero remaining `og:image` references to `hero-*` / `banner-*` / `memorial-banner` source files; zero remaining `og:image:width content="2400"`; zero relative-path `og:image` values.
- [x] **Section B** — Vercel deployment for SHA `2329366` reached `state: READY` on production for both tiers (deployment IDs: classic=`dpl_kskPtCy5U2iHV5f4p71uYUfg63v9`, premium=`dpl_75S6iE8qR9mjhLo7mjhh8kyeXvEC`). Recovered the two prior CANCELED docs commits in the same push.
- [x] **Section C** — Live `curl` of all 9 production URLs confirms each page serves the new `minnero-og-*.jpg` variant with `og:image:width content="1200"` and `og:image:height content="630"`, plus matching `twitter:image`. HEAD requests on the OG image URLs return `200 OK` with `Content-Length` 11151 (preview) and 14448 (memorial) — well below the 600 KB WhatsApp ceiling. The remaining opengraph.xyz warnings shown in the screenshot ("missing headline-in-image", "missing call-to-action-in-image", "description short") are aesthetic — checklist explicitly says skip for noindex demos.
- Reference: `C:\Project\prototypes\_docs\_skill\CUSTOMER-FORK-FINISH-CHECKLIST.md`.

**Notes for future agents:**
- If the customer ships replacement banner photos later, regenerate the corresponding `minnero-og-*.jpg` from the new source via the same ffmpeg recipe — otherwise the link preview will show stale photos.
- WhatsApp caches OG previews ~30 min independently from opengraph.xyz. To bust on demand: paste `https://demo-minnero-<tier>.ibithun.com/?v=2` (any unused query string) into a fresh chat once.

## 02b5ba3 — Initial commit: Lysning classic + premium scaffold
**Date:** 2026-05-01
**Tier(s) touched:** scaffolding
**Reason:** Establish baseline before review-cycle changes begin.
**Files changed:** all (initial)
**Notes for future agents:** This commit captures the project as first delivered to the user. Several rough edges were already flagged in the user's first review (i18n on classic, hover animations on classic, mobile menu ugliness, hero mismatch between tiers, visitor badge on classic, missing BUILD_PROGRESS.md). Subsequent commits address each.
