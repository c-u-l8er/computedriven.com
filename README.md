# computedriven.com

The root of ComputeDriven, a systems studio. **A product index and a funnel**,
not a product page.

One static page, no build step, no npm. `index.html` carries its own CSS and a
little vanilla JavaScript. The portfolio nav is loaded from `/amp-nav.js`, which
is deployed by `sync-nav.sh` in the [&] workspace rather than edited here.

## What this page is, and what changed on 2026-08-25

Until 2026-08-25 the root **was** the T&R product page. Measured by section,
~79 KB of its ~99 KB of body copy was about the operating system, 8 KB about
Cloud, and 11.8 KB about ComputeDriven itself — on a domain with three products.
The hero made a studio-level claim and then handed you a FreeBSD image.

That page moved out of this repository intact — to
[`c-u-l8er/tr.computedriven.com`](https://github.com/c-u-l8er/tr.computedriven.com),
which took this repository's history with it (see **Deployment**). This file is
its replacement, and it has a different job:

**One destination, two ways in.**

- **[World] Cloud** is the destination — the thing being funnelled toward.
- **T&R** is door one: download the operating system, run it on metal you own.
- **Super (CD)** is door two: the compute surface, **a desktop application**
  (Tauri v2 — see `super/cockpit/` in the workspace). You download and install
  it.

**Both doors are a download.** The page said otherwise until 2026-08-25 — the
funnel labelled Super "nothing to install" and the `#ways` headline read *"the
other asks nothing of your machine"*, which was simply false about a desktop
app. Corrected in six places: the meta description, the SVG path label, the SVG
subtitle, the mobile `.funnel-stack` row, the `.vh` text equivalent, and the
headline. **Do not reintroduce "no install" as Super's differentiator** — the
real distinction is narrower and still worth making: *both are a download, only
one of them replaces what you boot.*

The page is laid out to make that shape legible before any card is read — a
figure in the hero, then Cloud at hero weight, then the two doors as siblings.
**Do not flatten this into three equal product cards.** Three equal cards state
that the studio has three things; the current layout states which one you are
being sold and that there are two ways to buy it. That is the whole reason the
root stopped being a product page.

## Every link points at its final destination, including the dead ones

**Travis's call, 2026-08-25, and it reverses what this page did earlier that
day.** Every product and article link is wired to the URL it will have when the
stack is deployed, not to the URL that answers today. The page is built to be
correct at deploy time.

Eight of them do not resolve right now:

| link | why not | fixed by |
|---|---|---|
| `cloud.computedriven.com` | no DNS record | Pages project + custom domain |
| `super.computedriven.com` | no DNS record | Pages project + custom domain |
| `tr.computedriven.com` | no DNS record | Pages project + custom domain |
| `/capability-composition-masterclass` | file untracked in `AmpersandBoxDesign` | commit + push |
| `/ai-factories-masterclass` | same | same |
| `/continuous-verification-masterclass` | same | same |
| `/world-economics-masterclass` | same | same |
| `/masterclasses` | same | same |

(Eight links, two causes: three domains with no DNS, five files never
committed. `agentic-engineering-masterclass` is the one masterclass that *is*
committed, and it works — which is how the split was spotted.)

**The tradeoff, stated once so it is not rediscovered as a bug.** The earlier
version of this page refused to link anything that did not resolve — status
pills instead of buttons, `not posted` markers instead of anchors, everything
routed through `#join`. That version was correct on the day and wrong for the
week after, because it would have needed a second editing pass the moment each
domain came up, and a page that needs an edit to stop lying is a page that will
keep lying. Wiring the destinations means the page is finished once and the
remaining work is deployment.

**So do not "fix" a dead link here by unlinking it.** Deploy the thing.

### Pre-push checklist

Nothing on this page needs editing before launch. These do:

1. Pages project for `tr.computedriven.com` → repo `c-u-l8er/tr.computedriven.com`
2. Pages project for `cloud.computedriven.com` → the `cloud.computedriven.com/` dir
3. Pages project for `super.computedriven.com` → `super/site/`
4. `AmpersandBoxDesign`: commit and push the five untracked pages in `site/`

Verify by status code, not by looking at the tree — a file existing locally is
what caused five of these eight to be linked from the shared nav while 404ing in
public:

```
for h in tr cloud super; do printf "%-8s " $h; getent hosts $h.computedriven.com >/dev/null && echo ok || echo NO-DNS; done
for u in agentic-engineering capability-composition ai-factories continuous-verification world-economics; do
  printf "%-28s " $u; curl -s -o /dev/null -w '%{http_code}\n' https://ampersandboxdesign.com/$u-masterclass; done
```

**Status pills stay.** `in dev` on Cloud and Super describes the *product*, not
whether the site answers — both are genuinely unfinished, the shared nav says so
too, and a working link to an in-development product is not a contradiction.
Remove a pill when the product ships, not when its domain resolves.

## `#join` is load-bearing — do not rename it

The shared portfolio nav's CTA points **every property in the portfolio** at
`https://computedriven.com/#join` (7 references in `amp-nav.js`). That anchor
used to be the T&R boot-report section. When the T&R page moved, the anchor had
to stay and acquire a world-level meaning, or seven inbound links across the
portfolio would land on a page with no such id.

It is now the contact form plus a pointer to the boot-report path on GitHub.
**Boot reports themselves live on the T&R site**, which is correct — they are
about that product — but the studio-level "talk to us" has to answer here.

## Third parties, in full

The contact form posts to **Formspree** (`https://formspree.io/f/xaewoadr`).
Nothing else on this page calls out at load. Verified on the local server
2026-08-25 with zero off-origin resource entries:

```
performance.getEntriesByType('resource').map(r=>new URL(r.name,location.href).host).filter(h=>h&&h!==location.host)
```

**Separately, and not visible from this repository:** the host injects
`static.cloudflareinsights.com/beacon.min.js` at the edge. It is not in
`index.html` and not in what the server returns to `curl` — it took driving the
live domain in a real browser to find. The page names it rather than denying it.
**Do not add a "no telemetry" claim here.** That claim was on the old root page,
was false for as long as it existed, and was removed on the day it was found.
Re-verify against the live domain before writing any sentence about what this
page does or does not load:

```
agent-browser open https://computedriven.com/
agent-browser eval "performance.getEntriesByType('resource').map(r=>new URL(r.name).host).filter(h=>h!=='computedriven.com')"
```

## The form has three honest paths

It is a real `<form>` with `action` and `method`, so it works with scripting
off; the script only upgrades it to an inline reply. `_gotcha` is Formspree's
honeypot, positioned off-screen rather than `display: none`.

**"Sent" prints only on an actual 2xx.** Failures print the reason the endpoint
gave and the typed message survives for a retry. A form that thanks you on
submit and drops the message is precisely the failure this studio is about — if
you touch that handler, keep all three paths.

## Fonts

Self-hosted, `fonts/space-grotesk.woff2` and `fonts/jetbrains-mono.woff2`,
latin subset, **53,720 bytes for both**. Each is a **variable font covering the
whole 400–700 range**, which is why `@font-face` declares
`font-weight: 400 700` — declaring a single weight there silently flattens every
bold on the page. The T&R repository carries its own copy of both files, so the
two sites are independent; **if you ever re-subset or replace a font, do it in
both places** — nothing checks that they match.

## The funnel figure is two figures

The hero diagram is an SVG on a 760-unit viewBox. Below 640px it would scale to
about 0.39×, rendering its 11px labels at roughly 4px, so a CSS media query
hides it and reveals `.funnel-stack` — a vertical text version measured at
16.3px/11.2px on a 375px viewport. **If you edit one, edit both**; they are the
same claim and there is nothing keeping them in sync but this paragraph.

## The blog slot

`#writing` lists the five masterclasses as five live `.read` rows. **Four of
them 404 today** — the HTML exists and looks finished in
`AmpersandBoxDesign/site/` but is **untracked**, so it was never committed and
never deployed. `masterclasses.html` and `factory.html` are in the same state.
They are linked anyway, per the policy above; the fix is `git add`, not an edit
here.

Those URLs also come out of `amp-nav.js`, so **the shared portfolio nav has been
shipping the same five dead links on every property that embeds it.** That is a
change to `AmpersandBoxDesign`, not to this repo.

When real posts exist, add them **above** the masterclasses using the same
`.read` row (number in `.n`, title and standfirst in `.t`, `→` in `.a`) and
change the kicker to `Writing · latest first`.

## Outbound links: what was checked, and what is knowingly dead

Checked by HTTP status 2026-08-25 — **all 200**: `ampersandboxdesign.com`,
`opensentience.org`, `opensentience.org/box-and-box/`, `graphonomous.com`,
`workbench.opensentience.org`, `academy.opensentience.org`,
`docs.ampersandboxdesign.com`, `agentic-engineering-masterclass`, and the T&R
boot-report issue template.

**Knowingly dead, by decision, until deployment:** the eight in the table above.

This page does not claim every link works. It claims every link points where it
should. Those are different promises and only the second one is keepable before
launch — but it does mean **you cannot use "the page has no broken links" as a
pre-push gate here.** Use the checklist instead.

## Deployment

`computedriven.com` is its own repository — `c-u-l8er/computedriven.com` —
deployed by push to Cloudflare Pages. No build step; this directory is the site.

### The T&R page is no longer here

It moved out of this repository entirely on 2026-08-25, to
**[`c-u-l8er/tr.computedriven.com`](https://github.com/c-u-l8er/tr.computedriven.com)**
(public, `main`), which is the sibling directory `../tr.computedriven.com/` in
the workspace.

**That repository carries this one's history.** It was made by cloning
`computedriven.com` at `e4e45fa` rather than starting empty, because the 24
commits up to that point are about the T&R HTML — the fonts, the images, the
beacon, the form — and they explain that page, not this one. This repository's
history continues from the same point; the two share ancestry up to the split,
which is what actually happened.

Consequences here: `img/` is gone (all nine files were T&R screenshots), and
this page's `og:image` points at `https://tr.computedriven.com/img/og-card.jpg`
— **another origin**, which is fine for a crawler and means the card breaks if
that site is ever taken down. Replace it with a card of this page's own the
first time Cloud or Super has a real screen to photograph.

### `tr.computedriven.com` has no DNS record — and this page depends on it

Checked 2026-08-25:

```
getent hosts tr.computedriven.com
```

The T&R links here are absolute (`https://tr.computedriven.com/`, `/#get`,
`/#thesis`) because there is no longer a local `/tr/` to point at. So **until a
Cloudflare Pages project exists with `tr.computedriven.com` attached as a custom
domain, the root's only live door goes nowhere** — T&R is the one product of
three that ships today, and it is the one this depends on.

**T&R is the one to do first.** All three product domains are dead (see the
checklist above), but Cloud and Super are honestly labelled `in dev` — a visitor
who finds those closed learns something true. T&R is labelled
`v0.3 · downloadable`, so a dead link there contradicts the page's own claim
about itself, which is the only one of the three that actually costs credibility.

Check with DNS rather than a browser tab: a 404 from a parked host and a missing
record look identical in a browser and completely different to `getent`.

An earlier draft of this page pointed those links at `/tr/` and shipped the T&R
site as a subdirectory here, which worked without any Cloudflare change at all.
That is no longer the layout, and the interim it protected is gone — the tradeoff
was made deliberately in exchange for the T&R site being its own repository with
its own history, which is the thing that could not be had both ways.
