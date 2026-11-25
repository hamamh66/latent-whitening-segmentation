# Latent Space Whitening for Robust Medical Image Segmentation

Code and results for **block-based Cholesky whitening at the bottleneck of 3D segmentation networks**, evaluated for robustness to additive Gaussian noise and isotropic Gaussian blur.

## What is studied

Three bottleneck conditioning strategies are compared in two volumetric backbones (3D U-Net and 3D ResUNet):

- **Baseline**: no conditioning.
- **STD**: channel-wise standardization (diagonal variance scaling).
- **LOP (Latent Orthogonal Projection)**: full cross-channel whitening of the empirical channel covariance by inverse Cholesky factorization, with shrinkage toward a scaled identity.

A target-free volume regularization loss (**VolReg**), which penalizes the mini-batch variance of the latent log-determinant, is also evaluated on the 3D ResUNet backbone.

## Data

Medical Segmentation Decathlon, Task 04 (Hippocampus, T1-weighted MRI; anterior and posterior structures), publicly available at <http://medicaldecathlon.com>. The dataset is not redistributed here.

## Protocol

- Training: AdamW (learning rate 3e-4, weight decay 1e-4), batch size 4, 80 epochs.
- Stress tests: additive Gaussian noise (σ = 0.05, 0.10, 0.15) and isotropic Gaussian blur (σ = 0.5, 1.0, 1.5), plus the uncorrupted condition.
- Comparisons: whiten vs. baseline, whiten vs. STD, STD vs. baseline, VolReg vs. baseline.
- Statistics: ten seeds; per-seed paired Cohen's d_z on per-volume Dice differences; percentile bootstrap (2,000 resamples) over the seed-level effects; mask failure rates reported separately.

## Repository contents

| File | Content |
|---|---|
| `Preprocessing.ipynb` | Data loading and preprocessing of the Hippocampus volumes |
| `LOP_clean_v4.ipynb` | Models (baseline, STD, LOP, VolReg), multi-seed training, stress tests, statistics and figures |
| `robust_effects_10seeds.csv` | Effect sizes with bootstrap intervals, sign agreement and mask failure rates for every comparison, scenario, structure and backbone |

## Running

Open the notebooks in Jupyter or Google Colab, run `Preprocessing.ipynb` first, then `LOP_clean_v4.ipynb`. A GPU is recommended for training.

## Licence

MIT — see [LICENSE](LICENSE).
