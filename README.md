# oAd-mCD40L study analysis code

This repository contains the analysis notebooks used for the single-cell RNA-seq and single-cell TCR-seq analyses in the manuscript:

> **Oncolytic membrane-bound CD40L rewires macrophages to boost CD8+ T cell responses and antitumor immunity**

The study investigates how an oncolytic adenovirus expressing membrane-bound CD40L (oAd-mCD40L) remodels the ovarian-cancer tumor microenvironment, with emphasis on macrophage reprogramming, CD8+ T-cell responses, cell-cell communication, gene trajectories, pathway activity, and TCR clonality.

## Repository contents

| Notebook | Purpose |
| --- | --- |
| `pp_annotation_linear_original_cells.ipynb` | Main Python workflow for quality control, scVI-based integration, dimensionality reduction, clustering, cell-type annotation, marker analysis, differential expression, and export of major cell populations. |
| `OR.ipynb` | Calculates treatment-associated enrichment or depletion of annotated cell populations using Fisher's exact test and visualizes odds ratios as a heatmap. |
| `monocle.ipynb` | Performs Monocle/Monocle 3 trajectory and pseudotime analysis, primarily for the myeloid compartment. |
| `Genetraj.ipynb` | Uses GeneTrajectory to infer gene-gene distances, gene programs, and ordered gene trajectories. |
| `GSEA.ipynb` | Runs GSVA and GSEA-based pathway analysis using GO, KEGG, and related gene-set resources. |
| `cellChat_pub.ipynb` | Reconstructs and compares cell-cell communication networks with CellChat and ligand-receptor information derived from CellPhoneDB. |
| `scRepertoire.ipynb` | Integrates single-cell TCR data with annotated transcriptomes and analyzes clonal expansion, overlap, diversity, and treatment/site-specific repertoire patterns. |

## Suggested analysis order

1. Process raw sequencing reads with the upstream CeleScope workflow. The upstream read-processing pipeline is not included here.
2. Run `pp_annotation_linear_original_cells.ipynb` to create the integrated and annotated single-cell objects.
3. Run `OR.ipynb` to compare cell-type distributions among treatment groups.
4. Run `monocle.ipynb` and `Genetraj.ipynb` for cell-state and gene-program trajectory analyses.
5. Run `GSEA.ipynb` for pathway-level interpretation.
6. Run `cellChat_pub.ipynb` for cell-cell communication analysis.
7. Run `scRepertoire.ipynb` for TCR repertoire analysis.

The exact order of downstream notebooks can be changed after the annotated objects and metadata tables have been generated.

## Software

The notebooks use both Python and R.

### Python

Major packages include:

- `scanpy`
- `scvi-tools`
- `scvelo`
- `scrublet`
- `anndata`
- `pandas`
- `numpy`
- `scipy`
- `scikit-learn`
- `matplotlib`
- `seaborn`

The manuscript reports Scanpy 1.9.1 and scVI 1.1.6 for the principal single-cell workflow.

### R

Major packages include:

- `Seurat` and `SeuratWrappers`
- `CellChat`
- `scRepertoire`
- `monocle` and `monocle3`
- `GeneTrajectory`
- `GSVA`, `GSEABase`, `clusterProfiler`, and `msigdbr`
- `ComplexHeatmap`, `pheatmap`, `ggplot2`, and related visualization packages
- `sscVis`
- `reticulate` and `sceasy`

The manuscript reports CopyKAT 1.1.0, Hotspot 0.9.0, and GeneTrajectory 1.0.0 for associated analyses. Package versions should be recorded in a lock file or environment export before the final archival release.

## Input data

Sequencing data and large processed objects are not included in this repository. The notebooks expect files such as:

- annotated or raw `.h5ad` objects;
- Seurat `.rds` objects;
- count matrices and cell metadata tables;
- V(D)J contig-annotation files;
- CellPhoneDB-derived ligand-receptor tables.

Before publication, add the public accession or archive link for the raw and processed data here:

> **Data accession:** to be added by the authors.

Do not upload controlled-access human data, identifiable sample information, raw FASTQ/BAM files, or other large data directly to GitHub. Deposit sequencing data in an appropriate scientific repository and link the accession from this README.

## Running the notebooks

1. Clone or download this repository.
2. Create Python and R environments containing the packages listed above.
3. Download the study data from the accession that will be added to this README.
4. Recreate the expected local directory structure or update each notebook's configuration cells.
5. Start JupyterLab and run the notebooks in the suggested order.

Several notebooks currently assume project folders such as `transcriptome/`, `VDJ/`, or `TCR/`. Edit the `mtx`, `outputdir`, `project_ID`, and related variables near the beginning of each notebook to match your system.

## Portability and reproducibility notes

- `Genetraj.ipynb` contains an author-specific Python interpreter path under `/home/xili/`; replace it with the path to your own environment.
- `GSEA.ipynb` contains an author-specific working path under `/home/xili/Documents/GSEA`; replace it before running.
- `cellChat_pub.ipynb` contains an author-specific CellPhoneDB directory under `/media/xili/`; replace it with your local database path.
- In the CellChat database-loading cell, verify that the interaction, complex, cofactor, and gene-information objects each point to the intended input table before running the analysis.
- Some installation commands are embedded in notebooks. For a stable release, install dependencies in a dedicated environment rather than modifying the environment during analysis.
- Random seeds, exact package versions, and a machine-readable environment specification should be added before the final journal or archive release.

## Reproducibility status

The notebooks document the analysis logic but are not yet a fully self-contained reproduction package because the input data, exact environment lock files, and public data accession are not included. A release tag should be created for the version associated with the submitted or accepted manuscript.

## Citation

If you use this code, please cite the associated manuscript:

> Xiong S, Si Y, Zhang X, et al. *Oncolytic membrane-bound CD40L rewires macrophages to boost CD8+ T cell responses and antitumor immunity*. Manuscript.

Replace this entry with the final journal citation and DOI after publication.

## License

No reuse license has been selected yet. The authors should add an appropriate `LICENSE` file before public release. Until then, the absence of a license means that reuse rights are not granted by this repository.

## Contact

Please direct questions to the corresponding author listed in the manuscript. The authors may replace this sentence with a preferred public contact address before release.
