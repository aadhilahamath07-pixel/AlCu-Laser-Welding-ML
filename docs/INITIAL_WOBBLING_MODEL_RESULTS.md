# Initial wobble-welding model evaluation

**Updated:** 16 November 2026  
**Status:** Preliminary exploratory result with five-fold grouped validation; not a validated predictive model.

## Dataset used

The initial modelling subset was created from the Notebook 06 audited master dataset and saved as `wobbling_ml_dataset.csv`.

| Item | Value |
|---|---:|
| Usable records | 29 |
| Unique process conditions | 17 |
| Conditions represented once | 5 |
| Conditions represented twice | 12 |
| Missing values in 3 inputs + 2 targets | 0 |

**Inputs**
- `laser_power_W`
- `tangential_wobbling_speed_mm_s`
- `wobbling_diameter_mm`

**Targets evaluated separately**
- `bead_width_face_mm`
- `bead_width_interface_mm`

The 29 records cover 17 of the 18 original wobble process conditions. Replicates from a process condition were kept together using GroupKFold validation. Five folds were used, with 3–4 held-out conditions per fold. This avoids placing repeated records from the same condition in both training and validation partitions.

## Initial cross-validation results

The reported metrics below are pooled out-of-fold predictions from five-fold GroupKFold validation. They were supplied from the executed Colab analysis on 16 November 2026.

| Target | Model | MAE (mm) | RMSE (mm) | R² |
|---|---|---:|---:|---:|
| Face bead width | Mean baseline | 0.3948 | 0.5262 | -0.1444 |
| Face bead width | Ridge regression | 0.3901 | 0.5204 | -0.1196 |
| Face bead width | Extra Trees | **0.2830** | **0.3979** | **0.3457** |
| Interface bead width | Mean baseline | **0.1036** | **0.1488** | **-0.0533** |
| Interface bead width | Ridge regression | 0.1231 | 0.1640 | -0.2806 |
| Interface bead width | Extra Trees | 0.1068 | 0.1655 | -0.3040 |

### Interpretation

**Face bead width:** Extra Trees was the strongest of the three tested approaches in this initial comparison. Relative to the mean baseline, its MAE was lower by approximately 28.3%. Its pooled out-of-fold R² was positive (0.3457). This is a promising preliminary signal, not evidence of reliable generalisation.

**Interface bead width:** Neither Ridge nor Extra Trees improved on the mean baseline across all reported metrics. The baseline remains the preferred reference for this target. Do not claim that interface width can currently be predicted from these three inputs.

## Model configurations reported

- Mean baseline: `DummyRegressor(strategy="mean")`.
- Ridge: `StandardScaler()` followed by `Ridge(alpha=1.0)`.
- Extra Trees: `ExtraTreesRegressor(n_estimators=300, min_samples_leaf=2, random_state=42)`.
- Validation: `GroupKFold(n_splits=5)`, grouped by the three process inputs combined into `condition_group`.
- Metrics: pooled out-of-fold MAE, RMSE and R².

## Fold-wise stability results

Notebook 08 was subsequently run on the same 29-row CSV. The following values are the mean ± standard deviation across its five validation folds. Fold-level R² is especially unstable because each fold contains only 5–6 test records.

| Target | Model | Fold MAE mean ± SD (mm) | Fold RMSE mean ± SD (mm) | Fold R² mean ± SD |
|---|---|---:|---:|---:|
| Face bead width | Mean baseline | 0.3953 ± 0.0996 | 0.5008 ± 0.1745 | -2.0522 ± 3.3053 |
| Face bead width | Ridge regression | 0.3921 ± 0.1377 | 0.4866 ± 0.2041 | -2.1189 ± 3.4930 |
| Face bead width | Extra Trees | 0.2795 ± 0.1198 | 0.3603 ± 0.1764 | -0.5447 ± 1.8890 |
| Interface bead width | Mean baseline | 0.1017 ± 0.0738 | 0.1208 ± 0.0928 | -0.4144 ± 0.6184 |
| Interface bead width | Ridge regression | 0.1224 ± 0.0565 | 0.1474 ± 0.0769 | -2.1761 ± 2.3371 |
| Interface bead width | Extra Trees | 0.1080 ± 0.0517 | 0.1485 ± 0.0808 | -3.3298 ± 4.6045 |

The fold-wise summary supports a cautious interpretation: Extra Trees has lower average fold MAE for face bead width than the mean baseline, but its fold-level R² varies substantially and has a negative mean. For interface bead width, the mean baseline has the lowest average fold MAE and RMSE. Therefore, face width remains exploratory, while interface width is not currently supported as a predictive target.

Notebook 08 also generated out-of-fold observed-versus-predicted and residual plots for the face-width Extra Trees model, together with `wobbling_fold_metrics.csv`, `wobbling_pooled_oof_metrics.csv`, and `wobbling_oof_predictions.csv` in `notebook08_grouped_ml_outputs.zip`. The plots are diagnostic, not independent validation.

## Limitations and required checks

1. Only 29 records and 17 independent process-condition groups are available. Each validation fold contains only 3–4 held-out conditions, making estimates sensitive to the split.
2. Fold-level R² values are highly variable and can be misleading with only 5–6 test records per fold. Interpret MAE/RMSE alongside pooled out-of-fold metrics and the raw predictions.
3. This is validation across process conditions within one source dataset, not external validation across independent publications or laboratories.
4. The feature set is limited to three reported wobble process variables. Material grade/thickness, joint configuration, beam setup and other potentially relevant variables are not represented here.
5. Do not use these models to recommend production settings or claim an optimum welding window.
6. Do not tune repeatedly against these same folds and present the resulting score as an unbiased final estimate.

## Current research decision

- Retain **face bead width** as the primary exploratory modelling target for this wobble dataset. Extra Trees improves on the baseline in pooled MAE/RMSE and in mean fold MAE, but fold-level R² is unstable; describe this as preliminary evidence only.
- Keep **interface bead width** as a secondary descriptive target, not a successful prediction target: the mean baseline outperforms both candidate models on pooled MAE/RMSE and average fold MAE/RMSE.
- Do not pool these wobble records with conventional welding, Shin et al. or Lee et al. records.
- The next priority is to improve independent evidence and feature coverage, not to over-tune these 29 records. Seek additional comparable wobble-welding measurements and verify experimental metadata.

## Reproducibility

Notebook 08 contains the evaluation workflow. The source CSV is generated by the previous preparation workflow and is not included as a committed model result unless data provenance and redistribution permissions have been reviewed. Preserve the exact input CSV and notebook version used for any future manuscript table.
