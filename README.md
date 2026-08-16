# TriModalNet

Official reference implementation of **TriModalNet**, a multimodal deep-learning framework for solid-tumor survival prediction. TriModalNet integrates radiology, histopathology, and multi-omic genomics using six directed pairwise cross-modal attention pathways and hierarchical gating.

## Overview

The accompanying notebook provides the principal components described in the manuscript:

- cohort assembly from GDC, TCIA, TCGA-CDR, and CPTAC resources;
- preprocessing routines for radiology, whole-slide pathology, and multi-omic data;
- modality-specific radiology, pathology, and genomics encoders;
- six directed cross-modal attention pathways;
- edge-level and modality-level gating;
- discrete-time survival and ranking losses;
- patient-level five-fold training and evaluation routines;
- a minimal deterministic example for exercising the evaluation workflow; and
- inference routines for generating survival estimates and attention outputs.

## Architecture

TriModalNet combines three modality-specific representations:

1. **Radiology:** a 3-D ResNet-50 encoder initialized from MedicalNet/Med3D.
2. **Histopathology:** UNI ViT-L/16 tile representations aggregated using attention-based multiple-instance learning.
3. **Genomics:** a sparse denoising autoencoder for gene expression, mutation, and copy-number inputs.

The representations are projected into a shared 512-dimensional space. Six directed attention pathways model asymmetric interactions between radiology, pathology, and genomics. Hierarchical gates then combine the interaction features for discrete-time survival prediction.

## Repository contents

```text
TriModalNet/
├── TriModalNet_reference_implementation.ipynb
├── README.md
└── requirements.txt
```

## Installation

Python 3.11 is recommended.

```bash
git clone https://github.com/iraqmoon/TriModalNet.git
cd TriModalNet

python -m venv .venv
```

Activate the environment:

```bash
# Linux or macOS
source .venv/bin/activate

# Windows PowerShell
.venv\Scripts\Activate.ps1
```

Install the required packages:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

OpenSlide also requires a system library. On Ubuntu or Debian:

```bash
sudo apt-get update
sudo apt-get install -y libopenslide0
```

For GPU training, install the PyTorch build compatible with the CUDA version available on your system.

## Running the notebook

```bash
jupyter lab TriModalNet_reference_implementation.ipynb
```

The notebook can also be opened in Google Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/iraqmoon/TriModalNet/blob/main/TriModalNet_reference_implementation.ipynb)

The self-contained example and evaluation routines can be run without patient data. Cohort construction, preprocessing, model training, and patient inference require the corresponding source data, pretrained weights, and local data-loading configuration.

## Input modalities

| Modality | Expected representation |
|---|---|
| Radiology | Normalized `128 x 128 x 128` CT or MRI volume |
| Histopathology | Tiles of shape `N x 3 x 224 x 224` or pre-extracted UNI features of shape `N x 1024` |
| Genomics | A 1,737-element vector representing expression, mutation, and copy-number information for 579 genes |

Missing modalities are handled by learned missing-modality tokens.

## Data access

The notebook references data obtained from the Genomic Data Commons, The Cancer Imaging Archive, TCGA clinical outcome records, and CPTAC. Patient-level data are not included in this repository. Researchers must obtain the required datasets from the original providers and comply with their access, governance, consent, and data-use requirements.

Do not upload patient data, controlled-access genomic files, clinical identifiers, credentials, access tokens, or private institutional paths to a public repository.

## Pretrained weights

Full model training or inference requires the applicable pretrained weights and trained checkpoints, including:

- MedicalNet/Med3D initialization for the radiology encoder;
- UNI ViT-L/16 weights for pathology feature extraction; and
- trained TriModalNet checkpoints for patient-level inference.

These files are not redistributed. Users should obtain them from their authorized original sources and follow the corresponding license and access conditions.

## Minimal example and reproducibility

The notebook contains a small deterministic demonstration for running the evaluation and statistical routines without distributing patient-level data. This demonstration is provided to illustrate the software workflow; it is not a replacement for training and evaluation on the study cohorts.

The reference protocol uses:

- five patient-level folds;
- split, initialization, and augmentation seeds of `42`, `137`, and `271`;
- a 60-month prediction horizon divided into 20 three-month intervals;
- 1,000 bootstrap resamples; and
- 10,000 paired permutations.

Exact reproduction of patient-level results requires the authorized source data, cohort definitions, fold assignments, pretrained weights, and trained model checkpoints used in the study.

## Citation

If you use this implementation, please cite the associated manuscript. Replace the publication details after acceptance:

```bibtex
@article{trimodalnet,
  title   = {TriModalNet: Pairwise Cross-Modal Attention for Multimodal Cancer Survival Prediction},
  author  = {Author names},
  journal = {Journal name},
  year    = {Year},
  doi     = {DOI}
}
```

## Code availability

A reference implementation of TriModalNet, including the model architecture, preprocessing procedures, survival-loss functions, evaluation routines, and a minimal runnable example, is publicly available in this repository. Patient-level data are not redistributed because of the applicable data-use restrictions.

## License

Add an open-source license approved by all authors and relevant institutions before public release.

## Contact

For questions or bug reports, please open a GitHub issue or contact the corresponding author.
