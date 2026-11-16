# Master research datasets

Store the consolidated experimental dataset and its supporting audit/schema files here.

Expected files:
- master_experimental_dataset.csv (84 records; Notebook 05)
- master_experimental_dataset_qc.csv (Notebook 06 audited version)
- master_dataset_audit.csv
- master_data_dictionary.csv

Rules:
- Keep the original master dataset and QC-enriched dataset distinct.
- Preserve source identifiers, record identifiers, units, missingness and eligibility flags.
- Do not combine source-specific modelling targets without a documented comparability assessment.
- Keep the audit and data dictionary alongside the dataset.
- Check redistribution permissions and provenance before committing.

**Status (16 November 2026):** the 84-record master and QC outputs were generated and reviewed in Colab, but the actual CSV files have not yet been added to this directory.
