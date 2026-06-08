# Replicating "Artful Paltering" (Rogers et al., 2017), Study 1

A close, preregistered replication of **Study 1** of Rogers, Zeckhauser, Gino, Norton, & Schweitzer (2017), *"Artful Paltering: The Risks and Rewards of Using Truthful Statements to Mislead Others,"* Journal of Personality and Social Psychology, 112(3), 456–473.

**Outcome: a successful replication of all three target findings** (N = 376, Prolific, 2026). Preregistration: <https://osf.io/w2qcd/overview>

## The original finding

In an incentivized used-car negotiation, sellers chose between an honest reply and a misleading reply to a buyer's direct question. The misleading reply was either a *palter* (a true-but-misleading statement) or an outright *lie* — and lying was made the *more* lucrative option. Study 1 reported that people (1) chose to mislead more often when the option was a palter than a lie, (2) rated paltering as more ethical than lying, and (3) the ethicality difference mediated the choice.

## Replication results

| Finding | Original (N = 550) | This replication (N = 376) |
|---|---|---|
| Chose to mislead — palter vs. lie | 71% vs. 55%, χ²(1) = 14.58 | 61.7% vs. 26.6%, χ²(1) = 47.0, *p* < .001 |
| Ethicality — palter vs. lie | *M* = 4.05 vs. 2.12, *d* = 1.39 | *M* = 4.17 vs. 2.01, *d* = 1.48 |
| Mediation — ethicality → choice | significant indirect effect | significant; MC CI [0.38, 0.68], BCa bootstrap [0.33, 0.77] |

All three replicated. The choice-rate gap was *larger* than the original, driven by a much lower willingness to lie outright. The full write-up is in [`ReplicationReport.html`](ReplicationReport.html) (source: [`ReplicationReport.qmd`](ReplicationReport.qmd)).

## Repository structure

```
replication/
├── ReplicationReport.qmd    # Final report (Quarto) — the main deliverable
├── ReplicationReport.html   # Rendered report, with the side-by-side figure
├── script/
│   ├── 00_priori_power_analysis.Rmd   # A priori power analysis (target N = 380 @ 90%)
│   ├── 01_clean.Rmd                   # De-identify + clean the raw export -> data_clean.rds
│   └── 02_analysis.Rmd                # Confirmatory + exploratory analyses (4 samples)
├── data/
│   ├── data_deid.rds        # De-identified, duplicate-marked (no PROLIFIC_PID)
│   └── data_clean.rds       # Analysis-ready, fully flagged (input to 02 and the report)
├── original_paper/          # Rogers et al. (2017) PDF + supplemental appendix
└── project.Rproj
```

Each `.Rmd` / `.qmd` has a rendered `.html` committed alongside it.

## Reproducing the analysis

Requires **R ≥ 4.1** (for the native `|>` pipe) and:

```r
install.packages(c("tidyverse", "lavaan", "semTools", "pwr", "psych"))
```

The pipeline reads the committed, **de-identified** data, so it runs with no PII present:

1. **`script/01_clean.Rmd`** — reads `data/data_deid.rds`, applies the preregistered exclusion flags, and writes `data/data_clean.rds`. (The step that reads the raw Qualtrics export is gated off via `RAW_PII_AVAILABLE <- FALSE`; the raw file containing `PROLIFIC_PID` is intentionally not in the repo.)
2. **`script/02_analysis.Rmd`** — reads `data/data_clean.rds`; runs H1 (χ²), H2 (Welch *t* + reliability), and the mediation (WLSMV/probit with Monte Carlo and BCa-bootstrap CIs) across the preregistered sample and three robustness samples.
3. **`ReplicationReport.qmd`** — renders the report: `quarto render ReplicationReport.qmd`. **Slow step:** the mediation bootstrap refits the model 2,000 times.

`data/data_clean.rds` is already committed, so steps 2–3 run on their own; step 1 only needs re-running if the cleaning logic changes.

## Privacy

No raw data or Prolific IDs are stored in this repository. `PROLIFIC_PID` is read only inside the (disabled) de-identification chunk of `01_clean.Rmd` and is dropped before anything is saved, so every committed file is de-identified.

## Author

Ke Fang (fangke@stanford.edu) · Stanford PSYCH-251
