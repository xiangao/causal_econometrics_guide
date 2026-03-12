# Causal Econometrics Guide

## Project Overview
A Quarto book covering causal econometrics with observational data. Topics: identification & potential outcomes, estimation (RA, IPW, AIPW, IPWRA), machine learning for causal inference, nonparametric methods (IF-based, TMLE, DoubleML), difference-in-differences (including staggered/ETWFE, synthetic control, synthetic DiD), IV & RDD, and causal mediation analysis.

Converted from RevealJS slides to a multi-chapter Quarto book in March 2026.

## Structure
- `_quarto.yml` — book configuration (cosmo theme, chapters, parts)
- `index.qmd` — preamble/landing page
- `identification.qmd` — Identification & Potential Outcomes
- `estimation.qmd` — Estimation: RA, IPW, AIPW
- `machine-learning.qmd` — Machine Learning for Causal Inference
- `nonparametric.qmd` — Nonparametric Causal Methods (IF, TMLE, DoubleML)
- `did.qmd` — Difference-in-Differences (ETWFE, Synthetic Control, Synthetic DiD)
- `iv-rdd.qmd` — IV & Regression Discontinuity
- `mediation.qmd` — Causal Mediation Analysis
- `data/` — datasets (CSV, DTA)
- `images/` — PNG images
- `_book/` — rendered HTML output

## Build
```
quarto render        # build the book
quarto preview       # live preview in browser
```

## R Dependencies
ggplot2, ggdag, npcausal, boot, SuperLearner, tmle, tidyverse, ranger, hdm, lmtest, sandwich, haven, grf, rpart, glmnet, splines, reshape2, stringr, causaldata, fixest, did, broom, skimr, bslib, etwfe, synthdid, sem, MASS, rdrobust, lavaan, data.table, medoutcon, DoubleML, mlr3, mlr3learners

## Notes
- Each chapter has a hidden setup chunk (`#| include: false`) loading required libraries
- `execute: freeze: auto` in `_quarto.yml` caches R output; delete `_freeze/` to force re-run
- Data files are in `data/`, images in `images/`
- Source slides: `~/projects/claude/causal_econometrics/code/causal_econometrics.qmd`
