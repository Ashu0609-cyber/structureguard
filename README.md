# StructureGuard

**Uncertainty-Aware Super-Resolution for Microscopy Reliability Research**

---

## Project Vision

AI-based super-resolution can make low-resolution microscopy images appear sharper and more detailed. However, reconstructed details may not always accurately represent the underlying biological structures — enhancement can cause two distinct structures to appear merged, make one continuous structure appear split, or introduce other structural distortions.

StructureGuard investigates whether uncertainty estimates from AI super-resolution models correlate with actual reconstruction errors, and provides a pipeline to highlight regions that may require expert inspection or higher-resolution imaging.

> **Scientific principle:** Uncertainty does not automatically mean a region is incorrect.
> StructureGuard experimentally investigates the relationship between uncertainty and error.
> No results are assumed in advance.

---

## Research Objectives

1. **AI-based microscopy super-resolution** — develop and benchmark SR models on fluorescence microscopy data.
2. **Uncertainty estimation** — produce per-pixel uncertainty maps alongside SR outputs to flag potentially unreliable regions.
3. **Structural reconstruction reliability** — use paired low/high-resolution reference images to evaluate reconstruction fidelity.
4. **Quantitative validation** — measure whether uncertainty estimates correlate with actual reconstruction errors using image quality metrics and calibration analysis.

---

## Pipeline Overview

```
Raw LR Image
     │
     ▼
┌─────────────────┐
│  Preprocessing  │  Normalization, patch extraction, dataset splits
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Super-Resolution│  SR model: LR → SR prediction
│     Model       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   Uncertainty   │  Per-pixel uncertainty estimation
│   Estimation    │
└────────┬────────┘
         │
         ├──────────────────────────┐
         ▼                          ▼
┌─────────────────┐      ┌─────────────────────┐
│   SR Output     │      │  Uncertainty Map     │
│  (Enhanced img) │      │  (Reliability mask)  │
└────────┬────────┘      └──────────┬──────────┘
         │                          │
         └──────────┬───────────────┘
                    ▼
         ┌─────────────────┐
         │   Evaluation    │  PSNR, SSIM, calibration, error correlation
         └────────┬────────┘
                  │
                  ▼
         ┌─────────────────┐
         │  Visualization  │  Heatmaps, overlays, comparison plots
         └────────┬────────┘
                  │
                  ▼
         ┌─────────────────┐
         │  Streamlit App  │  Interactive inspection interface
         └─────────────────┘
```

---

## Repository Structure

```
StructureGuard/
│
├── data/
│   ├── raw/            # Original downloaded datasets — never modified
│   ├── processed/      # Normalized, tiled, format-converted images
│   └── splits/         # Train/val/test index files (JSON/CSV) for reproducibility
│
├── src/                # Importable Python package — core pipeline logic
│   ├── preprocessing/  # Image loading, normalization, patching, augmentation
│   ├── datasets/       # Dataset loading, validation, dataset-specific utilities
│   ├── models/         # Abstract base classes and shared model utilities
│   ├── super_resolution/  # SR model architecture definitions
│   ├── uncertainty/    # Uncertainty estimation methods (research candidates)
│   ├── evaluation/     # Image quality and uncertainty calibration metrics
│   └── visualization/  # Heatmaps, comparison plots, Streamlit-compatible figures
│
├── experiments/
│   ├── baseline/       # Conventional upscaling benchmarks (bicubic, Lanczos, etc.)
│   ├── training/       # Training and experiment execution scripts (Colab-compatible)
│   └── evaluation/     # Evaluation scripts; results written to results/
│
├── app/                # Streamlit interactive interface
├── tests/              # Unit and integration tests (run with: pytest tests/)
├── checkpoints/        # Saved model weights — gitignored
├── results/            # Generated SR images, uncertainty maps, plots — gitignored
│
├── config.yaml         # Reproducible experiment configuration
├── requirements.txt    # Essential project dependencies
└── README.md           # This file
```

---

## Technology Stack

| Category | Tools |
|---|---|
| Language | Python 3.11 |
| Deep Learning | PyTorch, Torchvision |
| Image Processing | OpenCV, scikit-image, NumPy |
| Visualization | Matplotlib, Plotly |
| Interface | Streamlit |
| Version Control | Git / GitHub |
| GPU Training | Google Colab |
| IDE | Google Antigravity |
| OS | Windows 11 |

---

## Development Workflow

### Local Development

```powershell
# Activate the virtual environment
c:\Projects\StructureGuard\struc\Scripts\Activate.ps1

# Install dependencies (after approval)
pip install -r requirements.txt

# Run the Streamlit app
streamlit run app/main.py

# Run tests
pytest tests/
```

### Google Colab (GPU Training)

GPU-intensive training is designed to run on Google Colab. Training scripts in
`experiments/training/` are written to be Colab-compatible. Model checkpoints are
saved to `checkpoints/` and can be synced via Google Drive or direct download.

### Experiment Reproducibility

All experiments are controlled by `config.yaml`. Set the random seed, image
dimensions, dataset paths, and model hyperparameters there before running any
experiment. Never hard-code values in scripts.

---

## Data

The project targets fluorescence microscopy datasets with paired low-resolution and
high-resolution images (e.g. BioSR). Data files are gitignored and must be
downloaded separately. See `data/raw/` for placement instructions once the data
acquisition step is approved.

---

## Evaluation Principles

- Uncertainty maps are evaluated against actual reconstruction error — not assumed
  to be correct.
- Image quality is measured with established metrics (PSNR, SSIM) applied to SR
  outputs against high-resolution ground truth.
- Calibration analysis tests whether high-uncertainty predictions actually correspond
  to high reconstruction error regions.
- All results are generated by scripts in `experiments/evaluation/` and stored in
  `results/`. No results are hard-coded.

---

## Project Status

| Phase | Status |
|---|---|
| Project initialization | ✅ Complete |
| Environment setup | ✅ Python 3.11.9 venv (`struc`) |
| Architecture design | ✅ Approved |
| Data acquisition | ⏳ Pending |
| Preprocessing pipeline | ⏳ Pending |
| Baseline experiments | ⏳ Pending |
| SR model implementation | ⏳ Pending |
| Uncertainty estimation | ⏳ Pending |
| Evaluation & calibration | ⏳ Pending |
| Streamlit interface | ⏳ Pending |

---

## Scientific Integrity

StructureGuard is a research project. All findings must be reproducible, all metrics
must be computed from real data, and no experimental outcomes are assumed in advance.
Uncertainty estimation methods are treated as research candidates subject to
experimental validation — not guaranteed solutions.
