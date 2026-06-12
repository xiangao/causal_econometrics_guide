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
