# LaundVend

**Better laundry. More choices.**
A front-end prototype by **Flexbox Pod**, built with HTML and CSS only.

LaundVend connects households in Lagos with laundry vendors who are genuinely
available at the pickup and delivery times the customer needs.

---

## The problem

Laundry vendors already operate across Lagos, and most already wash, iron,
collect and deliver. Supply is not the bottleneck. **Visible availability is.**

A customer who needs clothes washed on a particular day cannot tell which vendor
has capacity that day. Opening hours do not answer the question, so every
booking still begins with a round of calls and messages. On the other side, a
vendor with a free afternoon has no simple way to tell nearby customers they are
open for work. Demand and supply exist at the same moment and never meet.

## The solution this site demonstrates

Put **availability and scheduling first in the search**, not last.

The customer starts with their area, the service they need and the pickup window
that suits them. Only then do they see vendors — and only the vendors who can
meet all three. Every listing then presents the same facts in the same order
(availability, location, trust, pricing) so two vendors can be compared at a
glance instead of over two phone calls.

That single idea is what each page is built to serve.

---

## Pages

| Page | Purpose |
| --- | --- |
| `index.html` | Landing page: the problem, the approach, feature summary, vendor preview, coverage |
| `how-it-works.html` | The six-step order journey and the six order-tracking stages |
| `vendors.html` | The vendor directory — availability filter and comparable listings |
| `book.html` | Pickup request form |
| `request-received.html` | Confirmation shown after the booking form is submitted |
| `guide.html` | Guide and FAQ, grouped by topic |
| `problem-statement.html` | The research and problem statement behind the project |
| `404.html` | Not-found page |

## Project structure

```
.
├── index.html                 ← entry point
├── how-it-works.html
├── vendors.html
├── book.html
├── request-received.html
├── guide.html
├── problem-statement.html
├── 404.html
├── favicon.svg
├── robots.txt
├── sitemap.xml
├── vercel.json                ← security and cache headers
├── css/
│   ├── brand.css              ← design tokens, reset, base elements, primitives
│   └── site.css               ← header, footer, components, page sections
└── images/
```

`brand.css` holds **only** tokens and base/primitive rules. `site.css` holds
components. Nothing is styled inside an HTML file, and there are no inline
`style` attributes anywhere.

## Running it

It is a static site — no build step, no dependencies, no JavaScript.

```bash
# open directly
xdg-open index.html

# or serve it
python3 -m http.server 8000    # then visit http://localhost:8000
```

## Technology

- **HTML** — semantic structure and content
- **CSS** — design system, layout, responsiveness and all interaction

There is no JavaScript in this project by design. The FAQ accordions use native
`<details>`/`<summary>`, and the navigation is responsive without a script.

---

# Corrections log

Everything below was wrong, missing or broken in the previous version and has
been fixed. Grouped by kind.

## 1. The site did not work when deployed

| # | Problem | Fix |
| --- | --- | --- |
| 1.1 | **No `index.html` existed.** Every nav link across the project pointed at `index.html`, and a static host serves that file as the site root. The deployed site had no home page and every "Home" link was dead. | Added `index.html` as the real landing page. |
| 1.2 | **Case-sensitive path bugs.** `directory.html` linked `style.css` (file was `Style.css`) and `css/brand.css`, `css/landing.css` (directory was `CSS/`). These resolve on Windows and macOS but **404 on Linux hosting**, so the deployed page shipped unstyled. | All paths are now lowercase `css/` and match the files exactly. |
| 1.3 | **Broken relative paths inside subdirectories.** `HTML/problem-statement.html` linked `./CSS/brand.css`, which resolves to `HTML/CSS/brand.css` — a path that never existed. | Flat structure; every page links `css/brand.css` and `css/site.css` from the root. |
| 1.4 | **Links to pages that did not exist:** `main.html`, `services.html`, `pricing.html`, `contact.html`, `logo.png`. | Removed. Every link now points at a page that exists. |
| 1.5 | **A hard-coded absolute link** to `https://laundvend.vercel.app/index.html` inside the nav, which broke local development and any other deployment. | Replaced with a relative link. |
| 1.6 | No `404.html`, so a mistyped URL gave the host's default error page. | Added a branded `404.html`. |

**Verified:** every internal link, anchor (`#id`) and asset reference on all
eight pages was checked programmatically and resolves.

## 2. Duplicated and conflicting files

| # | Problem | Fix |
| --- | --- | --- |
| 2.1 | `how-it-works.html` existed **twice**, byte-for-byte identical, at the root and in `HTML/`. Editing one silently left the other stale. | One copy. |
| 2.2 | **Two different files both named `brand.css`** — one at the root, one in `CSS/`, with completely different contents. Which one applied depended on the page. | One `css/brand.css`. |
| 2.3 | Five stylesheets (`Style.css`, `landing.css`, `directory.css`, `guide.css`, `brand.css`) redefined the same resets, colours and header rules in conflicting ways. | Two stylesheets with one clear responsibility each. |
| 2.4 | Files named after a developer rather than their purpose: `Samuel.html` (the booking form), `Style.css`. | Renamed to `book.html` and `css/site.css`. |
| 2.5 | `main.js` contained only `console.log('Hello World!')` and was dead code. | Deleted — the project is HTML and CSS only. |

## 3. Invalid and deprecated HTML

| # | Problem | Fix |
| --- | --- | --- |
| 3.1 | **`Samuel.html` had an unclosed `<head>`** — no `</head>` tag at all, so the browser had to guess where the body began. | Valid document structure on every page. |
| 3.2 | Same file had a **stray `</div>`** closing an element that was never opened, and a mangled image tag whose leftover `width="70px" height="70px">` text leaked out and rendered as visible content on the page. | Removed. |
| 3.3 | **Deprecated `<center>` element** used in `landing.html`. Removed from HTML years ago. | Layout is done with CSS. |
| 3.4 | **Deprecated `border="0"` attribute** on images. | Removed; borders are CSS. |
| 3.5 | Invalid attribute values such as `width="100px"` (HTML width takes a bare number, not a unit). | Correct `width`/`height` integers. |
| 3.6 | **Presentational markup used for emphasis** — `<strong><em>` wrapped around body text and `<em>` inside every nav link, to make text look a certain way rather than to mean anything. | Removed; visual weight is CSS, and `<strong>` now only marks genuine importance. |
| 3.7 | Brand name spelled three different ways across the project: *Laundvend*, *LaundVend* and **"LaundVand"** (a typo in the `<title>` of the how-it-works page, the text search engines show). | **LaundVend** everywhere. |
| 3.8 | Heading levels skipped from `<h1>` straight to `<h3>`, which breaks screen-reader document outlines. | Heading order is now continuous on every page (checked programmatically). |

## 4. It was not one website

| # | Problem | Fix |
| --- | --- | --- |
| 4.1 | **Five pages, five completely different headers** — different logos, different nav links, different markup, different styling. `directory.html` used a remote image logo, `guide.html` used a text logo, `problem-statement.html` used a missing `logo.png`, and `how-it-works.html` **had no header or footer at all** — it rendered as a bare fragment. | One header and one footer, byte-identical on all eight pages, with the current page marked via `aria-current="page"`. |
| 4.2 | No consistent typography, spacing or colour. Each stylesheet invented its own values. | A single token scale in `css/brand.css` — colour, type, spacing, radii, shadows — used by every rule. |
| 4.3 | The logo was loaded from **`i.ibb.co`, a third-party image host**, on every page: an external dependency outside the team's control that could break or disappear, plus a render-blocking cross-origin request for a 90 KB JPEG. | Replaced with an inline SVG mark and HTML wordmark. No request, sharp at any size, and it adapts to the colour scheme. |

## 5. Accessibility

| # | Problem | Fix |
| --- | --- | --- |
| 5.1 | **`--ink` was used as the body text colour but was never defined**, in either brand sheet. Text fell back to the browser default black instead of the brand colour — a live bug. | All tokens are defined; no rule references an undefined variable. |
| 5.2 | **Failing colour contrast.** The teal `#20C9C3` was used as text on white at roughly **2:1**, far below the WCAG AA minimum of 4.5:1. | Split into `--accent` (fills and borders) and `--accent-ink` (text). Every text/background pair in the site was measured; the lowest now passes at 4.6:1, and the muted grey was darkened after testing showed it at 4.2:1 against the page background. |
| 5.3 | No skip link — keyboard users had to tab through the whole nav on every page. | "Skip to main content" on every page. |
| 5.4 | No visible focus styles, so keyboard focus was invisible against the default outline in several places. | One consistent, high-contrast focus ring site-wide. |
| 5.5 | **Tap targets below the 24×24px minimum** (WCAG 2.5.8): footer links were 18px tall and the consent checkbox 20px. | Enlarged; all pages now pass at 320px, 768px and 1440px. |
| 5.6 | Images had weak or duplicate alternative text. | Alt text describes each photo; decorative images use `alt=""`, and every icon is `aria-hidden`. |
| 5.7 | No landmarks or labelled navigation on most pages. | `<header>`, `<nav aria-label>`, `<main id="main">`, `<footer>` on every page. |
| 5.8 | The booking form's fields were not grouped, and its `<legend>` headings rendered with a border line struck through the text. | Fields grouped in `<fieldset>` with real `<legend>` elements, and the legend box-model conflict is fixed. |
| 5.9 | No respect for a reduced-motion preference. | `prefers-reduced-motion` disables transitions and smooth scrolling. |

**Verified:** every form control on every page has a matching `<label for>`, and
no label points at a control that does not exist.

## 6. Responsiveness

| # | Problem | Fix |
| --- | --- | --- |
| 6.1 | The vendor listings were **not a grid** — every card stacked full width at all screen sizes, so comparing vendors on a laptop meant scrolling past one card at a time. Comparison is the whole point of the page. | Responsive card grid that reflows from four columns to one. |
| 6.2 | The availability filter was a single stacked column at every width. | Four-across on desktop, two on tablet, one on phone. |
| 6.3 | The how-it-works steps were locked to three columns until a single 760px breakpoint, so they were cramped on tablets. | Fluid grids with sensible intermediate steps. |
| 6.4 | The header overflowed and wrapped awkwardly between roughly 860px and 1040px, dropping the call-to-action onto its own misaligned row. | Breakpoint corrected; below 1040px the nav becomes a full-width scrollable row and the CTA stays beside the logo. |
| 6.5 | Fixed font sizes did not scale between breakpoints. | Fluid type with `clamp()`. |

**Verified:** all eight pages were rendered at **320px, 768px and 1440px** and
none produces horizontal page scroll.

## 7. Performance and production readiness

| # | Problem | Fix |
| --- | --- | --- |
| 7.1 | **Images had no `width`/`height`**, so the page jumped as each photo loaded (cumulative layout shift). | Dimensions on every image, plus `loading="lazy"` and `decoding="async"` on below-the-fold photos. |
| 7.2 | `--body: 'Inter'` was declared as the body typeface but **Inter was never loaded**, so the site silently fell back to a system font. Roboto was loaded on some pages and not others. | Both faces loaded once, in a single request, with `display=swap` and full fallback stacks. |
| 7.3 | Google Fonts was requested without `preconnect` on several pages, adding a round trip. | `preconnect` on every page. |
| 7.4 | No favicon — browsers requested `/favicon.ico` and got a 404 on every page load. | `favicon.svg` matching the brand mark. |
| 7.5 | No meta description on most pages, and no social sharing metadata anywhere. | Unique description per page, plus Open Graph, Twitter Card and canonical tags. |
| 7.6 | No `robots.txt` and no `sitemap.xml`. | Both added. |
| 7.7 | No `.gitignore`, so OS and editor files could be committed. | Added. |
| 7.8 | `vercel.json` had been added and deleted repeatedly and was absent, so the deployment sent no security or caching headers. | Restored with `X-Content-Type-Options`, `Referrer-Policy`, `X-Frame-Options`, `Permissions-Policy`, and long-lived caching for static assets. |
| 7.9 | Dead CSS — rules for classes no page used. | Removed. The stylesheets and the markup are now a closed set: no class is used without a rule, and no rule exists without a use. |
| 7.10 | No print styling. | Print stylesheet added, which matters for a page carrying vendor prices. |

## 8. Design and product corrections

| # | Problem | Fix |
| --- | --- | --- |
| 8.1 | **The landing page did not show the product's core idea.** It described availability in prose but never showed an availability search. | The hero now leads with a static "example search" card — location, service, pickup window, delivery window, and the vendor it matched — so the idea is visible in the first screen. |
| 8.2 | **The booking form was a dead end.** It had no `action` and no `method`, so pressing "Request booking" did nothing at all. A "Booking request received" message sat in the markup below it, hidden by `display: none` and only revealable by JavaScript that did not exist — so the form could never give the user any feedback, ever. | The form submits to a real confirmation page (`request-received.html`), which states plainly that this is a prototype. The unreachable success block is gone. |
| 8.3 | The search form on the directory had no `action` either. | Submits to `vendors.html` — the honest static equivalent of a filtered result. |
| 8.4 | Vendor listings buried the availability chip beside the vendor name, squeezing names onto two lines and misaligning the cards. | Availability moved onto the photo, where it reads first and leaves the name full width. |
| 8.5 | Vendor details were loose `<p><strong>Label:</strong> value</p>` pairs — a label/value structure not marked up as one. | `<dl>` description lists, which is what the content actually is, laid out on a grid so the same field sits in the same place on every card. |
| 8.6 | The FAQ answered a generic laundry service, not this one. It never mentioned availability, matching, or what happens when a vendor cannot make a window. | Rewritten around the product, and grouped into Getting started / Pickup & delivery / Care & trust / Pricing & payment. |
| 8.7 | Copy repeatedly promised order tracking, but no page ever explained what the stages were. | The six order stages are laid out on `how-it-works.html`. |
| 8.8 | Nothing on the site addressed vendors, despite them being half the marketplace and half the stated problem. | A "for vendors" section on `how-it-works.html` and a vendor column in the footer. |
| 8.9 | Sample vendor data was presented as though real, with no indication otherwise. | Footer states on every page that listings are illustrative sample data. |
| 8.10 | Single fixed colour scheme. | Light and dark schemes from the same tokens, both meeting AA contrast. |

---

## Notes for the team

**Connecting a backend.** Both forms use `method="get"` because a static host
cannot accept a POST. When an API exists, change the booking form to
`method="post" action="/api/requests"` — the field names are already sensible
(`service`, `area`, `pickup-date`, `pickup-time`, `delivery-date`,
`delivery-time`, `first-name`, `last-name`, `email`, `phone`, `address`,
`notes`). Until then, be aware that GET puts submitted values in the URL, which
is fine for the search filter but should not carry real personal details.

**Set the real domain.** The canonical, Open Graph and sitemap URLs currently
use `laundvend.vercel.app`. Update them in the `<head>` of each page and in
`sitemap.xml` when the final domain is decided.

**Vendor photography.** `images/washing.jpeg` is a marketing flyer rather than a
photograph, so it sits oddly beside the other listings. Replacing it with a
photo of actual work — and compressing all six images, which are currently
90–165 KB each and larger than they are ever displayed — would be the next
worthwhile improvement.

**Adding a vendor.** Copy an existing `<article class="vendor">` block in
`vendors.html` and change the content. Use `chip--today` or `chip--tomorrow` on
the badge. No CSS changes are needed.

**Keep the token discipline.** Colours, spacing and radii live in
`css/brand.css`. Use the existing tokens rather than adding new literal values —
that is what keeps the pages looking like one site. Note in particular that
`--brand` is a *filled surface* colour that always pairs with white text; use
`--ink` for text.

---

**Flexbox Pod**
