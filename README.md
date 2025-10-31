# Geneformer Tumor Classification

Fine-tuning single-cell foundation models (Geneformer or scGPT) to classify tumor vs. normal cells using colorectal cancer scRNA-seq data (GSE144735).

## Quick Start

### 1. Setup

Install dependencies:
```bash
pip install -r requirements.txt
```

For Colab users: Use a High-RAM runtime. The notebooks will handle dependency installation automatically.

### 2. Data Preparation

**Quality Control** (`notebooks/01_quality_control.ipynb`)
- Downloads GSE144735 data from GEO
- Filters cells and genes using standard QC thresholds
- Selects highly variable genes
- Outputs: `gse144735/processed/gse144735_filtered_raw.h5ad`

**Tokenization** (`notebooks/02_tokenisation.ipynb`)
- Converts gene expression into ranked token sequences
- Each cell becomes a sequence of top expressed genes
- Outputs: Token matrix and metadata in `gse144735/processed/tokens/`

**Train/Val/Test Splits** (`notebooks/03_splits.ipynb`)
- Creates patient-wise data splits (no cell leakage between splits)
- 4 patients for training, 1 for validation, 1 for test
- Outputs: Split indices in `gse144735/processed/tokens/`

### 3. Model Training

Coming soon - transformer fine-tuning pipeline

## Dataset

**GSE144735**: Single-cell RNA-seq from 6 colorectal cancer patients
- 27,414 cells after QC
- 3 tissue types: Normal, Border, Tumor
- 6 donors (KUL01, KUL19, KUL21, KUL28, KUL30, KUL31)

## Project Structure

```
gse144735/
├── raw/              # Downloaded GEO files
└── processed/
    ├── *.h5ad        # Filtered AnnData objects
    └── tokens/       # Tokenized data and splits
notebooks/            # Analysis workflows
```

## Notes

- The tokenization uses a vocabulary of 24,471 genes
- Each cell is represented by up to 2,048 top-expressed genes
- All splits are patient-wise to ensure generalization
