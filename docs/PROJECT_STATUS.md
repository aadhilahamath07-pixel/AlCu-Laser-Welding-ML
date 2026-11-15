# Project status and research decisions

**Last updated:** 15 November 2026  
**Repository:** [AlCu-Laser-Welding-ML](https://github.com/aadhilahamath07-pixel/AlCu-Laser-Welding-ML)

## Completed

- Repository structure and research documentation established.
- Original source files stored under `data/raw/`.
- Conventional and wobble source datasets standardised through notebooks.
- Master dataset assembled: 84 records across four source groups.
- Notebook 06 QC output reviewed: 84 records, 45 columns, no duplicate master IDs, duplicate source-record keys or exact duplicate rows.
- Target-specific eligibility flags and source-specific modelling subsets generated.
- Notebook 07 created for source-aware exploratory data analysis.

## Current dataset composition

| Source | Records |
|---|---:|
| Conventional laser welding | 27 |
| Wobble laser welding | 36 |
| Shin et al. (2021) | 11 |
| Lee et al. (2022) | 10 |
| **Total** | **84** |

## Open data-quality questions

1. Interpret the six conventional records whose bead width, interface width and penetration depth are all zero. Keep them until source context resolves whether they represent no weld, no measurable fusion or another reporting convention.
2. Preserve seven missing wobble width measurements. Six have a “Specimen cut” note; one is unexplained.
3. Recheck Lee et al. (2022) force values for four flagged conditions against the original publication. Do not train on the force target until resolved.
4. Complete source-level metadata for the conventional and wobble datasets, including exact publication references and extraction locations.
5. Verify target definitions and experimental configurations before any cross-study pooling.

## Current target subset counts

| Target subset | Rows |
|---|---:|
| Conventional bead width | 21 |
| Conventional interface width | 21 |
| Conventional penetration depth | 21 |
| Wobble face bead width | 29 |
| Wobble interface bead width | 29 |
| Wobble penetration depth | 36 |
| Shin tensile-shear strength | 11 |
| Shin electrical resistance | 11 |
| Shin welding area | 11 |
| Lee average force | 0 |

## Immediate next stage

Notebook 07 performs exploratory data analysis separately by source. It describes distributions, input–output scatter relationships, repeated-condition structure and within-source correlations. The first EDA pass is descriptive; it must not be interpreted as evidence of causation or proof that separate studies can be pooled.

After EDA:
1. Review feature coverage and target distributions.
2. Confirm source provenance and comparable measurement definitions.
3. Choose one target and one source group for an initial baseline model.
4. Use a validation strategy that respects repeated conditions and study identity.
5. Expand to cross-study evaluation only if the number of independent studies and compatible observations supports it.

## Research integrity rules

- No fabricated values, undocumented corrections or silent row deletion.
- No blanket imputation before train/test splitting.
- Keep raw data immutable.
- Report small sample sizes and source heterogeneity honestly.
- Distinguish model association from physical causation.
- Do not claim a validated model or optimized welding window before validation and experimental confirmation.
