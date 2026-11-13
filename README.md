# GHM-CMNet-CT-Deblurring

Official implementation of:

**GHM-CMNet: GHM Multiwavelet-Based CNN–Mamba Network for CT Image Motion Deblurring**

## Overview

GHM-CMNet is a CT image motion deblurring framework that combines the Geronimo–Hardin–Massopust (GHM) multiwavelet transform with complementary CNN- and Mamba-based feature processing.

The framework decomposes each input image into 16 GHM coefficient maps organized into low-, mixed-, and high-frequency groups. Corresponding subband features are processed through parallel CNN and Mamba branches and integrated using adaptive gated fusion.

The proposed architecture combines:

- GHM multiwavelet decomposition and reconstruction
- Parallel CNN and Mamba feature processing
- Adaptive gated feature fusion
- Low-frequency-guided alignment
- Frequency Collaborative Refinement (FCR)
- Band-specific residual coefficient prediction
- Full-resolution residual refinement
- Adversarial training

## Dataset

Experiments were conducted using the publicly available **MosMed-L** dataset, derived from MosMedData.

Two PNG slices were selected per patient. The data were split at the patient level with no patient overlap:

| Split | Patients | Images |
|---|---:|---:|
| Training | 777 | 1554 |
| Validation | 111 | 222 |
| Testing | 222 | 444 |

Images are single-channel grayscale CT images with a spatial resolution of 352 × 352 pixels.

## Experimental Protocol

The proposed model was trained using:

- Training patch size: `88 × 88`
- Training blur: synthetic linear motion blur, kernel length ∈ [3, 15] pixels
- Training blur orientation: uniformly sampled from [0°, 180°)
- Validation blur: kernel length = 15 pixels
- Test blur: kernel length = 15 pixels
- Validation and test blur orientations: deterministically generated
- Epochs: `30`
- Batch size: `2`
- Optimizer: Adam
- Generator learning rate: `1.5e-4`
- Discriminator learning rate: `1e-4`
- Adversarial loss weight: `0.002`
- Random seeds for independent runs: `123`, `456`, and `789`

The checkpoint with the highest validation PSNR based on EMA weights is used for final evaluation.

## Model Configuration

The full GHM-CMNet configuration uses:

- 16 GHM coefficient maps
- 40 band-feature channels
- 192 feature channels
- 8 CNN residual blocks
- 8 Mamba blocks
- Dual CNN–Mamba branches
- Adaptive gated fusion
- Low-frequency-guided alignment
- Frequency Collaborative Refinement
- Full-resolution refinement
- Adversarial training

## Evaluation

Final evaluation is performed on complete `352 × 352` test images degraded by synthetic linear motion blur with a kernel length of `15` pixels.

Four image-quality metrics are reported:

- PSNR ↑
- SSIM ↑
- LPIPS ↓
- DISTS ↓

## Results

In the primary evaluation using seed `123`, GHM-CMNet achieved:

| Metric | Result |
|---|---:|
| PSNR (dB) ↑ | 33.3550 |
| SSIM ↑ | 0.9535 |
| LPIPS ↓ | 0.0328 |
| DISTS ↓ | 0.0875 |

Across three independent training runs using seeds `123`, `456`, and `789`, GHM-CMNet achieved:

| Metric | Mean ± Std |
|---|---:|
| PSNR (dB) ↑ | 33.2142 ± 0.2418 |
| SSIM ↑ | 0.9528 ± 0.0009 |
| LPIPS ↓ | 0.0344 ± 0.0018 |
| DISTS ↓ | 0.0886 ± 0.0013 |

The proposed model was compared with FFTformer, MaIR, NAFNet, and SwinIR.

## Environment

The experiments were conducted using:

- Python 3.10.12
- PyTorch 2.11.0
- CUDA 12.8
- NVIDIA GeForce RTX 5070 Ti Laptop GPU
- WSL2 Ubuntu 22.04

Required Python packages are listed in:

```text
requirements.txt
```

Install them using:

```bash
pip install -r requirements.txt
```

## Repository Contents

The repository provides:

- `Module.py` — model implementation and supporting functions
- `Proposed_Model.ipynb` — training and evaluation notebook
- `Model_Analysis/Model_Complexity.ipynb` — parameter and inference-time analysis
- `Model_Analysis/` — computational complexity results
- `Results/` — quantitative evaluation, ablation, training, and run-to-run results
- Pretrained EMA generator checkpoint available through GitHub Releases

## Training and Evaluation

The main notebook contains the workflow for:

1. Creating/loading `Module.py`
2. Training GHM-CMNet
3. Evaluating the trained model

Dataset, checkpoint, and output paths should be set to the corresponding local directories before execution.

## Pretrained Model

The pretrained EMA generator checkpoint is available in GitHub Releases:

**[GHM-CMNet Pretrained Weights — v1.0.0](https://github.com/suadadnajim-code/GHM-CMNet-CT-Deblurring/releases/tag/v1.0.0)**

Checkpoint filename:

```text
best_ema_generator.pt
```

The checkpoint can be used for evaluation without retraining the model.

## Reproducibility

The repository includes the implementation and experimental materials supporting the reported GHM-CMNet results, including the training and evaluation pipeline, quantitative results, ablation experiments, run-to-run variability, and computational complexity analysis.

## Citation

Citation information will be added after publication.
