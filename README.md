# Pre-Training Distribution Drives Cross-Lingual Entanglement: A Linear Concept Ablation Study of the English Bottleneck in Sub-4B LLMs

This repository contains the data and analysis pipeline to reproduce the findings presented in the paper: *Pre-Training Distribution Drives Cross-Lingual Entanglement: A Linear Concept Ablation Study of the English Bottleneck in Sub-4B LLMs*.

## Repository Structure

* `data/`: Contains the raw ablation metrics (200 `.csv` files) generated from the Kaggle environment. 
* `analysis_output/`: The destination directory for the generated JSON metric tables and Figure 1.
* `data_validation_notebook.ipynb`: Validates the structural integrity and schema of the raw CSV files.
* `json_generate.ipynb`: Extracts the core BPB and Probe Accuracy metrics for the ~3B parameter models (Tables I-III).
* `scale_analysis.ipynb`: Aggregates scaling data across parameter counts and generates the scaling trajectory figure (Figure 1).
* `pyproject.toml` / `uv.lock`: Dependency management configurations.

## Environment Setup

This project requires Python >= 3.12. 

Using `pip`:
```bash
pip install -r requirements.txt
```

Using `uv`:
```bash
uv sync
```

## Data Generation (Kaggle Integration)

For immediate reproducibility, the raw generative and structural ablation data (`data/` directory) is already included in this repository.

If you wish to re-run the heavy activation harvesting and LEACE matrix fitting pipeline from scratch, you can access the original generation code via Kaggle:

* **Kaggle Notebook:** https://www.kaggle.com/code/nnkurniawan/concept-erasure-ablation

**To reproduce the data generation:**

1. Execute the Kaggle notebook.
2. Download the resulting output archive.
3. Extract the `erasure_*.csv` and `validation_*.csv` files directly into the `data/` directory of this repository.

## Analysis Pipeline

To reproduce the tables and figures from the manuscript, execute the Jupyter notebooks in the following order:

1. **Verify Data Integrity:**
Run `data_validation_notebook.ipynb`. This ensures all 200 CSV files are present, structurally sound, and adhere to the expected metric boundaries.
2. **Generate Manuscript Tables:**
Run `json_generate.ipynb`. This notebook isolates the ~3B parameter models and computes the baseline metrics, generative impact (BPB Delta), and structural impact (Probe Accuracy Delta) tables. Outputs are saved to `analysis_output/`.
3. **Generate Scaling Trajectory Figure:**
Run `scale_analysis.ipynb`. This notebook processes the cross-scale models (14M to 4B parameters) and plots the generative impact of English erasure across capacities. The resulting figure is saved as `analysis_output/Fig1_Scaling_Trajectory.png`.
