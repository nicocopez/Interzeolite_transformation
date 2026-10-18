# Raman Spectra Preprocessing and NNLS in Python

A reproducible workflow for preprocessing Raman spectra and following phase evolution using **Non-Negative Least Squares (NNLS)**.

This notebook accompanies the work:

**N. Coca-Lopez, F. Rodriguez, L. F. Lundegaard, X. Li, E. Lozano Diz, S. B. Rasmussen, F. Tielens, M. Calatayud, M. A. Bañares, R. Portela and P. Beato, “Process monitoring of zeolite crystallization: Bridging in situ Raman spectroscopy, ex situ XRD, and predictive machine learning models”, Chemical Engineering Journal 543 (2026) 177865.**

DOI: https://doi.org/10.1016/j.cej.2026.177865

The open experimental dataset is available on Zenodo:

https://doi.org/10.5281/zenodo.18972297

## What does the notebook do?

The notebook uses real in situ Raman spectra acquired during the crystallization of zeolites and follows the transformation from **faujasite (FAU)** to **gismondine (GIS)**.

The complete Raman-processing workflow is:

**raw Raman spectra → spectral crop/harmonization → spike removal → baseline subtraction → diagnostic-region crop → normalization → NNLS**

The final NNLS model describes each Raman spectrum as a non-negative linear combination of representative FAU and GIS reference spectra, allowing their relative spectral contributions to be followed throughout crystallization.

## Dataset

The notebook uses:

`raman_crystallization_solid.nxs`

from the Zenodo dataset:

**Process Monitoring of Zeolite Crystallization: Bridging In situ Raman Spectroscopy, Ex situ XRD, and Predictive Machine Learning Models**

https://doi.org/10.5281/zenodo.18972297

The spectra are stored in **NeXus/HDF5** format together with their crystallization-time metadata.

Place `raman_crystallization_solid.nxs` in the same folder as the notebook before running it.

## Requirements

The workflow uses Python and the following packages:

```text
numpy
matplotlib
scipy
pybaselines
nexusformat
```

They can be installed with:

```bash
pip install numpy matplotlib scipy pybaselines nexusformat
```

## Main processing steps

### 1. Load the NeXus data

The Raman spectra and their corresponding crystallization times are extracted directly from the NeXus file and sorted chronologically.

### 2. Crop and harmonize the spectral axis

The spectra are restricted to the Raman region relevant to the analysis.

If necessary, spectra acquired on slightly different Raman-shift grids are interpolated onto a common spectral axis before they are assembled into a single data matrix.

### 3. Spike removal

Narrow positive and negative spikes are detected using their **peak prominence and width**.

The method is based on:

**N. Coca-Lopez, “An intuitive approach for spike removal in Raman spectra based on peaks' prominence and width”, Analytica Chimica Acta 1295 (2024) 342312.**

https://doi.org/10.1016/j.aca.2024.342312

Detected spike regions are conservatively replaced by interpolation from neighboring unaffected points.

### 4. Baseline subtraction

A slowly varying spectral background is removed using **Asymmetric Least Squares (AsLS)** baseline estimation.

### 5. Diagnostic Raman region

The spectra are restricted to the region containing the main spectral differences between FAU and GIS and normalized before spectral unmixing.

### 6. Non-Negative Least Squares

Each measured spectrum is modeled as:

\[
y \approx C_{\mathrm{FAU}}S_{\mathrm{FAU}}
+ C_{\mathrm{GIS}}S_{\mathrm{GIS}}
\]

with:

\[
C_{\mathrm{FAU}}, C_{\mathrm{GIS}} \geq 0
\]

Representative spectra acquired at **18 h** and **144 h** are used as the FAU and GIS references, respectively.

The NNLS coefficients are normalized to obtain the **relative Raman contributions** of both phases as a function of crystallization time.

## Why NNLS?

NNLS provides a simple and transparent approach to spectral unmixing when representative component spectra are available.

Rather than relying on a complex predictive model, the measured Raman spectrum is represented directly as a combination of physically interpretable reference spectra.

Although this notebook focuses on the FAU → GIS interzeolite transformation, the same general approach can be applied to other Raman or vibrational-spectroscopy problems involving evolving mixtures, phase transformations, crystallization processes or chemical reactions.

## Citation

If you use the dataset, workflow or scientific results, please cite:

N. Coca-Lopez, F. Rodriguez, L. F. Lundegaard, X. Li, E. Lozano Diz, S. B. Rasmussen, F. Tielens, M. Calatayud, M. A. Bañares, R. Portela and P. Beato,  
**Process monitoring of zeolite crystallization: Bridging in situ Raman spectroscopy, ex situ XRD, and predictive machine learning models**,  
*Chemical Engineering Journal* **543** (2026) 177865.  
https://doi.org/10.1016/j.cej.2026.177865

For the spike-removal methodology:

N. Coca-Lopez,  
**An intuitive approach for spike removal in Raman spectra based on peaks' prominence and width**,  
*Analytica Chimica Acta* **1295** (2024) 342312.  
https://doi.org/10.1016/j.aca.2024.342312

## Data availability

The complete experimental dataset is openly available at:

https://doi.org/10.5281/zenodo.18972297
