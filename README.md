# oAd-mCD40L study analysis code

Analysis notebooks accompanying the study:

> **Oncolytic membrane-bound CD40L rewires macrophages to boost CD8+ T cell responses and antitumor immunity**

The repository covers single-cell RNA-seq, single-cell TCR-seq, cell-state trajectories, pathway analysis, cell-population enrichment and cell-cell communication in the ovarian-cancer tumor microenvironment.

## Repository contents

| Notebook | Main task | Language |
| --- | --- | --- |
| [`pp_annotation_linear_original_cells.ipynb`](pp_annotation_linear_original_cells.ipynb) | Quality control, scVI integration, clustering, annotation, differential expression and export of major cell populations | Python |
| [`OR.ipynb`](OR.ipynb) | Fisher's exact tests and odds-ratio heatmaps for cell-population enrichment/depletion | Python |
| [`monocle.ipynb`](monocle.ipynb) | Monocle 3 trajectory and pseudotime analysis of the myeloid compartment | R |
| [`Genetraj.ipynb`](Genetraj.ipynb) | GeneTrajectory gene-program and ordered-trajectory analysis | R/Python |
| [`GSEA.ipynb`](GSEA.ipynb) | GSVA/GSEA pathway analysis | R |
| [`cellChat_pub.ipynb`](cellChat_pub.ipynb) | Per-sample CellChat inference and exploratory 2 x 2 treatment comparisons | R |
| [`scRepertoire.ipynb`](scRepertoire.ipynb) | TCR clonality, overlap, diversity and transcriptome integration | R |

Notebook outputs are intentionally cleared in the public repository. Figures and tables are regenerated from the study data.

## Recommended workflow

1. Process raw sequencing reads with the upstream CeleScope workflow (not included).
2. Run `pp_annotation_linear_original_cells.ipynb` to generate the integrated object and annotated population-level objects.
3. Run `OR.ipynb` for population-composition comparisons.
4. Convert/export the annotated myeloid object to the Seurat RDS expected by `monocle.ipynb`, then run Monocle 3.
5. Run `Genetraj.ipynb` and `GSEA.ipynb` for gene-program and pathway analyses.
6. Export the count matrix and cell metadata described below, then run `cellChat_pub.ipynb`.
7. Run `scRepertoire.ipynb` for the TCR analysis.

The downstream notebooks are independent after the annotated objects and metadata tables have been generated.

## Configuration and expected inputs

Large data files are not stored in GitHub. Paths are configured near the top of each notebook.

### Main annotation notebook

`pp_annotation_linear_original_cells.ipynb` reads a pooled AnnData file. The default lookup order is:

1. the path in `OAD_POOL_H5AD`;
2. `OAD_DATA_DIR/pool_raw.h5ad`;
3. `./pool_raw.h5ad`.

If `OAD_DATA_DIR` is not set, it defaults to `transcriptome/`. Example on Linux/macOS:

```bash
export OAD_DATA_DIR=/path/to/study/transcriptome
export OAD_POOL_H5AD=/path/to/study/pool_raw.h5ad
```

The AnnData object must contain the metadata fields used by the notebook, including `Sample_id`, `treatment`, `n_genes_by_counts`, `pct_counts_mt`, `total_counts` and `pct_counts_Rp`.

### Monocle 3 notebook

By default, `monocle.ipynb` expects:

```text
transcriptome/pool_Myeloid_DEG/pool_Myeloid_anno_raw.rds
```

The Seurat object must contain `Sample_id`, `treatment` and `anno_sub`. Edit `project_ID`, `mtx`, `n_neighbors`, `min_dist` and `early_genes` in the configuration cell when needed. The notebook now computes PCA before UMAP, learns the principal graph before ordering cells and saves the final `cell_data_set` as `monocle3_cds.rds`.

### CellChat notebook

By default, `cellChat_pub.ipynb` expects:

```text
transcriptome/pool_DEG/pool_raw.csv
transcriptome/pool_DEG/meta_treatment_anno.csv
```

Count-matrix columns and metadata row names must contain the same cell barcodes. Metadata must include `anno_sub` and `Treatment_Sample`.

The notebook uses `CellChatDB.mouse` by default. To use four CellPhoneDB-derived tables, set `use_custom_cellphonedb <- TRUE` and define `CELLPHONEDB_DIR`. The required files are:

```text
interaction_input_CellChatDB.csv
complex_input_CellChatDB.csv
cofactor_input_CellChatDB.csv
geneInfo_input_CellChatDB.csv
```

The macrophage label is standardized to `TAM.M2` across the three updated notebooks. The 2 x 2 CellChat comparison has only two samples per group; its consistency scores, effect sizes and p-values are exploratory. Pairwise contrasts share samples and must not be interpreted as independent biological replicates or as confirmatory inference.

## Software

The notebooks use both Python and R.

Main Python dependencies include `scanpy`, `scvi-tools`, `scvelo`, `scrublet`, `anndata`, `pandas`, `numpy`, `scipy`, `scikit-learn`, `matplotlib` and `seaborn`.

Main R dependencies include `Seurat`, `SeuratWrappers`, `monocle3`, `CellChat`, `scRepertoire`, `GeneTrajectory`, `GSVA`, `GSEABase`, `clusterProfiler`, `msigdbr`, `ComplexHeatmap`, `circlize`, `igraph`, `patchwork` and `ggplot2`.

Exact versions are not yet fully locked. Before archival release, add machine-readable environment files such as `environment.yml`/`requirements.txt` and `renv.lock`, then record `sessionInfo()` and `sc.logging.print_versions()` from the final analysis environments.

## Data availability and privacy

Do not upload controlled-access human data, identifiable sample information, FASTQ/BAM files or large processed objects to GitHub. Deposit raw and processed sequencing data in an appropriate scientific repository and replace the placeholder below with the public accession and access conditions.

> **Data accession:** TODO — add repository name, accession identifier and release date.

## Reproducibility checklist before publication

- [ ] Add the raw/processed data accession and access instructions.
- [ ] Add the final manuscript citation and DOI.
- [ ] Add Python and R environment lock files.
- [ ] Confirm sample IDs, treatment labels and cell-type labels against the final metadata.
- [ ] Run all notebooks from a clean environment and archive session/version information.
- [ ] Add an explicit open-source license (for example, MIT, BSD-3-Clause or GPL-3.0) after the authors select the intended reuse terms.
- [ ] Replace the generic contact statement with a maintained email address or issue-tracker policy.

## Citation

Xiong S, Si Y, Zhang X, et al. *Oncolytic membrane-bound CD40L rewires macrophages to boost CD8+ T cell responses and antitumor immunity*. Full journal citation and DOI to be added when available.

## License and contact

No software license has been selected yet; reuse terms are therefore not granted by this repository alone. For questions about the analysis, contact the corresponding authors listed in the manuscript or open a GitHub issue.
