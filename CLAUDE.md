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

## Review pass (2026-07-30)
Review of the 2026-07-28 rewrite of `poisson-iv.qmd` (10 commits), the only chapter changed since the 2026-07-04 agy-review fix pass. Report: `../_review3/review_20260730.md`. Identical findings and fixes applied to both this book and its R book counterpart.

**The mathematics was all correct** and was left alone. Re-derived independently before touching anything: Terza's $h(\cdot)$ from the truncated-normal moments $E[e^{\rho v}\mid v \ge -z\pi] = e^{\rho^2/2}\Phi(z\pi+\rho)/\Phi(z\pi)$; the Taylor expansion ($\partial_\rho e^{\rho^2/2}|_0 = 0$, $\partial_\rho[\Phi(\rho+\eta)/\Phi(\eta)]|_0 = \lambda(\eta)$, $\partial_\rho\{[1-\Phi(\rho+\eta)]/[1-\Phi(\eta)]\}|_0 = -\lambda(-\eta)$), hence Approach B as the first-order-in-$\rho$ approximation with $O(\rho^2)$ error; and the logit collapse $y_2\lambda/\Lambda - (1-y_2)\lambda/(1-\Lambda) = y_2 - \Lambda$. Claimed plims confirmed at $n=10^6$ (A 0.8383, B 0.8212 against the stated 0.839/0.822) and the additive-moment figure is exactly 0.6279 against "≈0.63".

**Citation provenance (the main finding).** All six `@imbens-wooldridge-2009-jel` citations pointed at the wrong document. Equations (3.23)–(3.25), (3.32), (3.34)–(3.36), "sec. 3.3", the $h(\cdot)$ formula and the Terza block quote are all from **Imbens & Wooldridge (2007), NBER Summer Institute Lecture Notes 6, "Control Functions and Related Methods"** (downloaded and text-matched; page 20 carries the quote verbatim), not from the 2009 *JEL* program-evaluation paper. New bib key `@imbens-wooldridge-2007-cf` added to `references.bib`; all six re-pointed. The 2009 JEL entry is now unused but kept.
- The quote had been truncated immediately before **"and still adopt (3.25). In fact, we assume $(r_1,v_2)$ is jointly normal."** — which reverses its sense, since (3.25) (independence from $\mathbf z$) is *re-asserted about the latent $v_2$*, not discarded. Restored, with a gloss.
- The unverifiable "Estimate P(W=1|x,z) by probit (or logit)…" quote was replaced with the verbatim lecture-notes wording; the probit-or-logit point is now derived (a logit fitted probability is just another function of $\mathbf z$) rather than attributed.

**$\rho$ is a coefficient, not a correlation.** The chapter defined $\mathrm{Corr}(c_1,v_2)=\rho$, but $h(\cdot)$ requires $\rho$ from $E(\exp(c_1)\mid v_2)=\exp(\rho v_2)$ — that is what makes $e^{\rho^2/2}$ the lognormal correction and puts $\rho$ on the probit index's scale. The chapter's own DGP settles it: $c_1 = 0.2 e_2$ with $v_2 = e_2$, so the correlation is exactly **1** while $\rho = 0.2$.

**A-vs-B contradiction.** The Approach-C callout said "Approach A … is the practical choice when many fixed effects are present" while the rewritten takeaway said to use B and treat A as a fallback. Both A and B run unmodified in FE-Poisson (only the first stage differs), so the callout now says so and prefers B.

**Approach C: state it as a bias-variance tradeoff, not a ranking.** Mullahy's multiplicative moments are genuinely *consistent* for this DGP — the omitted `ad`/`female` effects are independent multiplicative heterogeneity, absorbed by the intercept ($\beta_0 \to \beta_0 + \log E[e^c]$) — verified at $n=10^6$: 0.8015, sd 0.0013. **But consistency is not accuracy at a given $n$.** At the chapter's own $n=5000$ over 10 seeds: A mean 0.8323 / sd 0.0236 / RMSE 0.0393; B 0.8185 / 0.0262 / 0.0310; C 0.7985 / 0.0679 / 0.0645. C is nearly unbiased and yet has the **worst RMSE**, being ~3× noisier without the fixed effects. My first draft of this fix said "prefer Approach C" and had to be corrected — the rendered output refuted it. Its comparison paragraph already showed C closest (0.7844 vs A 0.8613, B 0.8544), so the paragraph now explains why that is a bias story with a variance cost.

## Prose tightening pass (2026-08-12)

Voice and concision edit across 4 foundational chapters shared with the Julia companion: `identification.qmd`, `estimation.qmd`, `did.qmd`, `iv-rdd.qmd`. Compressed wordy openings and multi-sentence explanations into shorter paratactic prose, switched to "we" voice. 19 insertions, 25 deletions. Mirrored identically in `causal_econometrics_julia`. Source committed, rendered, pushed.

## Simulation write-up pass (2026-08-15)

Applied the standing convention to **every** chapter: before each chunk, prose
stating what it is for, what the variables are, and — for simulated data — the
full DGP (sample size, every distribution, the treatment and outcome equations,
the true parameter). After each chunk, prose stating what the output shows.
Real-data chapters get a description of the sample in place of a DGP. No DGP,
estimator or result was changed; the only code edits are the two reproducibility
/ blank-output fixes listed at the end of this section. Every number quoted in
the new prose was checked against the `cell-output` block that produced it, and
every claimed "true" value was re-derived from the DGP.

**Truths derived and added where the chapter had none.** Several chapters
printed estimates with nothing to judge them against. Derived by integration
over the known DGP, not by simulation:
- `survival-causal`: from the Weibull PH DGP, $(\lambda_0 t)^k = 0.43302$ at
  $t=5$, giving a true risk difference of $-0.1084$ (estimates $-0.106$ /
  $-0.095$ / $-0.099$) and a true RMST difference of 0.2709 (unadjusted 0.203,
  IPW-adjusted 0.28).
- `mediation`: true $CDE(0) = 0.222$, $CDE(1) = 0.191$, $NDE = 0.218$,
  $NIE = -0.008$, $ATE = 0.211$.
- `continuous-treatments`: LMTP truths follow from the DGP —
  $E[Y(0,0)] = E[L_2] = 0.125$, $E[Y(1,1)] = 1.125$, contrast exactly 1.
- `did`: the staggered `.dta` carries the true unit-level effects (`te4`/`te5`/
  `te6`), so ETWFE can be scored — true overall ATT 3.677 against an estimate
  of 3.67, with event-time (3.11/3.81/4.58) and calendar (3.76/3.49/3.80)
  breakdowns all covered. Poisson ETWFE truth is
  $E[\mu_0 \mid \text{treated}] \times (e^{0.4}-1) = 7.649 \times 0.4918 = 3.762$.
- `shift-share-iv`: the DGP was only in the CSVs; reverse-engineered and
  verified ($B = S g$ to 1e-15), and the OLS bias is
  $\text{Cov}(X,u)/\text{Var}(X) = 0.3/0.315 = 0.95$, so OLS = 1.45 exactly as
  observed.

**Prose/output mismatches corrected** (the recurring defect, as in the deep read):
- `continuous-treatments`: "a linear regression of Y on D ... ignores confounding
  by X" — the confounding bias is **exactly zero** in that DGP. $X_1, X_2$ each
  raise $D$ by 0.5 but enter $Y$ with $+0.3$ and $-0.3$, so
  $0.3(0.5) - 0.3(0.5) = 0$. Naive slope 1.421, adjusted 1.409. Only functional
  form is at fault, and the common slope ≈ average true slope over observed doses.
- `shift-share-iv`: "the industry-specific IVs are all near the true effect" —
  they range $-39.5$ to $2.6$. Harmless, and the algebra says why:
  $\alpha_k\beta_k = g_k\text{Cov}(s_k,Y)/\text{denom}$, so $\text{Cov}(s_k,X)$
  cancels and an exploding $\beta_k$ always pairs with a near-zero weight.
- `matching`: love-plot prose would have implied all covariates improve — `educ`
  goes 0.055 → 0.096. Its `fig-cap` also named two methods while plotting one.
- `iv-rdd`: the closing "these two results are very different" — 2SLS 0.170 vs
  `rdrobust` 0.096, but `rdrobust`'s SE is 0.179, so the two are statistically
  indistinguishable. Rewritten as a precision difference, not a contradiction.
- `iv-rdd`: stated true LATE 0.60, output says 0.615.
- `bayesian-causal`: `fig-cap` asserted BCF gives "tighter, less noisy CATE
  estimates"; the ATE posterior SDs are 0.046 vs 0.048 and the ITE claim is not
  checkable from the output. Made descriptive.

**New findings worth keeping:**
- `matching`: just-identified CBPS and entropy balancing return **numerically
  identical** control weights (max diff 3e-12, ESS 98.4578 both). Both solve
  exact mean balance with weights of the form $\exp(x'\lambda)$. The chapter
  had presented them as unrelated methods with coincidentally identical tables.
- `causal-discovery`: the MC section produced a table and interpreted nothing.
  It is the correction to the single-dataset result — PC beats GES 0.952 to
  0.800 on one draw, but 0.947 to 0.933 averaged over 100.
- `g-methods`: `ltmle(abar = c(1,1))` returns $E[Y(1,1)] = 1.5106$, a
  counterfactual **mean**, not an effect — and the true effect is also 1.5
  because $E[Y(0,0)] = 0$ here. Easy to misread as a recovered effect. Also,
  the control arm prints $p < 2\times10^{-16}$ beside an interval containing zero.
- `mediation`: the three CDE estimates (0.2516/0.2553/0.2554) all sit ~0.03
  above the true 0.222. MC over 400 draws shows the estimator is unbiased (mean
  0.2206, sd 0.0159) — this seed is 1.9 SD high. Estimators agreeing with each
  other is a check on implementation, not on sampling error.
- `causal-discovery-latent`: hiding 2 of 8 nodes takes the graph from 11 edges
  over 8 nodes to 10 edges over 15 possible pairs — densification is the cost
  of latent confounding, and why F1 drops from 0.952 to 0.824.

**Two code fixes (the only non-prose changes in this pass):**
- `bayesian-causal`: the BCF subprocess set **no seed**, so its posterior draws
  changed on every real execution; the knitr cache hid this, and the numbers
  would have moved the first time anyone cleared it. A seed set in the parent
  session does *not* reach a separate R process, so `set.seed(42)` now goes
  **inside** the `Rscript -e` string. Verified: two independent seeded worker
  runs agree to 2.6e-13 (floating-point, not RNG divergence) with identical ATE
  means. Re-running changed the BCF figures from 1.982 / 0.046 / [1.897, 2.072]
  to **1.984 / 0.044 / [1.903, 2.072]**; the prose was updated to match. BART
  was already seeded and did not move.
  - When re-running this, delete `bayesian-causal_cache/html/bcf-fit_*`,
    `bcf-summary_*` **and** `compare-ite_*`. The latter two have unchanged code
    but consume `bcf_fit`, so they would otherwise serve results computed from
    the old draws — the knitr-cache trap noted above.
- `graphs-identification-estimation` and `graph-to-estimate`: `adjustmentSets`
  on a non-identified graph returns an empty list, which prints **literally
  nothing** — indistinguishable from a chunk that failed. Both chunks now assign
  the result and `cat()` its length, printing `Valid backdoor adjustment sets: 0`.

## Deep read (2026-07-30) — reusable gotchas

Full-depth pass over all 24 chapters, paired against the Julia companion. Log:
`~/projects/books/_review3/deepread_causal_books.md`.

### `synthdid`: the default SE is undefined with one treated unit

`summary(tau.hat)` on California Prop 99 returns `se = NA` and it is easy to skim
past. The default is a **jackknife** SE, which deletes one treated unit at a time —
and California is the only treated unit, so it is undefined. `bootstrap` fails for
the same reason. Only `placebo` works, and note that
`summary(tau.hat, se.method = "placebo")` does **not** populate the `se` field;
`synthdid_se(tau.hat, method = "placebo")` must be called directly. It is a
permutation quantity, so report the seed. With the seed in `did.qmd` it gives 8.37,
and the resulting 95% interval **includes zero**.

### State the truth next to every simulated estimate

`graph-to-estimate.qmd` prints the true front-door ACE (0.453) beside its estimate;
`graphs-identification-estimation.qmd` did not, so a reader had no way to judge
0.0775. Derived it analytically (0.0547) and added it, plus the naive contrast for
comparison. Both chapters now anchor their numbers.

### Prefer the population value over the realized-sample one as a benchmark

`nonparametric.qmd` used `mean(Y.1 - Y.0)` as "True_Psi". With binary potential
outcomes at n=1000 that has a sampling SD of about 0.020 — comparable to the
estimator SEs it is being compared against, and the reason this book printed 0.21
where the Julia companion printed 0.184 for the *same* DGP. The population ATE is
0.202785 by quadrature; both books now compute and report it.

### Describe what the code actually produced

Two prose/output mismatches this pass, both in chapters that were otherwise correct:
- The dose-response curve `1 + 2d - 0.15d^2` was called "hump-shaped ... falls at
  high doses". Its vertex is at 6.67 and the largest dose drawn is 6.39, so it
  increases across the whole support.
- The policy tree was described as splitting near the optimal threshold of 0.5. The
  printed depth-2 tree treats `X1 in (0.422, 0.627]` **or** `X1 > 0.644` — a
  non-monotone rule with a gap, for a monotone `tau`.

### If a section promises a comparison, compute it

"Comparing matching to weighting" contained only generic advice. The chapter's own
two estimates are full matching 1977 (SE 704) and entropy balancing 1273 (SE 770) —
exactly one SE apart, with p flipping 0.005 to 0.098, and entropy balancing has the
*better* mean balance. Built the table from the existing fitted objects so the
numbers regenerate rather than being hardcoded.

### Reviewing rendered output

Search only inside `<div class="cell-output...">` blocks. Plain-text searches of the
rendered HTML also hit Quarto's echoed-source panel, which produced several false
findings in earlier passes (e.g. matching a string that lives inside a chunk's own
`tryCatch`).
