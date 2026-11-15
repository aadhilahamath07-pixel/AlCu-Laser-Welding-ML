# Dataset documentation

This directory is intended for structured data extracted from published aluminium–copper (Al–Cu) laser-welding studies.

## Data organization

- **raw/**: source-derived extraction files. Do not add publisher PDFs, figures or supplementary material unless redistribution is permitted.
- **processed/**: cleaned tables used for analysis.
- **master_dataset.csv**: optional consolidated table after the schema has stabilized.

## Recommended record-level fields

| Field | Description |
|---|---|
| record_id | Unique observation identifier |
| study_id | Stable identifier for the publication or experimental campaign |
| doi | DOI or stable publication URL |
| publication_year | Publication year |
| source_location | Figure, table, page or supplementary-file reference |
| data_origin | Experimental, numerical simulation or other; never leave ambiguous |
| al_grade, cu_grade | Material designations exactly as reported |
| al_thickness_mm, cu_thickness_mm | Sheet thicknesses |
| joint_type | Lap, butt or source-specific configuration |
| laser_power_w | Laser power |
| welding_speed_mm_s | Travel speed |
| beam_offset_mm | Offset with a documented sign convention |
| focus_position_mm | Focus position and reference convention |
| beam_diameter_mm | Diameter and definition used by the source |
| shielding_gas | Gas and flow rate, if reported |
| target_name | Measured response, e.g. tensile strength or IMC thickness |
| target_value | Numeric measured value |
| target_unit | Unit of the measured response |
| test_method | Test standard, specimen geometry and/or measurement method |
| extraction_method | Table, text, digitized plot, supplementary data |
| extraction_notes | Assumptions, uncertainty, ambiguity or exclusions |

The final schema may use one row per weld condition and separate target columns, or a long format with one row per condition–target pair. Choose based on the coverage and missingness pattern after reviewing the literature.

## Data rules

1. Preserve the original reported value and unit before converting anything.
2. Record conversion formulas and conventions; do not overwrite source values.
3. Do not infer unreported parameters from context without explicitly labelling the estimate and its uncertainty.
4. Keep experimental and simulated observations distinguishable.
5. Record study identity so train/test splits can be grouped by publication or campaign.
6. Identify replicates, averages, standard deviations and sample counts separately whenever reported.
7. Keep the exact source location for every extracted measurement.
8. Do not combine measurements with incompatible definitions or test methods without justification.
9. Check the publication's copyright and reuse terms before sharing extracted tables or digitized data publicly.

## Missing values

Use blank/NA values for unavailable measurements. Do not replace missing values with zero. Any imputation must be performed within the training fold during model validation, not on the full dataset before splitting.

## Status

The schema is a starting proposal. It should be revised after the first group of papers has been screened and the actual reporting patterns are known.
