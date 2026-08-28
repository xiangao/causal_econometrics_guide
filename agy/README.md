# agy's review and rewrite of the Poisson-IV chapter (2026-08-28)

Kept for provenance. These files are **not** part of the book build.

- `poisson-iv-review.md` — review of the pre-existing chapter.
- `poisson-iv-revised.qmd` — agy's rewrite (08:25 version). This is the basis of
  the current `../poisson-iv.qmd`, after the corrections listed below.
- `poisson-iv-revised.html`, `poisson-iv-revised_files/` — agy's standalone render.

## Corrections made before installing it as the chapter

Substantive:

1. **Deleted the "bias < 2% for |rho1| <= 0.3" claim** (§4 reason 5, §6 step 3, and
   the practical-strategy thresholds). Measured at n = 2e6 with a correctly
   specified probit and c1 = rho1*v2 exactly, so Terza's h is exactly right:

   | rho1 | generalized CF | bias | exact | bias |
   |---|---|---|---|---|
   | 0.1 | 0.8029 | 0.36% | 0.8015 | 0.18% |
   | 0.2 | 0.8076 | 0.95% | 0.8005 | 0.06% |
   | 0.3 | 0.8199 | 2.48% | 0.8032 | 0.41% |
   | 0.4 | 0.8320 | 4.00% | 0.7999 | -0.02% |
   | 0.5 | 0.8515 | 6.44% | 0.7993 | -0.09% |

   The claim fails at the boundary it names (2.48% at rho1 = 0.3), and in the
   chapter's earlier binary DGP the generalized CF plim was 0.821 at rho1 = 0.2,
   i.e. 2.7%. The error size depends on how far the probit index reaches into the
   tail, so it is design-specific and no numerical threshold is stated.
2. **§3.1 joint normality.** Joint normality of (c1, v2) delivers e1 ⟂ v2, not
   v2 ⟂ z. The two restrictions are now kept apart.
3. **§4 reason 4 mechanism.** "Overfit" replaced by misspecification bias in h's
   functional form; one scalar parameter cannot overfit.
4. **§3.5 attribution.** Multiplicative quasi-differencing attributed to
   Chamberlain (1992), Wooldridge (1999) and Windmeijer (2000), not Wooldridge
   (2010). Bib entries added to `../references.bib`.

Code:

5. **Stata bootstrap bug.** `mean_z`/`mean_x` were computed outside the bootstrap
   program, so with `idcluster(new_id)` every replication reused Mundlak means
   built on the original panel. They are now rebuilt on the resampled clusters
   inside the program, which also now absorbs `new_id`.
6. **R demo** now reports the true values and a naive baseline, and says
   explicitly that one n = 250 draw is a syntax template, not evidence.

Book integration:

7. Standalone YAML front matter replaced by an `# ` chapter heading so the file
   inherits the book's html+pdf format.
8. All 21 citations restored to `references.bib` keys; agy's version had none.
9. Equation labels renumbered into order of appearance.
10. Unused `library(MASS)` / `library(tidyverse)` dropped; garbled Mundlak
    step 3 rewritten.
