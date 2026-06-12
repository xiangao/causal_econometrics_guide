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

## Cross-book parity with Julia companion

Results are validated to match `causal_econometrics_julia` within 1% (real data) or 5% (simulated).

### Shared datasets (generated in R, loaded by both books)
Chapters that load pre-generated CSVs from `data/` to ensure identical results:
- `survival-causal.qmd` → `data/survival_sim.csv` (n=1500 Weibull; propensity intercept=-4 for ~30% treatment)
- `shift-share-iv.qmd` → `data/shift_share_sim.csv`, `shift_share_shares.csv`, `shift_share_shocks.csv`, `shift_share_bad_v.csv`, `shift_share_bad_noise.csv`
- `causal-discovery.qmd` → `data/pisa_usa2022.csv` — **real** PISA data (USA 2022, 3,890 students; 6 nodes: HISEI, HOMEPOS, IMMIG, GRADE, GENDER, MATH = mean of 10 PVs; survey weight `W`). Generated from `~/projects/pisa-covid-did/output/pisa_pooled.rds` (draft at `~/projects/pisa_discovery_demo/`). **`PARED` is absent in PISA 2022 — do not add it as a node.** The section runs weighted PC + GES, tier-based orientation, and bootstrap stability; deliberately no F1 (real data has no ground truth).

To regenerate the simulated datasets: run `Rscript` with the DGP code at the top of each chapter (seed is set).

### Notable chapter additions (May 2026)
- `did.qmd`: Added **Nonlinear ETWFE** section demonstrating `etwfe(family = "poisson")` for count outcomes. Uses `cgroup = "never"` and simulated staggered count data.
- `causal-discovery.qmd`: Added **Real Data: Student Performance in PISA** section (PC + GES on design-weighted PISA 2022). Mirrored in the Julia book with PC + RSL-D.

## Notes
- Each chapter has a hidden setup chunk (`#| include: false`) loading required libraries
- `execute: freeze: auto` in `_quarto.yml` caches R output; delete `_freeze/` to force re-run
- Data files are in `data/`, images in `images/`
- CI does NOT render: `.github/workflows/publish.yml` just uploads the committed `_book/` to GitHub Pages on push to master. Render locally (one chapter at a time: `quarto render <chapter>.qmd`) and commit `_book/` + `_freeze/` along with sources.
- **knitr-cache trap**: `cache: true` hashes only a chunk's own code. After editing a DGP chunk, downstream chunks with unchanged code serve results computed from the OLD data — delete their entries in `<chapter>_cache/html/` (or the whole cache dir) before re-rendering.
- Source slides: `~/projects/claude/causal_econometrics/code/causal_econometrics.qmd`

## Review pass (2026-06-07)
Math/code audit + fixes across 11 chapters (audit trail: ../_review/). Key corrections:
- g-methods: stated "true" effects 1.0/0.5 were wrong (correct 1.5/1.0; code was right). Verified by hand: E[Y(a0,a1)]-E[Y(0,0)] = 1.0*a0 + 0.5*a1.
- causal-discovery: CPDAG-F1 compared transposed adjacency conventions (0.19 → 0.86 after fixing `amat_from_graph`).
- iv-rdd: compliance types redefined by potential treatments (W(0),W(1)), not observed (Z,W); Wald = LATE not ATE.
- mediation: NDE/NIE/ATE now computed and printed (was blank).
- estimation: linear-RA coefficient relabeled ATT (covariates demeaned at treated means).
- bayesian-causal, heterogeneous-effects: dropped the unobserved confounder U from the propensity → examples now identified; BCF posterior recovers true ATE.
- sensitivity-analysis: rewrote the broken Manski-bounds formula (proper width-K interval); risk-ratio CI now uses robust sandwich SEs.
- nonparametric: ψ=E[Y(1)] (a single arm mean), not "the ATE". identification: two-sided positivity 0<π(X)<1.
- survival-causal: REGENERATED shared data/survival_sim.csv (had zero censoring); added the censoring model the doubly-robust `ate()` requires. Generator: data/gen_survival.R.
Re-rendered + outputs verified.

## Review pass 2 (2026-06-10 → 2026-06-12)
Derivation-focused audit of ALL 20 chapters (audit trail: ../_review2/, fixes itemized in ../_review2/FIXLOG.md; every CRIT/MAJOR hand- or numerically verified before editing). Highest-severity corrections:
- heterogeneous-effects (CRIT): policy-tree example — `double_robust_scores()` returns an n×2 per-arm score matrix, NOT a CATE vector; `cbind(0, scores - cost)` built a meaningless 3-action tree, "treatment rate" counted the control action, and the welfare line recycled a vector against the matrix. Rewritten (cost 2, non-degenerate rule); tree now finds the true X1≈0.5 threshold.
- heterogeneous-effects (MAJOR ×2): panel section — stated "True ATE" omitted the +W main effect (truth said 0.566, DGP implied 1.566), AND unit_fe was independent of treatment so there was no FE confounding to demonstrate. New DGP: W depends on unit_fe; naive/clustered CF biased (~1.7), within-transform recovers truth (0.577 vs 0.566). `clusters=` fixes honesty/SEs, NOT confounding.
- causal-discovery-latent (CRIT): `true_mag_amat` inverted the pcalg amat.pag convention (amat[i,j] = mark AT j) — all directed edges in the "True MAG" panels were drawn backwards. FCI/RFCI cost story rewritten around the Possible-D-SEP stage (what RFCI actually skips; the 10 orientation rules run no CI tests).
- graph-to-estimate (CRIT): "When identification fails" example marked BOTH igraph edges as the latent pair → encoded plain A↔Y (no directed edge) → causal.effect returns "P(Y)" (identified!) while prose claimed it errors. Bow graph rebuilt with make_graph (graph_from_literal dedupes parallel edges); now errors "Not identifiable." Also: pcalg2dagitty needs the TRANSPOSED graphNEL matrix or every edge reverses.
- causal-discovery (MAJOR ×3): α-tradeoff direction was backwards (low α ⇒ SPARSER skeleton); PC-vs-GES callout contradicted the book's own tables (PC wins both metrics here; BIC is score-equivalent — no "directional signal"); PISA misorientation callout attributed GES's GRADE→IMMIG error to PC (PC's actual tier conflicts: HISEI/HOMEPOS→IMMIG).
- mediation (MAJOR): interventional-effects decomposition rewritten to the standard G_0/G_1 randomized-draw form (sum = overall effect ≠ ATE) matching the medoutcon estimand (Díaz et al. 2021, verified against package docs).
- g-methods (MAJOR): stabilized-weight display conditioned the denominator on L̄_{t−1} (skipping L_t, THE time-varying confounder); code was always right. Untrimmed MSM added: trimming reintroduces confounding bias (1.652 vs 1.560, truth 1.5).
- continuous-treatments (MED): lmtp styled print emits space-padded lines → Quarto renders literal `::: {.cell-output}` paragraphs; display via tidy()/$estimates instead.
- survival-causal: ate() three-estimator table added (G-formula/IPTW/AIPTW: −0.106/−0.095/−0.099 — "agreement" now visible). package-ecosystem/references: npcausal+medoutcon are GitHub-only; igraph (not Rgraphviz) does the plotting; references page can't show the full bib (chapter-level nocite ignored in HTML books).
