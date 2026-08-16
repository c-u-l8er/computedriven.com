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

## Images are WebP, and the one JPEG is deliberate

**Measured 2026-08-16: `img/` was 11.43 MB, and 8.08 MB of it was the greeter
alone** — a 5120×2880 PNG of stylised artwork, which is the wrong codec for that
content by an order of magnitude. The head was advertising 53,720 bytes of
self-hosted fonts on a page shipping eleven megabytes of screenshots.

Everything is WebP q90 now. **1.13 MB, down 90.1%**, and the conversion was
checked rather than assumed: RMSE against the PNGs is 0.4–0.6%, and the greeter's
darkest gradient — where banding would show first on a page this dark — was
compared crop-to-crop at 1:1. Lossless WebP was measured too and only reaches
~55–63% of PNG, so the saving is genuinely in the lossy step.

**`img/og-card.jpg` is the exception and must stay a JPEG.** Link-preview
crawlers are the one client that cannot be assumed to decode WebP, and a card
that fails to render is the entire reason `og:image` exists. It is the hero
frame at 59 KB. **Regenerate it whenever the hero image changes** — nothing
checks that it matches.

If you drop a new screenshot in, convert it:

```
magick img/whatever.png -quality 90 -define webp:method=6 img/whatever.webp
```

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

**The hero changed on 2026-08-16** from `tandr-three-apps.png` (three native
GTK applications on the road) to `tandr-runefort.png` (a RuneFort layout as a
pane), with `tandr-bendscript.png` replacing the `foot` terminal close-up.
The two protocols now render inside the shell and that is the newer claim.

**`tandr-three-apps.png` was NOT deleted — it moved into `#vehicle`.** Three
paragraphs there are evidence-free without it, and "unmodified native GTK
applications, on the road, with no `/dev/dri`" is the hardest thing on this page
to fake. If you are tempted to remove it, read the section first: it is the
photograph that argument is about, and the `og:image` used to point at it.

The trade is worth stating because it is a real loss: the three-apps frame is
the better *"what the hell is this"* image, and the hero now leads with a claim
that reads as a normal UI until you notice it is standing on a road. That was
Travis's call, 2026-08-16.

### `tandr-three-apps.png` was retired, not re-shot — 2026-08-16

Its right mirror read *"C · CAMERA and R · REEL are not built"*. **The reel
shipped**, so the page was publishing a frame that contradicted the ladder two
sections above it. It was deleted rather than annotated, and `#vehicle` now
carries `tandr-papers.png` — the same shell on the day the page was edited,
carrying two `.bend` documents where the applications stood.

**The native-application claim survives as a dated sentence with no photograph**,
which is how this page treats every other claim it cannot currently show. Do not
quietly restore a picture for it; either re-shoot one or leave the date.

`tandr-reel.png` was added to `#tracks` for the same reason: the ladder marks
TRACK and REPLAY `live_local` and there was no picture of the instrument that
proves it. It shows **one** track. A busier reel is a better picture and exactly
the same claim — if you replace it, keep the caption's "this shell has one track
on it" honest to whatever the new frame shows.

**A current native-application photograph is still wanted, and is blocked by two
separate faults found on 2026-08-16:**

1. **`RRABBIT/proxy/applications.json` points `/tandr-tr4-foot` at ssh port
   `2223`. The running `tr4` guest forwards `2224`.** Nothing listens on 2223 —
   `ssh -p 2223` is refused, `ssh -p 2224 -i PARKVPS/var/parkvps_ed25519
   park@127.0.0.1` answers `tr4` and has both `foot` and
   `/usr/local/bin/waypipe-tandr`. So the entry cannot launch as written. That
   file belongs to the shell lane, not to this site, so it was not edited.
2. **The proxy accepts the launch and spawns nothing.** `__launch(...)` returns
   without error, no `waypipe` process appears, and the console shows only
   `Session created`. This matches the documented stale-session trap in
   `RRABBIT/docs/RUNBOOK.md` — a proxy session outlives the page, and the fix is
   to **restart the proxy AND reload**. The proxy on 8912 belongs to another
   session and was left alone.

Also seen while trying: the shell took a **shader compile error → WebGL context
lost** on one load (`An error occurred compiling the shaders`), which is a third
distinct fault from the two `__whyNoContext()` names. A clean browser cleared it.

**To finish this:** restart `npm run proxy`, fix the port in `applications.json`,
`__launch('/tandr-tr4-foot')` two or three times, and shoot at 1920×1080 with the
idle brake fed. Then delete this section.

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
