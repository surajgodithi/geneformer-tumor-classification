# Geneformer Tumor Classification

Fine-tuning single-cell foundation transformers (Geneformer or scGPT) to separate tumor from normal cells using colorectal cancer scRNA-seq data.

## GSE144735 quality control workflow

- Use the notebook `notebooks/01_quality_control.ipynb` to download the GEO supplements, assemble the AnnData object, and run QC defaults (>=200 genes per cell; genes detected in >=3 cells). HVGs are selected via Scanpy's cell_ranger flavour (no scikit-misc dependency).
- The notebook saves two artefacts ready for tokenisation: `gse144735/processed/gse144735_filtered_raw.h5ad` and the 5k highly variable gene slice `gse144735/processed/gse144735_hvg5k.h5ad`.

### Running it

1. Activate an environment with Scanpy + Geneformer prerequisites. One liner if starting from scratch:
   ```bash
   pip install -r requirements.txt
   ```
   This installs Scanpy, pandas, PyTorch, etc. Geneformer itself is not on PyPI right now; when you're ready to fine-tune it, follow the upstream repo's install instructions (clone and `pip install -e .`) in the same environment.

   Colab users: use a High-RAM runtime for this dataset. The notebook installs dependencies inside the kernel; Python 3.12 is supported (no scikit-misc required).
2. Execute the notebook top-to-bottom; downloads land in `gse144735/raw/` and filtered results in `gse144735/processed/`.
3. Review the QC summaries, then move on to token generation and model fine-tuning.

## Tokenisation

- Use `notebooks/02_tokenisation.ipynb` to convert the filtered AnnData into ranked gene tokens per cell (default top 2,048 genes), and write outputs under `gse144735/processed/tokens/`:
  - `gene_vocab.tsv` (gene symbol to token id mapping)
  - `gse144735_gene_rank_tokens.npz` (tokens and lengths)
  - `gse144735_tokens_metadata.tsv` (Patient/Class/Sample and token lengths)

## Splits

- Use `notebooks/03_splits.ipynb` to create donor-wise train/val/test indices (4/1/1 over six donors) from the token metadata.
- Outputs under `gse144735/processed/tokens/`:
  - `splits_by_patient.npz` (arrays: `train_idx`, `val_idx`, `test_idx`; lists of patients)
  - `splits_summary.tsv` (counts by split/patient/class for sanity checks)

## Next steps

- Define donor-level train/validation/test splits (patient-wise) using the token metadata.
- Train the transformer head and capture metrics, visualisations, and biological readouts.
