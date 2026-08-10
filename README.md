# computedriven.com

The homepage of ComputeDriven, a systems studio.

One static page, no build step, no dependencies. `index.html` carries its own
CSS and a little vanilla JavaScript; `img/` holds the screenshots. The portfolio
nav is loaded from `/amp-nav.js`, which is deployed by `sync-nav.sh` in the
[&] workspace rather than edited here.

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

## Editing rules

Every status on the page is measured, sourced, or marked *not built*. If you
change a number, change the measurement record under it in the same commit —
including the date and the machine. If a claim gets weaker, say so; the
"Untested & unfinished" section is a feature, not a backlog.
