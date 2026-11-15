# Source code

Reusable Python functions will be introduced here as the workflow stabilises.

Planned modules:

- `data_validation.py`: schema checks, identifier checks and source-specific QC rules.
- `preprocessing.py`: train-fold-only preprocessing utilities.
- `eda.py`: reusable source-aware descriptive summaries and plots.
- `modeling.py`: baseline regressors and grouped validation helpers.
- `metrics.py`: consistent regression metrics and uncertainty summaries.

The current workflow is notebook-led (01–07). Do not duplicate notebook logic into this directory until it has been tested, documented and made reusable. Any function that changes or filters observations must preserve a traceable record of the rule applied.
