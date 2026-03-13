---
title: Technology mediation in child sexual exploitation and abuse in Africa and Asia
subtitle: README
authors: Ghai et al. (2026)
format: 
  typst:
    toc: true
    toc-depth: 2
  pdf:
    toc: true
    toc-depth: 2
    echo: false
    number-sections: true
    fig-caption: true
    fig-height: 12
    fig-width: 8
    latex-engine: xelatex
    fontsize: 10pt
    mainfont: "Times New Roman"
    header-includes:
      - \usepackage{pdflscape}   
      - \usepackage{geometry}     
      - \geometry{margin=1in}     
      - \usepackage{longtable} 
      - \usepackage{adjustbox}
---

\newpage

## Overview

This repository contains the analytical code for a study examining technology-facilitated child sexual exploitation and abuse (CSEA) across Eastern and Southern Africa and Southeast Asia. The analysis uses data from the Disrupting Harm study conducted by UNICEF, ECPAT International, and INTERPOL.

## Repository Structure

```
project/
├── reference-data/         # ITU data for population burden validation
│   ├── Individuals using the Internet.csv         
│   └── Individuals using the Internet_gender.csv   
├── qmd/                    # Analysis scripts
│   ├── 1a_preprocess.qmd     
│   ├── 1b_population_burden.qmd
│   ├── 2a_disclosure.qmd
│   ├── 2b_CSEAmodelling.qmd
│   ├── 3a_disclosure_modelling.qmd
│   └── 3b_disclosure_modelling_imp.qmd
├── rendered/               # HTML outputs (code + results)
│   ├── 1a_preprocess.html 
│   ├── 1b_population_burden.html
│   ├── 2a_disclosure.html
│   ├── 2b_CSEAmodelling.html
│   ├── 3a_disclosure_modelling.html
│   └── 3b_disclosure_modelling_imp.html
└── README.md
```
## System Requirements

### Software dependencies

| Software | Version tested |
|---|---|
| R | 4.4.1 (2024-06-14) |
| CmdStan | 2.32.2 |
| Quarto | 1.4.x |

Compatible with Windows, macOS, and Linux.

Required R packages:

```r
install.packages(c(
  "tidyverse","rlang","survey","srvyr","mice","brms","cmdstanr","loo","emmeans",
  "bayesplot","bayestestR","posterior","marginaleffects","ggdist","ggrepel",
  "ggnewscale","ggh4x","viridis","RColorBrewer","colorspace","scales","corrplot",
  "knitr","kableExtra","gt","gtsummary","broom.mixed","sjPlot","sf",
  "rnaturalearth","rnaturalearthdata","DataExplorer","patchwork","gridExtra",
  "cowplot","naniar","collapse","flextable","officer","png","readxl","ggtext",
  "glue","readr","stringr","gridGraphics","janitor","here",
  "rstan","car","performance","tidybayes"
))

# CmdStan must be installed separately:
cmdstanr::install_cmdstan()
```

### Non-standard hardware

No non-standard hardware was required. The modelling scripts use parallel computation with up to 8 CPU cores. The pipeline was run on a standard desktop or laptop.

## Rendered Outputs

> **A demo dataset is not available.** The data used in this study involve sensitive child-level responses collected under a restricted access agreement with UNICEF and cannot be shared publicly. We considered generating synthetic data but determined this could risk misuse or misinterpretation given the sensitivity and complexity of the variables.
>
> **In lieu of a runnable demo, all scripts have been rendered to HTML outputs** (see `rendered/`) that show every line of code alongside all results, figures, and model diagnostics. Reviewers and readers are encouraged to inspect these outputs directly.

### Expected output

Each rendered HTML document shows:

- `1a_preprocess.html` — Prevalence estimates, demographic breakdowns, heatplots, and summary tables
- `1b_population_burden.html` — Population burden estimates, forest plots, scatterplots, and case count estimates with confidence intervals
- `2a_disclosure.html` — Disclosure rates, barriers, and co-occurrence analysis
- `2b_CSEAmodelling.html` — Marginal effects and predicted probabilities of victimisation
- `3a_disclosure_modelling.html` — Bayesian model summaries, posterior diagnostics, sensitivity checks, and manuscript figures
- `3b_disclosure_modelling_imp.html` — Pooled imputed model estimates, penalised regression sensitivity analyses, and full diagnostics

The most computationally intensive steps are the Bayesian multilevel models in `3a_disclosure_modelling.qmd` and `3b_disclosure_modelling_imp.qmd`, which fit models across 30 multiply imputed datasets using MCMC sampling. 

## Workflow

```
1. 1a_preprocess.qmd               -> Data cleaning, variable creation, prevalence estimates
2. 1b_population_burden.qmd        -> Population-level burden estimation
3. 2a_disclosure.qmd               -> Descriptive analysis of disclosure patterns
4. 2b_CSEAmodelling.qmd            -> Models predicting CSEA victimisation
5. 3a_disclosure_modelling.qmd     -> Complete-case Bayesian disclosure models
6. 3b_disclosure_modelling_imp.qmd -> Imputed Bayesian disclosure models
```

## Analysis Scripts

### 1. Prevalence Estimation

**`1a_preprocess.qmd`**
- Data cleaning and variable creation
- Survey-weighted prevalence estimates (overall, by demographics, by country)
- Abuse type comparisons (technology-facilitated versus all CSEA)
- Descriptive heatplots and tables

**`1b_population_burden.qmd`**
- Double-denominator approach to population-level burden estimation
- Combines child survey data with household survey internet exposure rates
- Monte Carlo uncertainty propagation
- Validation against ITU youth internet use statistics

### 2. Disclosure Descriptives

**`2a_disclosure.qmd`**
- Disclosure patterns and rates
- Barriers to disclosure
- Barrier overlap and co-occurrence analysis

### 3. Modelling

**`2b_CSEAmodelling.qmd`**
- Demographic and contextual predictors of technology-facilitated CSEA
- Urbanisation models (urban, rural, peri-urban)
- Average marginal effects and predicted probabilities of victimisation

**`3a_disclosure_modelling.qmd`**
- Multilevel Bayesian models predicting disclosure (complete-case analysis)
- Fitted using `brms` with `cmdstanr` backend
- Posterior summaries, diagnostic plots, and sensitivity checks

**`3b_disclosure_modelling_imp.qmd`**
- Disclosure models using multiply imputed data (M = 30 imputations)
- Pooled multilevel Bayesian estimates
- Sensitivity analyses using horseshoe penalised regression
- Full model diagnostics and manuscript figures

## Modelling Setup

All Bayesian multilevel models were estimated using `brms` with the `cmdstanr` backend. Key configuration:

- 4 chains × 2,000 iterations (1,000 warm-up), yielding 4,000 post-warmup samples
- Target acceptance rate (`adapt_delta`) = 0.99
- Parallel computation via threading (`brms.threads = parallel::detectCores() %/% 4`), up to 8 CPU cores
- 30 multiply imputed datasets (M = 30)

See `2b_CSEAmodelling.qmd`, `3a_disclosure_modelling.qmd`, and `3b_disclosure_modelling_imp.qmd` for complete model code, prior specifications, and diagnostics. Installation guide is not included as underlying data are held under restricted access and the pipeline cannot be executed without them.

## Data Confidentiality

The data used in this analysis are highly sensitive, involving reports of technology-facilitated sexual exploitation and abuse of children. For ethical reasons:

- The raw dataset **cannot be shared publicly**, as it includes sensitive child-level responses
- Data were shared under a **restricted access agreement** with UNICEF and are governed by strict ethical protections
- All analyses were reviewed to ensure they respect the **privacy, dignity, and safety** of child participants

## License and Open Science

This software is released under the **MIT License** 
(OSI-approved; https://opensource.org/licenses/MIT).

We are committed to open science and have released all preprocessing and modelling code publicly, with all scripts rendered to **HTML** showing code and output side-by-side. While the underlying data are restricted (see Data Availability above), all analysis steps are fully documented 
to support methodological transparency.

**Last updated:** March 2026
