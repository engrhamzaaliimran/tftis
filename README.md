# TFTIS

## Temperature–Frequency-Tuned Impedance Spectroscopy for MOS Gas Sensing

This repository provides measurement data and fully reproducible analysis workflows for **temperature–frequency-tuned impedance spectroscopy (TFT-IS)** applied to metal-oxide-semiconductor (MOS) gas sensors.

TFT-IS combines impedance spectra measured across multiple sensor operating temperatures to capture temperature-dependent frequency responses. In the provided workflows, the imaginary impedance component, (Z''(f)), is fitted with an exponentially modified Gaussian (EMG) model. The fitted parameters from three operating temperatures are combined into a compact 12-dimensional representation for each measurement sweep.

The repository currently includes datasets and Google Colab notebooks for:

* ethanol–acetone gas-mixture measurements;
* single-gas measurements, including ethanol, acetone, hydrogen, and zero-air reference conditions;
* reproducible EMG feature extraction and principal-component analysis (PCA).

---

## Repository Contents

```text
tftis/
├── README.md
├── TFTIS_GasMixtures_Results_Reproduce.ipynb
├── TFTIS_SingleGas_Results_Reproduce.ipynb
├── mixtures-data.zip
└── single-gas-data/
    ├── Gas_Acetone/
    ├── Gas_Ethanol/
    ├── Gas_H2/
    └── Gas_ZeroAir/
```

| File or folder                              | Description                                                                                                     |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| `TFTIS_GasMixtures_Results_Reproduce.ipynb` | Google Colab notebook that reproduces EMG feature extraction and PCA analysis for ethanol–acetone gas mixtures. |
| `TFTIS_SingleGas_Results_Reproduce.ipynb`   | Google Colab notebook that reproduces the single-gas EMG feature extraction and pairwise PCA visualization.     |
| `mixtures-data.zip`                         | Compressed HDF5 dataset containing dense impedance-spectroscopy measurements for ethanol–acetone gas mixtures.  |
| `single-gas-data/`                          | Curated HDF5 impedance datasets for acetone, ethanol, hydrogen, and zero-air reference measurements.            |
| `README.md`                                 | Repository overview, dataset description, and reproduction instructions.                                        |

---

## Dataset Overview

### Ethanol–Acetone Gas Mixtures

The gas-mixture dataset contains dense impedance-spectroscopy measurements acquired at three sensor operating temperatures:

* 150 °C
* 250 °C
* 350 °C

The accompanying notebook extracts a 12-dimensional EMG representation from the spectra and produces a PCA visualization of the ethanol–acetone mixture feature space.

### Single-Gas Measurements

The `single-gas-data/` directory contains curated HDF5 measurements organized by:

* gas species;
* relative humidity;
* sensor operating temperature;
* gas concentration.

The currently included gases and reference conditions are:

* acetone;
* ethanol;
* hydrogen;
* zero air.

Measurements are organized across the available humidity levels and the three operating temperatures of 150 °C, 250 °C, and 350 °C.

---

## Analysis Method

Both notebooks use the same core TFT-IS feature-extraction concept.

1. The imaginary impedance component is calculated as:

   [
   Z'' = -|Z|\sin(\phi)
   ]

2. Each impedance sweep is fitted using an exponentially modified Gaussian (EMG) model.

3. Four EMG parameters are extracted at each operating temperature:

   * amplitude, (A);
   * peak position, (mu);
   * width, (sigma);
   * asymmetry parameter, (lambda).

4. The parameters from 150 °C, 250 °C, and 350 °C are concatenated to form a 12-dimensional feature vector:

   [
   [A,mu,sigma,lambda]*{150C}
   +
   [A,mu,sigma,lambda]*{250C}
   +
   [A,mu,sigma,lambda]*{350C}
   ]

5. The resulting feature data are standardized and visualized using PCA.

Each selected sweep is retained as an individual observation. No median aggregation is applied across sweeps.

---

## Reproducing the Gas-Mixture Results

Open the notebook directly in Google Colab:

[Open the gas-mixture notebook in Google Colab](https://colab.research.google.com/github/engrhamzaaliimran/tftis/blob/main/TFTIS_GasMixtures_Results_Reproduce.ipynb)

The notebook performs the following steps:

1. Clones this repository.
2. Loads and extracts the compressed gas-mixture HDF5 dataset.
3. Fits the EMG model to the impedance sweeps.
4. Creates the dense 12-dimensional EMG feature dataset.
5. Generates a PCA visualization of ethanol–acetone mixture structure.

Typical generated outputs include:

```text
emg_feature_dataset/
├── dense_full_EMG_12D_sweep_features.csv
└── dense_full_EMG_fit_quality_long.csv

MIXTURE_PCA_2D_ethanol_color_acetone_marker_clean.png
```

---

## Reproducing the Single-Gas Results

Open the notebook directly in Google Colab:

[Open the single-gas notebook in Google Colab](https://colab.research.google.com/github/engrhamzaaliimran/tftis/blob/main/TFTIS_SingleGas_Results_Reproduce.ipynb)

The single-gas workflow:

1. Clones this repository.
2. Reads the curated HDF5 files recursively from `single-gas-data/`.
3. Uses the first 35 sweeps from each HDF5 measurement file.
4. Fits the EMG model independently to each selected sweep.
5. Combines matching 150 °C, 250 °C, and 350 °C sweeps into 12-dimensional feature vectors.
6. Performs standardized PCA.
7. Generates a pairwise PCA visualization using PC1 and PC4.

Typical generated outputs include:

```text
emg12_first35_results/
├── EMG_12feat_Combined_AllGases.csv
├── PCA_scores_first35.csv
├── pca_explained_variance.csv
├── PCA_PAIRWISE_PCA_Pairwise_PC1_PC4.png
├── PCA_PAIRWISE_PCA_Pairwise_PC1_PC4.eps
├── source_file_manifest.csv
├── condition_merge_manifest.csv
├── fit_summary_by_file.csv
└── incomplete_or_skipped_conditions.csv
```

The `incomplete_or_skipped_conditions.csv` file documents measurements that cannot be combined into a complete 150 °C–250 °C–350 °C condition set or that could not be processed.

---

## Running Locally

The notebooks are designed to run directly in a standard Google Colab environment without manually uploading the datasets.

For local execution, clone the repository:

```bash
git clone https://github.com/engrhamzaaliimran/tftis.git
cd tftis
```

Then open either notebook using Jupyter Notebook, JupyterLab, or VS Code. The workflows use standard scientific Python packages, including:

```text
numpy
pandas
scipy
scikit-learn
matplotlib
h5py
```

---

## Intended Use

This repository is intended to support reproducible research on TFT-IS-based gas sensing, compact impedance-spectrum representations, and multi-temperature sensor operation.

The included notebooks provide a transparent starting point for:

* reproducing the reported PCA visualizations;
* inspecting raw HDF5 impedance measurements;
* extracting EMG-based spectral features;
* extending the analysis to additional gases, humidity conditions, sensors, or frequency-selection strategies.

---

## Contact

For questions, feedback, or collaboration related to this repository, please contact:

**Hamza Ali Imran**
[h.imran@lmt.uni-saarland.de](mailto:h.imran@lmt.uni-saarland.de)
