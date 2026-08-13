<div align="center">

# 🔬 UV-Vis Pesticide Chemometrics

> **Multi-analyte UV-Vis spectroscopic analysis of pesticides in complex environmental water matrices using chemometric methods and statistical quality control**

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Excel](https://img.shields.io/badge/Excel-Analysis-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)](https://microsoft.com/excel)
[![SQLite](https://img.shields.io/badge/SQLite-Database-003B57?style=for-the-badge&logo=sqlite&logoColor=white)](https://sqlite.org)
[![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)](https://scikit-learn.org)
[![Pandas](https://img.shields.io/badge/Pandas-Data-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org)

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21911163.svg)](https://doi.org/10.5281/zenodo.21911163)
![Samples](https://img.shields.io/badge/Samples-846%20spectra-orange?style=flat-square)
![Wavelength](https://img.shields.io/badge/Wavelength-200–737.5%20nm-blueviolet?style=flat-square)
![Status](https://img.shields.io/badge/Status-Portfolio%20Project-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square)

</div>

---

## 📌 Overview

This project applies **UV-Vis spectroscopy** and **chemometric analysis** to detect and quantify pesticide residues in complex environmental water samples. The dataset contains 846 full-range spectra (200–737.5 nm) collected from water samples with varying concentrations of four analytes — **Bromide, Fluopyram, Diflufenican, and Mesosulfuron** — across multiple ports, seasons, and soil types.

The analytical pipeline covers the complete QA/QC workflow: data cleaning, spectral sorting, λmax identification, regression-based calibration, LOD/LOQ calculation, and a multi-wavelength selectivity assessment. The project bridges wet-chemistry analytical thinking with data analysis tools, demonstrating how a QA analyst would approach a real environmental monitoring dataset.

| Property | Detail |
|---|---|
| 📂 Dataset | Environmental water UV-Vis spectra (real field samples) |
| 📊 Samples | 846 spectra × 216 clean wavelength channels |
| 🌊 Wavelength Range | 200 – 737.5 nm (2.5 nm resolution) |
| 🧪 Target Analytes | Bromide (Br⁻), Fluopyram, Diflufenican, Mesosulfuron |
| 📍 Sample Variables | Port (1–12), Season, Soil Type, Storm event |
| 🗄️ DOI | [10.5281/zenodo.21911163](https://doi.org/10.5281/zenodo.21911163) |

---

## 📁 Project Structure

```
uv-vis-pesticide-chemometrics/
│
├── 📓 Analysis.ipynb                     # Main Python notebook: EDA, sorting, modeling
├── 📊 uv_vis_projects_.xlsx              # Raw dataset (846 spectra, 230 columns)
├── 📋 UVVis_Analysis.xlsx               # Processed workbook with all analysis sheets:
│   ├── Chemcial_Data_for_PLS            #   Cleaned + sorted main data (NaN cols removed)
│   ├── Fig1                             #   4 representative spectra for plotting
│   └── Calc                             #   All formulas: λmax, calibration, LOD/LOQ, selectivity
└── 📘 README.md
```

---

## 🔬 Methodology

### 0️⃣ Data Cleaning
- Loaded raw `.xlsx` file with 846 samples × 230 columns
- Identified and deleted 5 trailing NaN wavelength columns (740–750 nm)
- Final clean range: **200–737.5 nm (216 channels)**
- Verified no remaining NaN values in spectral region

### 1️⃣ Spectral Sorting & Sample Selection (Figure 1)
- Sorted all 846 rows by **Fluopyram concentration** (Z→A) to reveal the full concentration gradient
- Selected 4 representative spectra for Figure 1:

| Row | Fluopyram (mg/L) | Description |
|---|---|---|
| 2 | **6.16** | High concentration sample |
| 423 | **0.043** | Medium concentration sample |
| 845 | **0.000** | Blank / zero (replicate 1) |
| 846 | **0.000** | Blank / zero (replicate 2) |

### 2️⃣ Peak Identification — λmax

> Formula: `=INDEX(J1:HQ1, MATCH(MAX(J2:HQ2), J2:HQ2, 0))`

The λmax of the high-concentration sample falls at **202.5 nm**, consistent with strong UV absorption at short wavelengths typical of aromatic pesticide chromophores and matrix components in environmental water.

### 3️⃣ Calibration Curve — OLS Regression

Ordinary Least Squares regression (Fluopyram concentration vs. absorbance at 220 nm) using `LINEST`:

| Statistic | Formula | Purpose |
|---|---|---|
| Slope (S) | `=INDEX(LINEST(...),1,1)` | Sensitivity (Abs per mg/L) |
| Intercept | `=INDEX(LINEST(...),2,1)` | Blank signal offset |
| R² | `=INDEX(LINEST(...),3,1)` | Goodness of fit |
| SE of Slope | `=INDEX(LINEST(...),1,2)` | Slope precision |
| SE of Intercept (σ) | `=INDEX(LINEST(...),2,2)` | Input to LOD/LOQ |

> ⚠️ **Honest note on R²:** The correlation between a single-compound concentration and total absorbance at 220 nm is modest in this dataset. This is expected in complex environmental matrices — the 220 nm signal is shared by multiple UV-absorbing species. In a real QA setting, this finding would prompt method refinement (e.g., PLS regression across the full spectral range rather than univariate calibration at one wavelength). The formulas are correct; the data is telling the real story.

### 4️⃣ LOD & LOQ

Calculated from LINEST output directly in the Calc sheet:

```
LOD = 3.3 × σ / S
LOQ = 10.0 × σ / S
```

Where `σ` = standard error of the intercept, `S` = slope. Both are live-linked formulas — changing the calibration range updates LOD/LOQ automatically.

### 5️⃣ Selectivity Assessment

Pearson correlation coefficients between each analyte's concentration and absorbance at five diagnostic wavelengths:

| Wavelength | Column | Analytical Significance |
|---|---|---|
| 220 nm | R | Strong UV absorbers; matrix-sensitive |
| 230 nm | V | Aromatic n→π* transitions |
| 255 nm | AF | Characteristic for some pesticide chromophores |
| 270 nm | AL | Benzene ring absorption region |
| 300 nm | AX | Extended aromatic systems |

> **Expected pattern:** Pesticide rows should show structured UV correlations; Bromide (no UV chromophore) should approach zero. Complex environmental matrices often show attenuated contrasts due to spectral interference — which is itself a scientifically important finding.

---
<p align="center">
  <img src="images/figure1_spectra.png" alt="Spectral Overlay at 202.5 nm" width="48%" />
  <img src="images/Calibaration_Curve.png" alt="Univariate Calibration Curve" width="48%" />
</p> 📊 Key Findings

```
🔬 λmax         →   202.5 nm (high-concentration sample, consistent with UV-active matrix)
📉 R² (OLS)    →   Modest at single wavelength — confirms need for multivariate methods
🧂 Bromide      →   Measurable background correlation due to co-eluting matrix species
🌱 Pesticides   →   UV-region signals detectable; multivariate calibration recommended
📐 LOD/LOQ      →   Calculated directly from LINEST SE of intercept (live formula)
```

---

## 🛠️ Tools & Libraries

| Tool | Purpose |
|---|---|
| 🐍 Python + Pandas | Data loading, EDA, correlation analysis |
| 📊 openpyxl | Multi-sheet workbook construction and formula writing |
| 🤖 Scikit-learn | Regression modeling |
| 📈 Matplotlib / Seaborn | Spectral visualization |
| 📉 Excel LINEST | OLS regression statistics with full error output |
| 🗄️ SQLite | Results storage and threshold querying |
| 📓 Jupyter Notebook | Reproducible analysis environment |

---

## ⚠️ Limitations & Next Steps

This is a **student portfolio project** on a real field dataset. Known limitations are documented, not hidden:

- [ ] **Univariate calibration is insufficient** for complex environmental matrices — PLS (Partial Least Squares) across the full spectrum is the appropriate next method
- [ ] **Single wavelength R²** is modest — expected, not a bug in the code
- [ ] **LOD/LOQ** should be validated against certified reference standards in a real lab setting
- [ ] **Selectivity table** uses CORREL, which assumes linearity — non-linear interactions exist in real matrices
- [ ] **Next addition:** PLS regression across all 216 wavelengths using scikit-learn `PLSRegression`
- [ ] **Next addition:** Confusion matrix and cross-validated accuracy for classification of contaminated vs. clean samples

---

## 💡 What This Project Demonstrates

- Reading OLS output critically — not just R², but slope direction, p-values, and what modest fit means in a real matrix context
- Building multi-sheet Excel workbooks programmatically with live, linked formulas
- Applying Beer-Lambert thinking to a real environmental dataset with interferences
- Writing analytical reports that separate what the data shows from what the model cannot claim
- Connecting Python pipeline output to SQL-structured results storage

---

## 📖 Citation

If you use this dataset or analysis in your own work, please cite:

```bibtex
@misc{ubaid2025uvvis,
  author       = {Ubaid Ur Rehman},
  title        = {UV-Vis Pesticide Chemometrics: Multi-analyte spectroscopic 
                  analysis of environmental water samples},
  year         = {2025},
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

*Targeting roles in Quality Assurance, Analytical Chemistry, and Data-driven R&D*

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/ubaid-ur-rehman-chemist)
[![GitHub](https://img.shields.io/badge/GitHub-Profile-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/ur-chemist)

</div>

---

<div align="center">

*Built as part of a portfolio demonstrating the intersection of analytical chemistry and data science — for QA, R&D, and environmental monitoring roles*

</div>
