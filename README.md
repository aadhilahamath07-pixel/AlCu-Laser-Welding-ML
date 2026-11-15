# Aluminium–Copper Laser Welding: Data-Driven Weld Quality Prediction

<p align="center">
  <strong>An explainable, literature-informed machine-learning framework for studying process–structure–property relationships in dissimilar Al–Cu laser welding.</strong>
</p>

<p align="center">
  <a href="https://www.python.org/"><img src="https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white" alt="Python 3.10+"/></a>
  <a href="https://scikit-learn.org/"><img src="https://img.shields.io/badge/ML-scikit--learn-F7931E?logo=scikitlearn&logoColor=white" alt="scikit-learn"/></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-green.svg" alt="MIT License"/></a>
  <img src="https://img.shields.io/badge/Project-Research%20in%20progress-orange" alt="Research in progress"/>
</p>

---

## Overview

Aluminium and copper are widely used in electrical and energy systems, including electric-vehicle battery connections, busbars, power electronics and electrical terminals. Joining these dissimilar metals by laser welding is challenging because their thermophysical properties differ substantially. Process conditions can influence melting, mixing, dilution, porosity, cracking and the formation of brittle aluminium–copper intermetallic compounds (IMCs).

This project investigates whether experimental results reported in scientific literature can be assembled into a traceable dataset and used to develop machine-learning models for predicting selected weld-quality characteristics.

The intended outcome is a **reproducible research workflow** connecting published process parameters to measured weld properties, with careful validation, model interpretation and (where justified by the data) process-window exploration.

> **Current status:** Literature review and dataset design. No trained model, validated prediction capability or optimized process window is claimed at this stage.

## Research questions

1. Which laser and joint parameters are most consistently reported in experimental Al–Cu laser-welding studies?
2. How strongly are reported process conditions associated with mechanical, geometric, metallurgical and electrical weld outcomes?
3. Can models trained on some publications predict results from a completely held-out publication?
4. Which variables drive model predictions, and how stable are those explanations?
5. If data coverage is sufficient, what trade-offs exist between strength, IMC formation, electrical resistance and defect-related outcomes?

## Why Al–Cu laser welding is challenging

Aluminium and copper differ in melting behaviour, thermal conductivity, reflectivity and density. These differences make the local thermal cycle and melt-pool behaviour sensitive to beam position, energy input, joint geometry and material thickness.

A key metallurgical concern is the formation of Al–Cu intermetallic phases (for example, Al₂Cu and AlCu). Their amount, distribution and morphology depend on the thermal history and mixing conditions. Excessive or unfavourable IMC formation can contribute to brittle behaviour. Other reported weld-quality concerns include pores, cracks, unstable penetration and incomplete joining.

The project therefore treats weld quality as **multi-dimensional** rather than reducing it to a single strength value.

## End-to-end research workflow

~~~mermaid
flowchart TD
    A[Identify peer-reviewed Al–Cu laser-welding studies] --> B[Screen papers and record metadata]
    B --> C[Extract experimental conditions and measured outcomes]
    C --> D[Normalize units and document uncertainty]
    D --> E[Build source-traceable dataset]
    E --> F[Exploratory data analysis]
    F --> G[Define target-specific datasets]
    G --> H[Train baseline and nonlinear ML models]
    H --> I[Publication-grouped validation]
    I --> J[Error analysis and explainability]
    J --> K{Enough comparable data?}
    K -- Yes --> L[Uncertainty-aware process-window exploration]
    K -- No --> M[Report evidence gaps and data limitations]
    L --> N[Research report and reproducible outputs]
    M --> N
~~~

## Candidate input features

The final feature set will depend on what the source papers report with sufficient consistency. Missing values will be documented; values will not be invented to fill gaps.

| Feature group | Candidate variables | Notes |
|---|---|---|
| Laser settings | Laser power (W), welding speed (mm/s) | Record actual reported values and laser mode |
| Beam configuration | Beam offset (mm), focal position, beam diameter (mm), wobble settings | Definitions and sign conventions must be checked per paper |
| Materials | Aluminium grade, copper grade, temper/condition | Preserve the source designation |
| Joint geometry | Lap/butt configuration, sheet thicknesses (mm), overlap/gap | Geometry can strongly affect heat flow |
| Process environment | Shielding gas, filler, welding direction, laser source | Include when reported |
| Derived descriptors | Nominal line energy, thickness ratio, nominal power density | Calculate only when definitions and units are compatible |

A commonly used nominal line-energy descriptor is:

$$
E_l = \frac{P}{v}
$$

where P is laser power in watts and v is travel speed in mm/s, giving J/mm. This is a simplified descriptor—not the absorbed heat input—because it does not account for absorptivity, beam distribution, interaction time or heat losses.

Derived variables will be checked for redundancy with their source variables to avoid giving models duplicate information.

## Candidate prediction targets

Different publications measure different outcomes. Models will therefore be developed for individual targets or compatible target groups rather than assuming every paper reports every property.

| Outcome family | Candidate targets |
|---|---|
| Mechanical | Tensile strength, maximum load, shear strength, elongation, fracture mode |
| Weld geometry | Weld width, penetration depth, fusion-zone dimensions |
| Metallurgical | IMC layer thickness, phase identification, microhardness |
| Electrical | Joint/contact resistance or resistivity, with test conditions recorded |
| Defects | Porosity, crack presence, lack of fusion, defect area/fraction |

Target definitions, units, specimen geometry and test standards must be retained. Measurements that are not directly comparable will not be silently combined.

## Data collection and quality controls

Each observation should be traceable to its source publication and, where possible, to a specific figure, table or experimental condition.

Recommended metadata include:

- Publication title, authors, year, DOI or stable URL
- Figure/table/page used for extraction
- Experimental versus numerical/simulated origin
- Material grades, thicknesses and joint configuration
- Original value, original unit and normalized value
- Measurement/test method and specimen details
- Extraction method (reported table, digitized plot, supplementary file)
- Any assumptions, ambiguity or extraction uncertainty

**Experimental and simulated results will be labelled separately.** They will not be pooled as if they were equivalent without a deliberate, documented analysis.

For plot-digitized values, the source figure and estimated digitization uncertainty should be recorded. Duplicate conditions and repeated specimens should be identified rather than treated as independent observations without justification.

See [data/README.md](data/README.md) for the proposed data dictionary and handling rules.

## Machine-learning strategy

The initial modelling plan is deliberately staged:

| Model | Role |
|---|---|
| Mean/median predictor | Simple reference baseline |
| Linear Regression / Ridge | Interpretable linear baseline |
| Polynomial Regression | Test selected nonlinear trends |
| Random Forest Regressor | Nonlinear ensemble baseline |
| Support Vector Regression | Kernel-based regression |
| Gradient boosting / XGBoost | Higher-capacity tabular model, if dataset size supports it |

An artificial neural network may be considered only if the amount and diversity of data justify its additional complexity. A more complex algorithm will not be treated as better unless it performs better on suitable held-out data.

### Validation designed for literature-derived data

Randomly splitting individual rows can produce overly optimistic results when many rows originate from the same paper or experimental campaign. The preferred evaluation is therefore **grouped by publication/study**:

- Keep all observations from a publication in the same fold.
- Use grouped cross-validation where the number of studies permits it.
- Reserve one or more studies for a final external-style test when feasible.
- Fit preprocessing and feature selection only within each training fold.
- Report the number of papers and observations in every split.

Metrics will include R², MAE and RMSE, with units and sample counts reported. Performance will be interpreted in light of dataset size, target range and between-study differences.

## Explainability, uncertainty and optimization

- **Explainability:** permutation importance and SHAP may be used to inspect model behaviour. These describe model associations; they do not establish physical causation.
- **Uncertainty:** bootstrap or other suitable uncertainty methods may be evaluated when data quantity and structure allow.
- **Optimization:** multi-objective exploration may be attempted only after validation is credible. Objectives could include increasing strength while reducing resistance, excessive IMC thickness or defect indicators.
- **Domain checks:** recommendations must remain within the observed and defensible data range. Extrapolated settings will be flagged rather than presented as reliable recommendations.

## Technology

- Python 3.10+
- pandas and NumPy for data handling
- Matplotlib and Seaborn for exploratory plots
- scikit-learn for preprocessing, validation and baseline models
- XGBoost (optional) for gradient boosting
- SHAP (optional) for model interpretation
- Jupyter for research notebooks
- Streamlit (later stage) for a proof-of-concept interface

## Repository structure

The repository will grow as the research progresses. The planned organization is:

~~~text
AlCu-Laser-Welding-ML/
├── data/
│   ├── raw/                 # Source-derived files; check reuse permissions
│   ├── processed/           # Cleaned, analysis-ready data
│   └── README.md            # Data dictionary and provenance rules
├── literature/              # Paper register and review notes
├── notebooks/               # Ordered, reproducible research notebooks
├── src/                     # Reusable data and modelling code
├── models/                  # Saved model artifacts (when appropriate)
├── results/
│   ├── figures/
│   └── metrics/
├── app/                     # Optional Streamlit demonstration
├── requirements.txt
├── LICENSE
└── README.md
~~~

Large datasets, licensed publisher content and raw paper PDFs should not be committed unless redistribution is permitted. The repository should store source references and derived data only in accordance with the relevant publication and data-use terms.

## Getting started

1. Clone the repository:

   ~~~bash
   git clone https://github.com/aadhilahamath07-pixel/AlCu-Laser-Welding-ML.git
   cd AlCu-Laser-Welding-ML
   ~~~

2. Create a virtual environment:

   ~~~bash
   python -m venv .venv
   ~~~

3. Activate it:

   **Windows PowerShell**
   ~~~powershell
   .venv\Scripts\Activate.ps1
   ~~~

   **macOS / Linux**
   ~~~bash
   source .venv/bin/activate
   ~~~

4. Install dependencies:

   ~~~bash
   pip install -r requirements.txt
   ~~~

5. Begin with the literature register and data dictionary before fitting any models.

## Research milestones

- [ ] Complete a focused literature search and screening protocol
- [ ] Build a publication register with DOI and experimental details
- [ ] Define a consistent data dictionary and unit-conversion rules
- [ ] Extract and quality-check experimental observations
- [ ] Perform exploratory data analysis and assess target coverage
- [ ] Establish simple baseline models
- [ ] Implement publication-grouped validation
- [ ] Compare suitable models and analyse prediction errors
- [ ] Evaluate explainability and uncertainty
- [ ] Explore process trade-offs if evidence supports it
- [ ] Prepare a reproducible report and manuscript

## Responsible use and limitations

This is a research project, not a certified welding procedure or industrial process-control system. Literature datasets may be small, heterogeneous and biased toward particular materials, joint designs or experimental setups. Reported values may use different test methods and definitions.

Predictions are exploratory estimates and must not replace physical validation, engineering review or applicable qualification standards. Any future model or application must communicate its training range, uncertainty and limitations.

## References

A verified bibliography will be maintained as papers are screened and included. Each dataset record should point to its source publication and exact extraction location. Do not add unverified citations or redistribute copyrighted figures and full-text papers.

## Contributing

Research-oriented contributions are welcome. Please document the source and extraction method for any proposed data addition, preserve original units, and explain any transformations. Code contributions should include reproducible examples and tests where practical.

## License

The original code in this repository is released under the MIT License. This does **not** grant permission to redistribute third-party papers, figures, tables or datasets. Those materials remain subject to their own copyright and licensing terms; see [LICENSE](LICENSE).

---

<p align="center">
  <strong>Materials Science × Laser Processing × Explainable Machine Learning</strong>
</p>
