# Mechanistic Characterization of the English Bottleneck and Cross-Lingual Entanglement in LLMs

This repository contains the data and analysis pipeline to reproduce the findings presented in the paper: *Pre-Training Distribution Drives Cross-Lingual Entanglement: A Linear Concept Ablation Study of the English Bottleneck in Sub-4B LLMs*.

<!-- ## Abstract
Large Language Models (LLMs) operate under strict parameter capacity constraints, which frequently manifest as the Curse of Multilinguality. To circumvent this limitation, models often develop an English bottleneck, routing non-English inputs through English-centric latent representations to conserve parameter space, complete with its biases. However, it remains unclear whether this cross-lingual entanglement is a limitation of the transformer architecture, or a byproduct of a skewed pre-training data distribution. This study characterizes the latent topologies of five sub-4B model families (Pythia, Llama 3.2, Qwen 2.5, BLOOM, and Gemma 3). Using LEAst-squares Concept Erasure (LEACE), English and regional Austronesian concepts are linearly ablated to measure the subsequent generative degradation (Bits-Per-Byte delta) and structural collapse (Probe Accuracy delta) of the Indonesian language representation. 

The experimental data indicates that while finite capacity drives the necessity for compression, dataset distribution strongly influences the geometric allocation strategy. Strictly monolingual models (Pythia) conserve capacity by treating zero-shot foreign tokens as unstructured out-of-distribution noise. Under English-dominant or unbalanced distributions (Llama 3.2, Qwen 2.5), models resolve parameter starvation by forcing Indonesian to act as a highly entangled, lossy projection of the English backbone, causing catastrophic generative collapse (up to 1.005 Bits-Per-Byte increase) upon ablation. Conversely, intentionally balanced architectures (BLOOM, Gemma 3) equitably distribute their limited parameter budget, constructing robust, orthogonal subspaces that mitigate entanglement. These findings suggest that downstream vulnerabilities, such as cross-lingual jailbreaking and Western-centric cultural bias, are deeply correlated with an English-dominant training distribution. Consequently, true alignment may require more equitable data balancing during pre-training to achieve geometric independence, rather than superficial post-training interventions. -->

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

* **Kaggle Notebook:** [The direct link to the notebook has been temporarily withheld to preserve double-blind peer review integrity. It will be restored upon publication]

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
