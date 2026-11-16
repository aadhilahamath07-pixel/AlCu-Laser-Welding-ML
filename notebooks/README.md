# Research notebooks

Run notebooks in numerical order. The notebooks are designed for Google Colab and use repository source files or generated CSV outputs.

| Notebook | Stage | Main output |
|---|---|---|
| 01 | Original-file inspection | Workbook/text inspection and source inventory |
| 02 | Conventional data cleaning | Standardised conventional records |
| 03 | Conventional dataset build | `conventional_laser_welding_standardized.csv` |
| 04 | Wobble dataset build | `alcu_wobbling_welding_standardized.csv` |
| 05 | Master construction | `master_experimental_dataset.csv` and audit/data dictionary files |
| 06 | Master quality control | `master_experimental_dataset_qc.csv`, QC reports and target-specific subsets |
| 07 | Exploratory data analysis | Descriptive tables, source-specific plots, correlation summaries and ZIP export |
| 08 | Grouped ML evaluation | Fold-wise metrics, pooled out-of-fold metrics, observed-vs-predicted and residual plots |

## Colab links

- [01 – Data inspection](https://colab.research.google.com/github/aadhilahamath07-pixel/AlCu-Laser-Welding-ML/blob/main/notebooks/01_data_inspection.ipynb)
- [02 – Conventional cleaning](https://colab.research.google.com/github/aadhilahamath07-pixel/AlCu-Laser-Welding-ML/blob/main/notebooks/02_conventional_data_cleaning.ipynb)
- [03 – Build conventional dataset](https://colab.research.google.com/github/aadhilahamath07-pixel/AlCu-Laser-Welding-ML/blob/main/notebooks/03_build_conventional_dataset.ipynb)
- [04 – Build wobble dataset](https://colab.research.google.com/github/aadhilahamath07-pixel/AlCu-Laser-Welding-ML/blob/main/notebooks/04_build_wobbling_dataset.ipynb)
- [05 – Build master dataset](https://colab.research.google.com/github/aadhilahamath07-pixel/AlCu-Laser-Welding-ML/blob/main/notebooks/05_build_master_experimental_dataset.ipynb)
- [06 – Master QC](https://colab.research.google.com/github/aadhilahamath07-pixel/AlCu-Laser-Welding-ML/blob/main/notebooks/06_master_dataset_quality_control.ipynb)
- [07 – Exploratory data analysis](https://colab.research.google.com/github/aadhilahamath07-pixel/AlCu-Laser-Welding-ML/blob/main/notebooks/07_exploratory_data_analysis.ipynb)
- [08 – Grouped ML evaluation](https://colab.research.google.com/github/aadhilahamath07-pixel/AlCu-Laser-Welding-ML/blob/main/notebooks/08_grouped_ml_evaluation.ipynb)

## Execution notes

- Sign in to the intended Google account before opening Colab.
- Notebook outputs are not automatically committed to GitHub.
- Notebook 06 expects the master CSV produced by Notebook 05.
- Notebook 07 expects the audited `master_experimental_dataset_qc.csv` produced by Notebook 06.
- Notebook 08 expects `wobbling_ml_dataset.csv`, generated during the initial wobble ML preparation. It can be uploaded when prompted.
- Keep downloaded outputs with their source and notebook version; do not overwrite raw data.
