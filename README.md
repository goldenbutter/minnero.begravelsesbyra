# Minnero Begravelsesbyrå — Demo

Two-tier static site demo for a small Norwegian funeral home. The same content is delivered through two different aesthetic + interaction registers so a customer can choose the level of polish they want before committing to a build.

## Brand

**Minnero** — *minne* (remembrance) + *ro* (peace, calm). Distinct from the family business *Lysning* (interior design).

## Live

- Classic: [demo-minnero-classic.ibithun.com](https://demo-minnero-classic.ibithun.com/)
- Premium: [demo-minnero-premium.ibithun.com](https://demo-minnero-premium.ibithun.com/)

Deployed on Vercel — two projects sharing this repo, each scoped to its tier folder via per-project Root Directory + `git diff HEAD^ HEAD --quiet ./` Ignored Build Step. Web Analytics enabled on both.

## Tiers

| | demo-minnero-classic | demo-minnero-premium |
|---|---|---|
| Pages | 4 (index, tjenester, om-oss, kontakt) | 5 (+ minneside) |
| Hero | Static `hero-candle.jpg` | `hero-candle.mp4` autoplay loop with poster fallback |
| Animations | Disabled — fully static | Scroll reveals, parallax, photo fade, floating memorial cards |
| i18n | Norwegian only (toggle commented out, strings preserved) | EN/NO toggle active, localStorage-persisted |
| Visitor badge | Commented out | Rendered in footer |
| Mobile menu | 260px compact dropdown | 320px liquid-glass slide-in from right edge |
| Interstitial photos | Static `.photo-strip` sections | Animated `.interstitial` with quote captions |

Both tiers share: the same Cormorant Garamond + Inter type pairing, the same colour palette (`#F5F3EE` ivory · `#5C6B7A` slate-blue · `#2C353D` deep slate · `#8E9AA6` accent), the same content/copy via `data-i18n` keys, the same image assets.

## Stack

Plain HTML / CSS / JS — no framework, no build step. Each tier is a self-contained folder.

## Folder structure

```
minnero.begravelsesbyra/
├── README.md
├── demo-minnero-classic/
│   ├── index.html
│   ├── tjenester.html
│   ├── om-oss.html
│   ├── kontakt.html
│   ├── favicon.svg
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   └── main.js
│   └── assets/
│       └── images/         (12 shared images — hero, hero-candle, preparing,
│                            wreath, chapel-interior, morning-water, banners,
│                            interstitials, memorial-banner)
└── demo-minnero-premium/
    ├── index.html
    ├── tjenester.html
    ├── om-oss.html
    ├── kontakt.html
    ├── minneside.html       (memorial wall — premium-only)
    ├── favicon.svg
    ├── css/
    │   └── style.css
    ├── js/
    │   └── main.js
    ├── data/
    │   └── memorials.json   (memorial wall content — JSON-driven)
    └── assets/
        ├── images/          (12 shared + minneside-graveyard.jpg)
        └── videos/
            └── hero-candle.mp4
```

## Local preview

```bash
python -m http.server 5500
# then visit:
#   http://localhost:5500/demo-minnero-classic/
#   http://localhost:5500/demo-minnero-premium/
```



Developed by [Bithun](https://github.com/goldenbutter)

