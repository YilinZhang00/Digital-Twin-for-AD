# Transition-Based Digital Twin Modelling for Alzheimer's Disease

This repository contains the code for the study:

**Transition-Based Digital Twin Modelling for Alzheimer's Disease under Sparse Longitudinal Data**

The project develops a personalised digital twin framework for Alzheimer's disease (AD) progression modelling using sparse and irregular longitudinal multimodal data. The framework combines transition-based prediction and sequence-based trajectory forecasting to support cognitive score prediction, diagnosis classification, uncertainty-aware forecasting, and subject-level what-if analysis.

## Overview

Alzheimer's disease progression is highly heterogeneous and is often observed through sparse, irregular follow-up visits. Many existing machine learning approaches focus on static classification or cohort-level risk prediction, which limits their usefulness for individualised monitoring and scenario-based analysis.

This study proposes a hybrid digital twin framework that models disease progression at the subject level. The framework includes:

- a transition-based MLP branch for short-term prediction between adjacent visits;
- a BiLSTM-Attention branch for longer-range temporal forecasting;
- uncertainty estimation using Monte Carlo dropout;
- patient-specific what-if trajectory analysis under controlled perturbations of clinically meaningful variables.

The main finding is that, under sparse and irregular ADNI follow-up conditions, local transition modelling can be more data-efficient and robust than relying only on sequence-based temporal models.

## Dataset

The study uses data from the Alzheimer's Disease Neuroimaging Initiative (ADNI), including:

- cognitive assessments;
- clinical variables;
- diagnosis labels;
- MRI-derived structural phenotypes from FreeSurfer.

After preprocessing, the working dataset contained 2,801 records from 760 subjects, aligned to six-month follow-up intervals from baseline to 60 months.

Targets used in the study:

- **MMSE** for cognitive score regression;
- **DX** for three-class diagnosis classification:
  - cognitively normal (CN);
  - mild cognitive impairment (MCI);
  - Alzheimer's disease (AD).

Please note that ADNI data are not included in this repository. Users must obtain access through the official ADNI data access process and prepare the data according to the preprocessing steps described in the paper.

## Method

The proposed framework contains two complementary branches.

### 1. Transition-Based MLP Branch

The MLP branch models adjacent visit transitions, such as:

```text
t -> t + 6 months
```

For each adjacent visit pair, the input contains the current visit's clinical, cognitive, and imaging features, and the targets are the next visit's MMSE and DX.

This branch is designed for robust short-horizon prediction under sparse longitudinal data. Feature selection is performed using minimum Redundancy Maximum Relevance (mRMR).

### 2. BiLSTM-Attention Branch

The BiLSTM-Attention branch models longer-range longitudinal dependencies across aligned follow-up visits. It uses observations up to month 18 to predict the 24-month endpoint.

This branch supports:

- sequence-based MMSE forecasting;
- diagnosis prediction;
- predictive uncertainty estimation;
- subject-specific what-if trajectory simulation.

Although this branch achieved lower point-prediction performance than the MLP branch, it provides temporally coherent trajectories and uncertainty-aware analysis, which are important for the digital twin interpretation.

## Main Results

On the held-out test set, the transition-based MLP branch outperformed the BiLSTM-Attention branch.

| Model | RMSE | MAE | ACC | AUC | Macro-F1 |
|---|---:|---:|---:|---:|---:|
| MLP | 2.149 | 1.529 | 0.906 | 0.976 | 0.908 |
| BiLSTM-Attention | 2.687 | 1.926 | 0.806 | 0.928 | 0.798 |

These results suggest that local transition modelling may be more suitable than full sequence modelling when longitudinal clinical data are sparse, irregular, and limited in sample size.

## Repository Structure

A suggested structure for this repository is:

```text
.
├── README.md
├── requirements.txt
├── data/
│   └── README.md
├── src/
│   ├── preprocessing/
│   ├── models/
│   ├── training/
│   ├── evaluation/
│   └── visualisation/
├── configs/
├── results/
└── figures/
```

Depending on the final code organisation, the exact file structure may differ.

## Installation

Clone the repository:

```bash
git clone https://github.com/YilinZhang00/Digital-Twin-for-AD.git
cd Digital-Twin-for-AD
```

Create a virtual environment:

```bash
conda create -n ad-digital-twin python=3.10
conda activate ad-digital-twin
```

Install dependencies:

```bash
pip install -r requirements.txt
```

## Usage

### 1. Prepare the data

Download the required ADNI data through the official ADNI portal. Then preprocess the clinical, cognitive, and MRI-derived features into the format expected by the training scripts.

The processed data should include:

- subject identifier;
- visit month;
- clinical and cognitive variables;
- MRI-derived structural features;
- MMSE target;
- DX diagnosis target.

### 2. Train the transition-based MLP branch

Example:

```bash
python src/training/train_mlp_transition.py --config configs/mlp_transition.yaml
```

### 3. Train the BiLSTM-Attention branch

Example:

```bash
python src/training/train_bilstm_attention.py --config configs/bilstm_attention.yaml
```

### 4. Evaluate the models

Example:

```bash
python src/evaluation/evaluate_models.py --config configs/evaluation.yaml
```

### 5. Run uncertainty-aware and what-if analysis

Example:

```bash
python src/evaluation/run_what_if_analysis.py --config configs/what_if.yaml
```

The command names above are templates and should be adapted to the final script names in the repository.

## Outputs

The framework can generate:

- MMSE regression results;
- diagnosis classification results;
- ROC curves;
- residual plots;
- reliability plots;
- patient-level predicted trajectories;
- uncertainty intervals;
- what-if trajectory visualisations.

## Key Findings

- Transition-based modelling achieved stronger predictive performance than sequence-based modelling in the current sparse ADNI setting.
- Feature selection using mRMR substantially improved both regression and classification performance.
- The sequence-based branch remained useful for uncertainty-aware forecasting and subject-level scenario analysis.
- The results highlight that temporal model design should match the structure and limitations of real-world clinical data.

## Limitations

This study has several limitations:

- it uses a single longitudinal cohort;
- the sequence branch depends on interpolation for irregular follow-up;
- what-if analysis is observational rather than causal;
- the MLP and BiLSTM branches differ in prediction horizon and effective sample construction;
- external validation on independent cohorts is needed.

## Citation

If you use this repository, please cite the associated paper:

```bibtex
@inproceedings{huang2026transition,
  title={Transition-Based Digital Twin Modelling for Alzheimer's Disease under Sparse Longitudinal Data},
  author={Huang, Yinyu and Zhang, Yilin and Michopoulou, Sofia and Kipps, Christopher and Attar, Rahman},
  booktitle={International Conference on AI in Healthcare},
  year={2026}
}
```

Please update the BibTeX entry with the final proceedings information once available.

## Acknowledgements

This research was supported by the National Institute for Health and Care Research (NIHR). Rahman Attar is supported by the NIHR Southampton Biomedical Research Centre. Sofia Michopoulou is supported by an NIHR Senior Clinical Practitioner Award.

## License

Please add the appropriate license for the repository before public release. If no license is provided, the default copyright restrictions apply.
