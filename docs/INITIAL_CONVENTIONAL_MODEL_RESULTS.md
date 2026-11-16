# Conventional Al–Cu Laser-Welding: Initial Grouped ML Results

## Dataset and validation

Notebook 09 evaluated 21 eligible records from seven laser-power/welding-speed conditions, with three repeats per condition. The six records with all three geometry outputs equal to zero remain preserved in the source/master data and were excluded from this default regression experiment because their physical meaning has not yet been verified.

Inputs: laser power (W) and welding speed (mm/s). Targets were modelled separately: weld bead width, interface width and penetration depth (mm).

Validation used leave-one-process-condition-out cross-validation. All repeats from a held-out power–speed condition were kept together. Models were a training-mean baseline, standardized Ridge regression and Extra Trees (300 trees, minimum leaf size 2). No hyperparameter tuning was performed.

## Pooled out-of-fold results

| Target | Model | MAE (mm) | RMSE (mm) | Pooled R² |
|---|---|---:|---:|---:|
| Weld bead width | Mean baseline | 0.2742 | 0.3095 | -0.2794 |
| Weld bead width | Ridge | 0.1199 | 0.1488 | 0.7042 |
| Weld bead width | Extra Trees | **0.0984** | **0.1372** | **0.7487** |
| Interface width | Mean baseline | 0.0995 | 0.1291 | -0.1576 |
| Interface width | Ridge | 0.0993 | 0.1212 | -0.0212 |
| Interface width | Extra Trees | **0.0777** | **0.1140** | **0.0972** |
| Penetration depth | Mean baseline | 0.0580 | 0.0660 | -0.1564 |
| Penetration depth | Ridge | **0.0532** | **0.0648** | **-0.1163** |
| Penetration depth | Extra Trees | 0.0596 | 0.0775 | -0.5959 |

## Fold variability

Fold MAE mean ± standard deviation (mm):

| Target | Model | Fold MAE mean ± SD |
|---|---|---:|
| Weld bead width | Mean baseline | 0.2742 ± 0.1036 |
| Weld bead width | Ridge | 0.1199 ± 0.0828 |
| Weld bead width | Extra Trees | 0.0984 ± 0.0751 |
| Interface width | Mean baseline | 0.0995 ± 0.0624 |
| Interface width | Ridge | 0.0993 ± 0.0576 |
| Interface width | Extra Trees | 0.0777 ± 0.0748 |
| Penetration depth | Mean baseline | 0.0580 ± 0.0168 |
| Penetration depth | Ridge | 0.0532 ± 0.0217 |
| Penetration depth | Extra Trees | 0.0596 ± 0.0287 |

Fold-level R² is highly unstable because each held-out condition contains only three measurements and within-condition target variation can be small. The fold R² mean is negative for every target/model combination (including weld bead width Extra Trees: -0.6521 ± 0.8080), despite positive pooled out-of-fold R² for bead width. Report pooled out-of-fold R² alongside MAE/RMSE and disclose this instability; do not present the pooled score alone as proof of robust generalization.

## Interpretation

1. **Weld bead width is the strongest preliminary target.** Extra Trees gives the lowest pooled MAE/RMSE and highest pooled R²; Ridge is also substantially better than the mean baseline. This is encouraging but remains a small-data result, not a validated production predictor.
2. **Interface width is weakly predictable in this experiment.** Extra Trees improves pooled MAE/RMSE over the baseline, but pooled R² is only 0.097 and fold variability is substantial.
3. **Penetration depth is not successfully predicted.** Ridge is marginally better than the mean baseline on MAE/RMSE, but all pooled R² values are negative; Extra Trees is worse than the baseline.
4. The experiment contains only seven independent process settings and two input variables. Performance may be sensitive to which condition is held out.

## Context versus the wobble experiment

The earlier wobble evaluation used 29 records from 17 process-condition groups. Its face-bead-width Extra Trees pooled metrics were MAE 0.2830 mm, RMSE 0.3979 mm and R² 0.3457. The conventional bead-width experiment reports lower pooled errors (MAE 0.0984 mm, RMSE 0.1372 mm) and higher pooled R² (0.7487). This is a descriptive comparison only: the datasets come from different experiments and their measurement protocols, process spaces and target definitions may differ. Do not infer that conventional welding is intrinsically more predictable or pool the records into one training set.

For wobble interface width, the mean baseline was stronger than the tested ML models (pooled MAE 0.1036 mm, RMSE 0.1488 mm, R² -0.0533). Conventional interface-width Extra Trees has pooled MAE 0.0777 mm, RMSE 0.1140 mm and R² 0.0972, but the same cross-study comparability caveat applies. Wobble penetration was not advanced as an ML target because 29/36 values were exactly 0.300 mm.

## Research decision and next work

- Retain conventional weld bead width as the leading preliminary modelling result; retain interface width as exploratory and do not claim useful penetration-depth prediction.
- Verify the six all-zero source records against the original paper/data notes before deciding whether they represent no weld/no fusion, measurement convention or transcription issue. Keep them in the master dataset meanwhile.
- Seek additional independent process conditions and metadata (material grades/thicknesses, joint geometry, beam/focus details, laser mode and measurement protocol) rather than tuning models on 21 rows.
- Seek external validation on an independent study before making generalization claims.
- Preserve the source-specific experiments separately. No causal, optimum-process-window or production-readiness claims are supported.

## Reproducibility

Notebook: `notebooks/09_conventional_grouped_ml_evaluation.ipynb`

Generated outputs supplied by the researcher: `conventional_fold_metrics.csv`, `conventional_pooled_oof_metrics.csv`, `conventional_eligible_model_data.csv`, and `conventional_oof_predictions.csv` (packaged in `notebook09_conventional_ml_outputs.zip`).
