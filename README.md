# computedriven.com

The homepage of ComputeDriven, a systems studio.

One static page, no build step, no npm. `index.html` carries its own CSS and a
little vanilla JavaScript; `img/` holds the screenshots. The portfolio nav is
loaded from `/amp-nav.js`, which is deployed by `sync-nav.sh` in the [&]
workspace rather than edited here.

It has exactly **one third-party runtime dependency**, and it is worth naming
rather than burying: the contact form posts to Formspree. Nothing else on the
page calls out at load, and the page still renders and downloads work with
`formspree.io` blocked — only the form stops.

**That sentence was false until 2026-08-16 and had been for as long as it
existed.** The head carried a `fonts.googleapis.com` stylesheet, so every
visitor made a request to Google that logged their IP before a single word
rendered — on a page whose hero says *no telemetry* and whose headline product
feature is *no first-boot download*. The fonts are self-hosted now:
`fonts/space-grotesk.woff2` and `fonts/jetbrains-mono.woff2`, latin subset,
**53,720 bytes for both**. Verified with `performance.getEntriesByType('resource')`
— zero entries outside the origin.

One thing to know before touching them: **each file is a variable font covering
the whole 400–700 range.** Google served byte-identical woff2 for every weight
requested (checked with sha256 across 400/500/600/700), so there is one file per
family and the `@font-face` declares `font-weight: 400 700`. Declaring a single
weight there would silently flatten every bold on the page. Measured after the
change: at 64px, weight 700 renders 1.50× the ink of weight 400 in Space Grotesk
and 1.31× in JetBrains Mono, so the axis is genuinely live.

## The argument the page makes

Data driven keeps the result. Compute driven ships the derivation.

> A claim is compute driven when the artifact carries enough — canonical input,
> executable semantics, a derived identity and its provenance — for an
> independent machine to derive the claim again.

Re-derivation proves fidelity, not correctness. A wrong computation re-runs
perfectly and is still wrong.

## What it links to

The first proof object is [T&R](https://github.com/c-u-l8er/travel-and-rrabbit),
a FreeBSD 15 distribution assembled from pkgbase. Images are published as
GitHub releases on that repo; this page links to them directly and quotes their
sha256 sums.

## Boot reports

The `#join` section exists because the portfolio nav's CTA reads "Join
ComputeDriven" and there was nothing here to join. What it offers is not a
mailing list. The page's last admitted gap — every measurement was taken on our
machines — is the one gap we cannot close ourselves, so joining is defined as
supplying the missing thing: a second machine.

Reports arrive as GitHub issues on
[travel-and-rrabbit](https://github.com/c-u-l8er/travel-and-rrabbit/issues?q=label%3Aboot-report),
filed through `.github/ISSUE_TEMPLATE/boot-report.yml` in that repo.

**The two counts in `.jn-count` are hand-maintained.** Nothing on this page
fetches at runtime, so `Reports received` and `From real hardware` are stale by
construction. Change them in the same commit that changes the date beneath
them, and count only what is actually filed — a page that inflates its own
evidence count has lost the whole argument. When the first report from real
hardware lands, the "Never booted on real hardware" gap card must change too.

## The contact form

Under the boot-report grid, posting to `https://formspree.io/f/xaewoadr`. **No
email address appears anywhere on this page** — that is the point of using an
endpoint, so do not reintroduce a `mailto:` as a "fallback".

It is a real `<form>` with `action` and `method`, so it works with scripting
off; the script only upgrades it to an inline reply instead of a redirect to
Formspree's own thank-you page. `_gotcha` is Formspree's honeypot and is
positioned off-screen rather than `display: none`.

**"Sent" is printed only on an actual 2xx from the endpoint.** A form that
thanks you on submit and drops the message is precisely the failure this site
is about. Failures print the reason the endpoint gave, and the typed message
survives so it can be retried. If you touch that handler, keep the three paths
honest: server error, network failure, success.

## Screenshots

`img/` holds only figures that show the current shell. The 2026-08-09 batch —
`m2-flat`, `m3-tubes`, `m4-overview`, `m8-native-flat`, `tandr-rrabbit-boot` —
was **deleted on 2026-08-16**, not merely unlinked. Four were already off the
page; `m3-tubes.png` was still the third figure in `#aboard` and showed the
concentric rainbow test pattern in windows with none of the instrument styling
the shell now has. A figure that makes the software look more experimental than
it is costs the same credibility as one that flatters it.

Every figure states its date, machine and rendering path in its caption. The
one exception is `tandr-greeter.jpg`, whose background art has **no recorded
provenance**, and the caption now says so rather than leaving it to be asked.

The gate pair (`tandr-gate-enter.png` / `tandr-gate-exit.png`) is meant to be
read side by side — blue panels are what you can do on arriving, green are ways
out, and the counter moves `1:0-2` → `1:2-2` between them. Keep them the same
crop size or the comparison stops working.

**Taking new ones:** drive the shell from source (`npm run m0`, port 8911) with
`window.__op({op:'park', road:'<lane>', z:<n>})` — entrance gantries sit at
`z=-180`, exits at `z=-2140`, and `__gantry()` reports both. **Feed the idle
brake while you shoot** (`setInterval(()=>window.__fed(), 120)`): without it the
FRAME gauge reads ~100 ms/frame, which is the brake and not the shell, and
publishing it would be a false claim in the unflattering direction. Awake, it
reads 16.7.

## Editing rules

Every status on the page is measured, sourced, or marked *not built*. If you
change a number, change the measurement record under it in the same commit —
including the date and the machine. If a claim gets weaker, say so; the
"Untested & unfinished" section is a feature, not a backlog.
