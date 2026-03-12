# Introduction to Causal Econometrics with Observational Data

A Quarto book covering causal inference methods for observational data, by Xiang Ao (HBS).

## Topics

1. **Identification & Potential Outcomes** — confounders, potential outcomes framework, ATE/ATT/ATU, unconfoundedness, overlap, SUTVA
2. **Estimation: RA, IPW, AIPW** — regression adjustment, inverse probability weighting, doubly robust estimators, IPWRA
3. **Machine Learning for Causal Inference** — penalized regression (LASSO, Ridge), trees, random forests
4. **Nonparametric Causal Methods** — influence function estimators, SuperLearner, TMLE, DoubleML
5. **Difference-in-Differences** — TWFE, staggered treatment (ETWFE), Goodman-Bacon decomposition, synthetic control, synthetic DiD
6. **IV & Regression Discontinuity** — instrumental variables, LATE, sharp RDD, fuzzy RDD
7. **Causal Mediation Analysis** — classical mediation, CDE, natural effects (NIE/NDE), interventional effects (IIE/IDE)

## Build

```bash
quarto render        # build the book
quarto preview       # live preview
```

## Requirements

R with packages: ggplot2, ggdag, npcausal, SuperLearner, tmle, tidyverse, grf, glmnet, etwfe, synthdid, rdrobust, lavaan, medoutcon, DoubleML, and others (see CLAUDE.md for full list).
