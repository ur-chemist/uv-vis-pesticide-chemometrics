<div align="center">

# 🔬 UV-Vis Pesticide Chemometrics

> **A fully reproducible negative-result study: evaluating univariate Beer–Lambert calibration and multivariate PLS regression for trace-level pesticide monitoring in 846 complex environmental water runoff samples**

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)](https://scikit-learn.org)
[![Pandas](https://img.shields.io/badge/Pandas-Data-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org)
[![Excel](https://img.shields.io/badge/Excel-Analysis-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)](https://microsoft.com/excel)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21911163.svg)](https://doi.org/10.5281/zenodo.21911163)
![Samples](https://img.shields.io/badge/Samples-846%20spectra-orange?style=flat-square)
![Wavelength](https://img.shields.io/badge/Wavelength-200–737.5%20nm-blueviolet?style=flat-square)
![Status](https://img.shields.io/badge/Status-Preprint%20Submitted-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square)

</div>

---

## 📌 Overview

This project applies UV-Vis spectrophotometry and chemometric analysis to detect and quantify pesticide residues in complex environmental water runoff samples. The dataset contains **846 full-range spectra (200–737.5 nm)** collected from water samples with varying concentrations of four analytes — **Bromide, Fluopyram, Diflufenican, and Mesosulfuron** — across 12 sampling ports, multiple seasons, and soil types.

The study is an honest, fully documented **negative-result investigation**: it demonstrates that UV-Vis spectrophotometry with linear chemometric calibration *cannot* reliably quantify trace pesticides in complex agricultural runoff without sample preparation, and it identifies the precise mechanistic reason why.

| Property | Detail |
|---|---|
| 📂 Dataset | Environmental water runoff UV-Vis spectra (real field samples) |
| 📊 Samples | 846 spectra × 215 clean wavelength channels |
| 🌊 Wavelength Range | 200–737.5 nm (2.5 nm resolution) |
| 🧪 Target Analytes | Bromide (Br⁻), Fluopyram, Diflufenican, Mesosulfuron |
| 📍 Sample Variables | Port (1–12), Season, Soil Type, Storm event |
| 🗄️ Code + Data DOI | [10.5281/zenodo.21911163](https://doi.org/10.5281/zenodo.21911163) |
| 🗄️ Source Dataset | [10.5281/zenodo.19324549](https://doi.org/10.5281/zenodo.19324549) |

---

## 🖼️ Visual Abstract

![Visual Abstract](visual_abstract.png)

---

## 📁 Project Structure

```
uv-vis-pesticide-chemometrics/
│
├── 📓 Analysis.ipynb                     # Main notebook: full analytical pipeline
├── 📊 uv_vis_projects_.xlsx              # Raw dataset (846 spectra, 230 columns)
├── 📋 UVVis_Analysis.xlsx               # Processed workbook — all formula sheets
├── 🐍 ubaid_pls_full_analysis.py        # Complete Python script (all 7 methods)
├── 🖼️  visual_abstract.png              # Graphical abstract
├── 📄 figures/
│   ├── fig1_spectra.png                 # Representative UV-Vis spectra
│   ├── fig2_pca_scores_bromide.png      # PCA bromide audit
│   ├── fig3_nc_selection.png            # RMSECV vs n_components
│   ├── fig4_vip_scores.png              # VIP score profile
│   ├── fig5a_fluopyram_predicted.png    # Predicted vs actual — Fluopyram
│   └── fig5b_bromide_predicted.png      # Predicted vs actual — Bromide
└── 📘 README.md
```

---

## 🔬 Methodology

### 0️⃣ Data Cleaning
- Loaded raw `.xlsx` file with 846 samples × 230 columns
- Removed 5 trailing NaN wavelength channels (740–750 nm)
- Final clean range: **200–737.5 nm (215 channels)**

### 1️⃣ Spectral Characterisation & λmax
- Sorted 846 samples by Fluopyram concentration
- Identified λmax at **202.5 nm** — consistent with π→π* transitions of aromatic pesticide chromophores
- Confirmed DOM dominates spectral profile across all samples

### 2️⃣ Univariate Beer–Lambert Calibration

| Metric | Value | Interpretation |
|---|---|---|
| Slope (β₁) | **−0.758729** | Physically impossible — negative |
| R² | **0.000738** | <0.1% variance explained |
| LOD / LOQ | **Negative** | Mathematically invalid |

Complete calibration failure — the Beer–Lambert law collapses in this matrix.

### 3️⃣ Cross-Validated PLS Comparison (5-fold CV, n=12)

| Model | RMSECV Bromide | R²cv Bromide | RMSECV Fluopyram | R²cv Fluopyram | R²cv Diflufenican | R²cv Mesosulfuron |
|---|---|---|---|---|---|---|
| Univariate Beer-Lambert (220 nm) | 694.7 | 0.179 | 0.991 | −0.004 | −0.004 | 0.002 |
| PLS mean-centred (12 comp.) | 560.5 | 0.466 | 0.751 | 0.423 | 0.002 | 0.027 |
| **PLS + SNV (12 comp.)** | **468.4** | **0.635** | 0.628 | **0.631** | **0.002** | **0.022** |

> ⚠️ **Honest note:** Despite R²cv of 0.631 for Fluopyram, the cross-validated predictions include physically impossible negative concentrations and heteroscedastic scatter — confirming the model is not analytically viable.

### 4️⃣ PCA Bromide Silent-Tracer Audit

> **PC1 explains 82.0% of total spectral variance** and co-varies with Bromide — a UV-transparent anion with no chromophore. This proves that dissolved organic matter (DOM), transported hydrologically with water flow, dominates the spectral dataset — not the pesticide signals.

The bromide audit elevates this from a scalar Pearson correlation to a multivariate proof: the contamination of spectral variance is not localised to one wavelength but pervasive across the entire dominant principal component.

### 5️⃣ VIP Score Analysis

```
VIP > 1.0:  202–300 nm ONLY  (maximum = 3.1 near 210–220 nm)
VIP < 1.0:  all wavelengths above 300 nm
```

Analytically relevant spectral information is confined to the UV region — precisely where DOM absorbs most intensely. This explains why selectivity is impossible without physical separation.

### 6️⃣ Predicted vs Actual — Three Failure Modes

All cross-validated outputs exhibit:
- **Severe point mass at zero** — inflating apparent R²cv
- **Negative concentration predictions** — physically impossible; calibration surface not constrained
- **Heteroscedastic scatter** — widens at higher concentrations (opposite of a good calibration)

---

## 📊 Key Findings

```
🔬 λmax           →  202.5 nm — π→π* transitions in UV region (DOM-dominated)
📉 Beer-Lambert   →  Complete failure: slope = −0.758729, R² = 0.000738
📊 PLS+SNV best   →  R²cv = 0.631 (Fluopyram), 0.635 (Bromide) — still not viable
🧂 Bromide proof  →  82.0% of PC1 variance = hydrological DOM, not analyte signal
🌱 Diflufenican   →  Completely undetectable across all models (R²cv ≈ 0.002)
📐 VIP region     →  202–300 nm only — same window as dominant DOM absorption
```

---

## 🛠️ Tools & Libraries

| Tool | Purpose |
|---|---|
| 🐍 Python + Pandas | Data loading, cleaning, EDA |
| 🤖 Scikit-learn (PLSRegression, PCA, KFold) | All ML models and cross-validation |
| 📈 Matplotlib | All 5 figures |
| 📉 Excel (LINEST, CORREL) | Supporting calculations |
| 📓 Jupyter Notebook | Reproducible analysis environment |

---

## ⚠️ Limitations & What This Means in Practice

This is a **documented negative result** on a real field dataset. Key limitations:

- [ ] Single catchment dataset — DOM composition varies regionally
- [ ] No fully independent external validation set (internal CV only)
- [ ] RMSECV units are native dataset units (Bromide range ~3,700× larger than pesticides)
- [ ] VIP values at 202–212 nm may reflect instrumental edge artefacts

**What should be done instead:**
- **SPE pre-concentration** (Oasis HLB, C18) — selectively removes DOM before UV-Vis
- **LC-MS/MS with MRM** — mass selectivity immune to DOM optical interference
- **MCR-ALS** — non-negativity constrained spectral deconvolution

---

## 💡 Why This Negative Result Matters

> *"Negative results, executed with mathematical rigour and open-source reproducibility, are the cartographic boundaries that prevent the scientific community from wandering into methodological dead-ends."*

The environmental sensor literature is full of UV-Vis sensors validated in clean water with high analyte concentrations. This study documents, with real field data and open code, exactly where and why those sensors fail — and what the signal-to-background ratio actually looks like (∼10⁻⁵). Any future researcher can reproduce every number here and benchmark new methods against this baseline.

---

## 📖 Citation

```bibtex
@misc{ubaid2026uvvis,
  author       = {Ubaid Ur Rehman},
  title        = {Evaluation of Chemometric-Assisted UV-Vis Spectrophotometry
                  for Trace-Level Pesticide Monitoring in Complex Environmental
                  Water Matrices: A Negative-Result Comparative Study},
  year         = {2026},
  publisher    = {Zenodo},
  doi          = {10.5281/zenodo.21911163},
  url          = {https://doi.org/10.5281/zenodo.21911163}
}
```

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21911163.svg)](https://doi.org/10.5281/zenodo.21911163)

---

## 👤 Author

<div align="center">

**Ubaid Ur Rehman**

BS Chemistry (Final Semester) | The Islamia University of Bahawalpur, Pakistan
CGPA: 3.84 / 4.0 | CM Honhaar Scholar | IChC 2026 Qualifier

*Targeting roles in Quality Assurance, Analytical Chemistry, and Environmental R&D*

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/ubaid-ur-rehman-chemist)
[![GitHub](https://img.shields.io/badge/GitHub-Profile-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/ur-chemist)

</div>

---

<div align="center">

*Fully open science — dataset, code, figures, and preprint all publicly archived*

</div>
