# Agent Briefing

## Project Snapshot
- Goal: fine-tune a single-cell foundation transformer (Geneformer/scGPT) to classify tumor vs. normal cells using the colorectal cancer scRNA-seq dataset GSE144735, then extend to richer downstream analyses.
- Current assets:
  - `notebooks/01_quality_control.ipynb`: pulls the GEO supplements with `pooch`, constructs an AnnData object, applies baseline QC (cells =200 genes, genes in =3 cells), normalises/log-transforms, flags 5k HVGs, and saves `gse144735_filtered_raw.h5ad` + `gse144735_hvg5k.h5ad`.
  - `README.md`: quickstart instructions, explicit dependency install command, and high-level next steps.
  - `.gitignore`: hides `project.md`, `.venv/`, and cache directories.

## Environment Expectations
- Preferred local setup: dedicated env (`python -m venv .venv` or `conda create -n geneformer python=3.10`). Install with:
  ```bash
  pip install -r requirements.txt
  ```
  This covers Scanpy/pandas/PyTorch. Geneformer still needs to be installed from its upstream repo (clone and `pip install -e .`) once you're ready to fine-tune, since it isn't published on PyPI yet.
- Register env as Jupyter kernel (`python -m ipykernel install --user --name geneformer`) or select the `.venv` interpreter directly in the notebook UI.
- Colab remains a fallback: run the dependency cell, optionally switch to GPU runtime.

## Outstanding Work
1. Enhance QC thresholds (e.g., mitochondria percentage, upper bounds) if needed.
2. Build scripts/notebooks for tokenising AnnData into Geneformer/scGPT input format with donor-level splits.
3. Set up training pipeline (fine-tuning loop, metrics, logging, visualisation).
4. Produce final documentation: methods, results, biological interpretation.

## Reminders for Future Sessions
- Check that `pooch` and other deps install in the active kernel; re-run install + restart runtime if `ModuleNotFoundError` appears.
- Maintain personal-project tone in notebooks/docs (aligns with portfolio narrative).
- Keep `project.md` private (ignored by git).
- Before coding, verify repo state via `git status -sb`; avoid touching unrelated user changes.

## Summary Writing Conventions
- When the user asks for a summary of completed work (e.g., QC, tokenisation, training), produce a journal-style methods summary (Nature Methods/Bioinformatics tone).
- Use sectioned prose with concise headers: Dataset and Setting; Data Acquisition; AnnData Assembly; Quality Control; Normalization and Highly Variable Genes; Outputs and Sanity Checks.
- Prefer plain text over inline code formatting for method names and file paths; reserve code formatting for commands only when essential.
- Use US spelling (normalization, tumor) and neutral phrasing (e.g., "enabling donor-level cross-validation in subsequent analyses").
- Report key quantitative results (cell/gene counts after filters, HVG size, basic QC stats).
- Keep sentences concise; avoid semicolons and overly compound constructions.
