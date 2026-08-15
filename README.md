# Introduction to Causal Econometrics with Observational Data

A Quarto book covering causal inference methods for observational data, by Xiang Ao (HBS). Mirrors the structure of the companion Julia book `causal_econometrics_julia`.

## Topics

1. **Identification** — potential outcomes, ATE/ATT/ATU, unconfoundedness, overlap, SUTVA; DAGs/ADMGs, backdoor and Pearl–Shpitser identification; applied NHEFS smoking-cessation case study
2. **Estimation** — regression adjustment, IPW, AIPW, IPWRA; influence-function-based methods, SuperLearner, TMLE, DoubleML
3. **Designs** — Difference-in-Differences (TWFE, staggered/ETWFE, Goodman–Bacon, synthetic control, synthetic DiD); IV and regression discontinuity (LATE, control function, sharp/fuzzy RDD); IV in Poisson with fixed effects (continuous and binary endogenous variables)
4. **Mediation** — classical mediation, CDE, natural effects (NIE/NDE), interventional effects (IIE/IDE)
5. **Causal Discovery** — PC and GES for observed-only systems; FCI and RFCI when latent confounders are present; a survey-weighted real-data example on PISA 2022 (background-knowledge tier orientation, bootstrap edge stability)
6. **Appendix** — R package ecosystem map

## Build

```bash
quarto render        # build the book
quarto preview       # live preview
```

CI auto-publishes to GitHub Pages on push to `master` via `.github/workflows/publish.yml`.

## Requirements

R with the following CRAN packages: `ggplot2`, `ggdag`, `dagitty`, `causaleffect`, `tidyverse`, `haven`, `causaldata`, `hdm`, `SuperLearner`, `tmle`, `DoubleML`, `mlr3`, `mlr3learners`, `did`, `etwfe`, `fixest`, `synthdid`, `sem`, `MASS`, `rdrobust`, `gmm`, `lavaan`, `pcalg`.

Bioconductor packages (required by `pcalg`): `graph`, `RBGL`, `Rgraphviz`. Install once via:

```r
install.packages("BiocManager")
BiocManager::install(c("graph", "RBGL", "Rgraphviz"))
install.packages("pcalg")
```

GitHub-only packages:

```r
remotes::install_github("ehkennedy/npcausal")
remotes::install_github("nhejazi/medoutcon")
```

> **2026-06-07:** Math/code review pass — see `CLAUDE.md` (Review pass section) for the list of corrections. Audit trail in `../_review/`.
>
> **2026-06-12:** Second review pass (derivation-focused, all 20 chapters; audit trail in `../_review2/`, fixes in `../_review2/FIXLOG.md`). Highlights: repaired policy-tree example (3-action score-matrix bug), panel causal-forest DGP (no actual FE confounding + wrong stated truth), interventional-effects decomposition now matches the `medoutcon` estimand, stabilized-weight display, PC α-tradeoff direction, True-MAG endpoint encoding (arrows were reversed), and the non-identifiability `causaleffect` example (was identifiable as coded).

> **2026-06-13:** Technical-audit fix pass (Codex audit in `../_technical_audit_20260613/`). Fixed sharp-RD DGP sign (jump now on the treated side; estimate ≈ +2, verified); rewrote the brittle RMST KM-integration helper; strengthened mediation natural-effect assumptions and labeled CDE IPW as discrete-mediator-only; defined the panel causal-forest within estimand precisely; added AIPW SE/cross-fitting caveats; corrected "doubly robust" phrasing and the nonparametric plug-in critique; qualified continuous-treatment DR; separated IV conditional-independence from exclusion; renamed trimmed/untrimmed IPW weight columns; hardened the BCF subprocess fallback; clarified the CPDAG-F1 orientation-only metric, the simplified latent-MAG oracle, and the PISA design approximation; removed `mean()` around scalars. Fixed pre-existing version drift exposed on re-render (rdrobust 4.0.0 dropped `all=`; fixest IV no longer allows the running variable in both endogenous and instrument lists). Corrected the `renv`-pinned claim in the preface. Rendered clean.

> **2026-07-30:** Review of the 2026-07-28 `poisson-iv` rewrite (report in `../_review3/review_20260730.md`). The mathematics checked out — Terza's exact correction, the first-order Taylor expansion behind Approach B, the logit generalized-residual collapse, and the claimed probability limits were all re-derived or re-verified at n=10^6. Four corrections: all six citations to Imbens & Wooldridge were re-pointed from the 2009 *JEL* paper to their actual source, the 2007 NBER Summer Institute Lecture Notes 6 on control functions (new bib entry); a truncated quote was restored to include "and still adopt (3.25)", which had reversed its meaning; rho is now defined as the coefficient in E(exp(c1)|v2) = exp(rho*v2) rather than as a correlation (the chapter's own DGP has correlation 1 while rho = 0.2); and two leftover passages recommending Approach A were reconciled with the takeaway's recommendation of B. Approach C is now presented as a bias-variance tradeoff rather than a ranking: Mullahy's moments are consistent here (0.8015 at n=10^6), but at the chapter's n=5000 C is roughly three times as noisy as B and so has the worst RMSE of the three.

> **2026-08-15:** Simulation write-up pass across all 20 content chapters. Every chunk now has prose before it saying what it does and — for simulated data — the complete DGP (sample size, every distribution, the treatment and outcome equations, the true parameter), and prose after it saying what the output shows. Real-data chapters describe the sample in place of a DGP. No DGP, estimator or result was changed, apart from two fixes: the BCF subprocess in `bayesian-causal` set no seed (so its posterior moved on every real execution, hidden by the knitr cache — a seed in the parent session does not reach a separate R process, so `set.seed(42)` now sits inside the `Rscript` call), and the `adjustmentSets` calls on non-identified graphs in `graphs-identification-estimation` and `graph-to-estimate` printed literally nothing, which is indistinguishable from a failed chunk; they now report the count. Where a chapter printed estimates with no benchmark, the truth was derived analytically from the DGP and added: the Weibull survival risk difference ($-0.108$) and RMST difference (0.271), the mediation $CDE(0) = 0.222$ / $NDE = 0.218$ / $NIE = -0.008$, the LMTP policy means (0.125 and 1.125, contrast exactly 1), the staggered-DiD true ATT (3.677, against ETWFE's 3.67), and the shift-share DGP itself, which existed only inside the stored CSVs. Six prose/output mismatches were corrected — most notably that the continuous-treatment chapter's "naive regression ignores confounding" is false for its own DGP (the bias is exactly zero by cancellation), that the shift-share claim "the industry IVs are all near the true effect" contradicts a range of $-39.5$ to $2.6$ (harmless, and the algebra shows why), and that the IV/RDD chapter's "these two results are very different" does not survive `rdrobust`'s standard error of 0.179. Full itemisation in `CLAUDE.md`.

> **2026-08-12:** Prose tightening pass across 4 foundational chapters (`identification`, `estimation`, `did`, `iv-rdd`) — compressed wordy openings, switched to "we" voice, tightened explanations. Mirrored in the Julia companion. No code or results changes.

> **2026-07-30 (deep read):** Full-depth pass over every topic, reviewed **paired**
> against the companion book so each acts as the other's control (log:
> `../_review3/deepread_causal_books.md`). The pairing is what found most of the
> errors; the recurring defect is a correction that landed in one book, chapter, or
> section and not its twin.
>
> Corrections here: the synthetic-DiD summary printed an unexplained `NA` standard
> error (the jackknife is undefined with a single treated unit — California — so a
> placebo SE was added, and the resulting interval includes zero); the
> continuous-treatment dose-response `1 + 2d - 0.15d^2` was described as
> "hump-shaped ... falls at high doses" when its vertex at 6.67 lies beyond the
> largest dose observed; the policy-tree prose claimed a single threshold near 0.5
> where the printed depth-2 tree yields a non-monotone rule; "True_Psi" in
> `nonparametric` was a realized-sample mean whose own sampling SD (0.020) rivals
> the estimator SEs, replaced by the exact population ATE 0.202785; the front-door
> estimate in `graphs-identification-estimation` had no truth to check it against
> (derived analytically: 0.0547); and the section titled "Comparing matching to
> weighting" contained no comparison — the two estimates are exactly one standard
> error apart, with *p* flipping 0.005 to 0.098 while the *better*-balanced method
> gives the smaller estimate.
