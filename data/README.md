# Data directory: provenance, schema and quality controls

This directory stores source-derived data for the Al–Cu laser-welding research project. Keep original files, standardised files and consolidated datasets in separate locations.

## Directory layout

- `raw/`: source files as received or extracted. Do not edit these in place.
- `processed/`: standardised source-level CSV files produced by the cleaning notebooks.
- `master/`: consolidated master dataset and audited versions, once reviewed and approved.
- `README.md`: data schema, provenance and handling rules.

Notebook outputs currently download to the Colab runtime; they are not automatically copied into this repository. Commit a generated CSV only after checking provenance, licensing and the corresponding audit.

## Current master dataset inventory

The audited Notebook 06 output contains 84 records and 45 columns:

| Source ID | Records | Main reported variables |
|---|---:|---|
| `conventional_laser_welding_dataset` | 27 | Laser power, welding speed, weld-bead width, interface width, penetration depth |
| `alcu_wobbling_welding_dataset` | 36 | Laser power, tangential wobbling speed, wobbling diameter, face/interface bead width, penetration depth |
| `shin_2021_metals_11050831` | 11 | Power, scan speed, reported heat input, tensile-shear strength, electrical resistance, welding area |
| `lee_2022_metals_12111957` | 10 | Upper material, welding speed, reported minimum/maximum/average force, fracture-location description |
| **Total** | **84** | **Four distinct source groups** |

The consolidated table is for traceability and auditing. It is not automatically a scientifically homogeneous training dataset.

## Master record identifiers

| Field | Meaning |
|---|---|
| `master_record_id` | Unique ID assigned in the consolidated master table |
| `source_id` | Source dataset or paper identifier |
| `study_id` | Experimental campaign/publication grouping identifier |
| `record_id` | Record identifier within its source |
| `replicate_id` | Replicate label where the source reports repeated specimens/conditions |

Retain the source workbook, paper DOI, exact table/figure/page and extraction notes alongside any future schema extension. A master ID alone is not sufficient provenance.

## Important current QC findings

The supplied Notebook 06 audit reports:

- 84 total records.
- 0 duplicate master IDs.
- 0 duplicate `source_id + record_id` keys.
- 0 exact duplicate rows.
- 6 conventional records have all three geometry outputs equal to zero. They are retained, flagged for physical interpretation and excluded from the default geometry regression subsets.
- 7 wobble records have missing bead-width measurements: 6 source notes say “Specimen cut”; 1 has no stated reason. Do not impute these widths.
- 4 Lee 2022 force records have reported range/average consistency flags. The force modelling subset is empty by design until the original paper table is verified.
- `fracture_area_reported` was renamed `fracture_location_reported` because its values are categorical fracture-location descriptions (Al, Cu, Joint, or not reported), not a measured area.

These checks do not establish that all other values are correct. Verify extracted values against the original sources before publication or modelling.

## Standardisation principles

1. Preserve source values and source units before normalisation.
2. Use explicit units in numeric column names (for example, `laser_power_W`, `welding_speed_mm_s`, `penetration_depth_mm`).
3. Keep categorical descriptors as categorical fields; do not encode them as arbitrary numbers without a documented modelling plan.
4. Keep missing values as NA. Zero is a physical numeric value and must not be used as a missing-value marker.
5. Preserve unusual values and attach a quality flag rather than deleting or correcting them without evidence.
6. Keep source-specific variables even when other studies do not report them.
7. Store material properties in a separate material table when their scope is material-grade-level rather than individual-weld-record-level.
8. Record transformations and derived variables separately from reported measurements.

## Target-specific eligibility fields

Notebook 06 creates Boolean flags rather than a single general-purpose eligibility field:

- Conventional: `eligible_weld_bead_width`, `eligible_interface_width`, `eligible_penetration_depth_conventional`.
- Wobble: `eligible_bead_width_face_wobble`, `eligible_bead_width_interface_wobble`, `eligible_penetration_depth_wobble`.
- Shin 2021: `eligible_tensile_shear_strength`, `eligible_electrical_resistance`, `eligible_welding_area`.
- Lee 2022: `eligible_force_average_N`.

Eligibility is a conservative data-screening rule, not proof that rows from different studies can be pooled. The default Lee force eligibility is false until source verification. The six conventional all-zero geometry records are retained in the master but excluded from default regression subsets pending interpretation.

## Target-specific subsets generated by Notebook 06

| Subset | Eligible rows |
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

Do not merge these subsets into one model without checking target definitions, process features, material grades, thickness, joint configuration and study effects.

## Missingness and repeated measurements

- Missing measurement: blank/NA, with source note if available.
- Zero measurement: retain as zero and investigate its physical meaning.
- Replicate: retain its identity; do not average repeats unless the analysis plan explicitly requires it and the replicate statistics are preserved.
- Repeated process condition: identify by the source's condition variables, not by assuming repeated rows are duplicates.
- Cross-study missing columns: expected in a combined wide-format master table. Assess missingness within each source.

## Modelling and validation cautions

- Split by study/publication where possible; row-wise random splitting can leak study-specific information.
- Fit imputation, scaling and feature selection inside each training fold.
- Keep experimental and simulated data separate unless a justified comparison is designed.
- Report sample counts, independent study counts, target units and uncertainty.
- Correlation and feature importance are descriptive/associational, not causal.

## Sources currently represented

- Shin et al. (2021), *Metals*, DOI: [10.3390/met11050831](https://doi.org/10.3390/met11050831).
- Lee et al. (2022), *Metals*, DOI: [10.3390/met12111957](https://doi.org/10.3390/met12111957).
- Conventional and wobble source datasets are represented by the raw workbooks in `raw/`. Preserve the source's title, authorship, DOI/URL and licensing metadata in the literature register before formal publication.
