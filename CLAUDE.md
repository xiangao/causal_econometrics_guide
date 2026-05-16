# Causal Econometrics Guide

## Project Overview
A Quarto book covering causal econometrics with observational data. Structure mirrors the companion Julia book `causal_econometrics_julia`. Topics: identification & potential outcomes (with DAG/ADMG workflow and applied NHEFS case study), estimation (RA, IPW, AIPW, IPWRA), nonparametric methods (IF-based, TMLE, DoubleML), difference-in-differences (including staggered/ETWFE, synthetic control, synthetic DiD), IV & RDD with the control function approach, IV in Poisson, causal mediation analysis, and causal discovery (PC/GES for observed, FCI/RFCI for latent variables).

Converted from RevealJS slides to a multi-chapter Quarto book in March 2026. Synced to match the Julia book's content in May 2026 (added DAG-workflow and smoking-cessation chapters, causal discovery part, R package ecosystem appendix; removed machine-learning chapter to match Julia structure). Further synced in May 2026: added causal-estimands chapter (Identification part) and graph-to-estimate chapter (Causal Discovery part) ported from Julia book.

## Structure
- `_quarto.yml` — book configuration (cosmo theme, chapters, parts)
- `index.qmd` — preamble/landing page
- `identification.qmd` — Identification & Potential Outcomes
- `causal-estimands.qmd` — Five Estimands on One DGP (ATE/ATT/LATE/CATE/QTE simulation)
- `graphs-identification-estimation.qmd` — DAGs/ADMGs, backdoor + Pearl-Shpitser ID, AIPW + front-door estimation (uses dagitty, causaleffect)
- `smoking-cessation-graphs.qmd` — Applied DAG workflow on NHEFS data (causaldata::nhefs_complete)
- `estimation.qmd` — Estimation: RA, IPW, AIPW, IPWRA
- `nonparametric.qmd` — Nonparametric Causal Methods (IF, TMLE, DoubleML)
- `did.qmd` — Difference-in-Differences (ETWFE, Synthetic Control, Synthetic DiD)
- `iv-rdd.qmd` — IV & Regression Discontinuity (with Control Function exposition)
- `poisson-iv.qmd` — IV in Poisson with Fixed Effects (CF, GMM)
- `mediation.qmd` — Causal Mediation Analysis
- `sensitivity-analysis.qmd` — Sensitivity analysis (Cinelli-Hazlett OVB via sensemakr, E-values via EValue, Rosenbaum bounds)
- `matching.qmd` — Matching estimators (MatchIt + cobalt + WeightIt; PS matching, full matching, CEM)
- `continuous-treatments.qmd` — Continuous & multivalued treatments (Hirano-Imbens GPS, Kennedy DR, LMTP via lmtp)
- `bayesian-causal.qmd` — Bayesian causal inference (BART + BCF; BCF wrapped in subprocess due to MCMC stdout encoding issue)
- `survival-causal.qmd` — Survival analysis with causal inference (Cox PH, IPCW, RMST, doubly-robust via riskRegression)
- `heterogeneous-effects.qmd` — Heterogeneous treatment effects with ML (meta-learners + causal forests via grf, BLP, CLAN, policy trees)
- `g-methods.qmd` — G-methods for time-varying treatments (parametric g-formula, IPTW + MSM via survey, LTMLE via ltmle)
- `causal-discovery.qmd` — Causal Discovery: Observed Variables (PC + GES via pcalg)
- `causal-discovery-latent.qmd` — Causal Discovery: Latent Variables (FCI + RFCI via pcalg)
- `graph-to-estimate.qmd` — From Graph to Estimate: backdoor/front-door/non-identified workflows (dagitty, causaleffect, AIPW)
- `package-ecosystem.qmd` — Appendix: R packages used in this book
- `data/` — datasets (CSV, DTA)
- `images/` — PNG images
- `_book/` — rendered HTML output

## Build
```
quarto render        # build the book
quarto preview       # live preview in browser
```

## R Dependencies
ggplot2, ggdag, dagitty, causaleffect, igraph, npcausal, boot, SuperLearner, tmle, tidyverse, ranger, hdm, lmtest, sandwich, haven, grf, policytree, sensemakr, EValue, ltmle, ipw, gfoRmula, survey, MatchIt, cobalt, WeightIt, optmatch, causaldrf, lmtp, BART, bcf, riskRegression, survminer, rpart, glmnet, splines, reshape2, stringr, causaldata, fixest, did, broom, skimr, bslib, etwfe, synthdid, sem, MASS, rdrobust, lavaan, data.table, medoutcon, DoubleML, mlr3, mlr3learners, gmm, parallel, pcalg, Rgraphviz, graph, gridExtra, marginaleffects (Rgraphviz and graph are Bioconductor packages required by pcalg)

## Notes
- Each chapter has a hidden setup chunk (`#| include: false`) loading required libraries
- `execute: freeze: auto` in `_quarto.yml` caches R output; delete `_freeze/` to force re-run
- Data files are in `data/`, images in `images/`
- Source slides: `~/projects/claude/causal_econometrics/code/causal_econometrics.qmd`
