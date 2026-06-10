# Transition-Based Digital Twin Modelling for Alzheimer's Disease

This repository contains the code used for the study:

**Transition-Based Digital Twin Modelling for Alzheimer's Disease under Sparse Longitudinal Data**

The full paper has been accepted by the **International Conference on AI in Healthcare (AIiH)**.  
A preprint is available on arXiv: [Transition-Based Digital Twin Modelling for Alzheimer's Disease under Sparse Longitudinal Data](https://arxiv.org/abs/2606.09671).

The study develops a personalised digital twin framework for Alzheimer's disease (AD) progression modelling using sparse longitudinal multimodal data from the Alzheimer's Disease Neuroimaging Initiative (ADNI). The framework compares a transition-based MLP model with a sequence-based BiLSTM-Attention model for cognitive score prediction, diagnosis classification, uncertainty-aware forecasting, and subject-level what-if analysis.

## Repository Structure

The current repository contains the following files:

```text
.
├── README.md        # Project description and usage guide
├── data.txt         # Data description or processed data information
├── feature.ipynb    # Feature preprocessing and feature selection
├── lstm.ipynb       # BiLSTM-Attention sequence modelling branch
└── mlp.ipynb        # Transition-based MLP branch
````

## Project Overview

Alzheimer's disease progression is heterogeneous and is often observed through sparse and irregular follow-up visits. Many existing machine learning approaches focus on static classification or cohort-level risk estimation, which limits their ability to support individualised monitoring and subject-level trajectory analysis.

This project proposes a personalised digital twin framework that supports subject-level prediction, uncertainty-aware forecasting, and scenario-based what-if analysis. The framework includes two complementary modelling branches:

1. **Transition-based MLP branch**
   This branch models adjacent clinical transitions, such as `t -> t + 6 months`. It uses the current visit's clinical, cognitive, and imaging-derived features to predict the next visit's MMSE score and diagnosis category.

2. **BiLSTM-Attention branch**
   This branch models longer-range temporal dependencies across aligned follow-up visits. It supports sequence-based forecasting, predictive uncertainty estimation, and patient-level what-if trajectory analysis.

The main finding is that, under sparse and irregular ADNI follow-up conditions, local transition modelling can be more data-efficient and robust than relying only on sequence-based temporal modelling.

## Dataset

The study uses data from the [Alzheimer's Disease Neuroimaging Initiative (ADNI)](https://adni.loni.usc.edu), including:

* clinical variables;
* cognitive assessments;
* diagnosis labels;
* MRI-derived structural phenotypes.

The main prediction targets are:

* **MMSE**: Mini-Mental State Examination score for regression;
* **DX**: diagnosis category for classification:

  * CN: cognitively normal;
  * MCI: mild cognitive impairment;
  * AD: Alzheimer's disease.

ADNI data are not included in this repository. Users must apply for access through the official ADNI data access process and prepare the data according to the preprocessing pipeline used in the notebooks.

## File Description

### `data.txt`

This file provides information about the dataset used in the study. It may include data source notes, variable descriptions, or processed data instructions.

### `feature.ipynb`

This notebook is used for feature processing and feature selection. It includes steps such as:

* loading and cleaning longitudinal ADNI data;
* handling clinical, cognitive, and imaging-derived variables;
* preparing static and dynamic features;
* applying feature selection, such as mRMR;
* generating the feature sets used by the prediction models.

### `mlp.ipynb`

This notebook implements the transition-based MLP branch. It includes:

* adjacent visit-pair construction;
* MMSE regression;
* diagnosis classification;
* model training and validation;
* evaluation using RMSE, MAE, accuracy, AUC, and macro-F1;
* comparison with conventional classifiers where applicable.

### `lstm.ipynb`

This notebook implements the sequence-based BiLSTM-Attention branch. It includes:

* construction of aligned subject-level longitudinal sequences;
* BiLSTM-Attention model training;
* MMSE forecasting;
* diagnosis classification;
* Monte Carlo dropout for uncertainty estimation;
* subject-level trajectory forecasting;
* patient-level what-if analysis.

## Main Results

The transition-based MLP branch achieved stronger held-out test performance than the BiLSTM-Attention branch.

| Model            |  RMSE |   MAE |   ACC |   AUC | Macro-F1 |
| ---------------- | ----: | ----: | ----: | ----: | -------: |
| MLP              | 2.149 | 1.529 | 0.906 | 0.976 |    0.908 |
| BiLSTM-Attention | 2.687 | 1.926 | 0.806 | 0.928 |    0.798 |

These results suggest that, when longitudinal clinical data are sparse and irregular, modelling local transitions between adjacent visits may provide a more robust and data-efficient strategy than learning full long-range temporal sequences.

## Publication

The full paper corresponding to this repository has been accepted by the **International Conference on AI in Healthcare (AIiH)**.

Preprint: [Transition-Based Digital Twin Modelling for Alzheimer's Disease under Sparse Longitudinal Data](https://arxiv.org/abs/2606.09671)

Please cite the paper if you use this repository or build upon this work.

## Citation

If you use this code or find this work useful, please cite:

```bibtex
@article{huang2026transition,
  title={Transition-Based Digital Twin Modelling for Alzheimer's Disease under Sparse Longitudinal Data},
  author={Huang, Yinyu and Zhang, Yilin and Michopoulou, Sofia and Kipps, Christopher and Attar, Rahman},
  journal={arXiv preprint arXiv:2606.09671},
  year={2026},
  note={Accepted by the International Conference on AI in Healthcare (AIiH)}
}
```

Please update the BibTeX entry with the final publication details once available.

## How to Use

### 1. Clone the repository

```bash
git clone https://github.com/YilinZhang00/Digital-Twin-for-AD.git
cd Digital-Twin-for-AD
```

### 2. Create an environment

For example, using Conda:

```bash
conda create -n ad-digital-twin python=3.13
conda activate ad-digital-twin
```

Install the required Python packages manually according to the notebook imports. A `requirements.txt` file can be added later for easier installation.

Common packages may include:

```bash
pip install numpy pandas scikit-learn matplotlib seaborn torch
```

Additional packages may be required depending on the feature selection and visualisation steps.

### 3. Prepare the data

Download the required ADNI data through the official ADNI portal. Then prepare the data according to the format expected by the notebooks.

The processed data should include:

* subject identifier;
* visit time or visit month;
* clinical and cognitive variables;
* MRI-derived structural features;
* MMSE target;
* diagnosis target.

### 4. Run the notebooks

The suggested order is:

```text
1. feature.ipynb
2. mlp.ipynb
3. lstm.ipynb
```

Run `feature.ipynb` first to prepare the feature set. Then run `mlp.ipynb` for the transition-based branch and `lstm.ipynb` for the sequence-based branch.

## Outputs

The notebooks can generate:

* MMSE prediction results;
* diagnosis classification results;
* feature selection results;
* ROC curves;
* residual plots;
* reliability plots;
* patient-level predicted trajectories;
* uncertainty intervals;
* what-if trajectory visualisations.

## Key Findings

* The transition-based MLP branch achieved the strongest point-prediction performance.
* mRMR feature selection improved both regression and classification results.
* The BiLSTM-Attention branch remained useful for uncertainty-aware forecasting and subject-level what-if analysis.
* The results show that temporal modelling strategies should match the sparsity and irregularity of real-world clinical data.

## Limitations

This study has several limitations:

* it uses a single longitudinal cohort;
* the sequence-based branch depends on interpolation for irregular follow-up;
* what-if analysis is observational rather than causal;
* the MLP and BiLSTM branches differ in prediction horizon and effective sample construction;
* external validation on independent cohorts is needed.

## Acknowledgements

This research was supported by the National Institute for Health and Care Research (NIHR).

## Disclosure of Interests

The authors have no competing interests to declare that are relevant to the content of this article.

```
```
