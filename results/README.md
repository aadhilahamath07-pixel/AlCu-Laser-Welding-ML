# Model and analysis results

Store reviewed, reproducible outputs from notebooks here, grouped by notebook and run/version.

Suggested layout:
- `notebook07_eda/`: approved EDA summary tables and figures.
- `notebook08_wobble_ml/`: fold metrics, pooled metrics, out-of-fold predictions and plots.
- `notebook09_conventional_ml/`: fold metrics, pooled metrics, out-of-fold predictions and plots.

For every result bundle, record:
- notebook path and Git commit;
- exact input dataset filename/version;
- target and feature definitions;
- validation grouping strategy;
- model settings and random seeds;
- date generated and software versions.

Do not commit temporary runtime files, duplicate ZIPs or outputs that cannot be traced to their input data and notebook version. Keep the written interpretation in `docs/`.

**Status (16 November 2026):** Notebook 08 and Notebook 09 output ZIPs were downloaded and analysed in the project conversation, but their underlying CSV prediction/metric files are not yet archived in this directory.
