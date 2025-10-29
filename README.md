# Geneformer Tumor Classification

Fine-tuning single-cell foundation transformers (Geneformer or scGPT) to separate tumor from normal cells using colorectal cancer scRNA-seq data.

## GSE144735 quality control workflow

- Use the notebook `notebooks/01_quality_control.ipynb` to download the GEO supplements, assemble the AnnData object, and run my QC defaults (=200 genes per cell, genes detected in =3 cells).
- The notebook saves two artefacts ready for tokenisation: `gse144735_filtered_raw.h5ad` and the 5k highly variable gene slice `gse144735_hvg5k.h5ad`.

### Running it

1. Activate an environment with Scanpy + Geneformer prerequisites. One liner if starting from scratch:
   ```bash
   pip install -r requirements.txt
   ```
   This installs Scanpy, pandas, PyTorch, etc. Geneformer itself is not on PyPI right now; when you're ready to fine-tune it, follow the upstream repo's install instructions (clone and `pip install -e .`) in the same environment.

   Colab users can rely on the first notebook cell that installs these packages.
2. Execute the notebook top-to-bottom; downloads land in `gse144735/raw/` and filtered results in `gse144735/processed/`.
3. Review the QC summaries, then move on to token generation and model fine-tuning.

## Next steps

- Convert the filtered AnnData into Geneformer/scGPT token sequences.
- Define donor-level train/validation/test splits.
- Train the transformer head and capture metrics, visualisations, and biological readouts.
