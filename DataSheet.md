# DataSheet, MitoCarta3.0 Embedding, Annotation, and Minimal-Edit Mutagenesis Outputs

## 1. Dataset overview
This repository includes curated and derived datasets supporting an in silico study that integrates **MitoCarta3.0** annotations with **ESM2 protein language model embeddings**, unsupervised machine learning, pathway network mapping, and **minimal-edit N-terminal mutagenesis** to explore mitochondrial proteome organization and tunable import signals.

These files are designed to be **reproducible research artifacts**, enabling re-creation of figures, re-analysis of embeddings, and extension of the mutagenesis design framework.

## 2. Motivation
Mitochondrial proteins are distributed across distinct compartments and pathways, yet the proteome is large and multifunctional. These datasets were created to:
- support systematic, proteome-wide visualization and clustering of mitochondrial proteins,
- map machine learning clusters to compartments and pathways,
- identify network hubs in mitochondrial pathways,
- quantify and rank predicted changes in mitochondrial targeting potential under minimal N-terminal edits.

## 3. Source data
### Primary source
- **MitoCarta3.0** (human), including annotations, pathways, and sequences.

### Derived sources in this repository
- ESM2 embeddings computed from MitoCarta FASTA sequences.
- Parsed and normalized annotation tables (tidy long format).
- Computed import-like scores and edit-effect summaries.

## 4. Dataset composition
The core unit of analysis is typically a **protein sequence record** identified by a FASTA header and/or gene symbol, with optional mapped compartment and pathway annotations. Depending on the file, the unit may be:
- a protein record (one row per protein),
- a protein-annotation record (multiple rows per protein when multiple compartments/pathways apply),
- a protein-window record (N-terminal sliding windows and edit proposals),
- an enrichment summary (cluster pathway or cluster compartment tables).

## 5. Files and one-sentence descriptions
### Cleaned and processed annotations
- **`MitoCarta3_original_clean.csv`**  
  Cleaned version of the original MitoCarta3.0 human dataset with standardized identifiers and annotation fields.

- **`MitoCarta3_processed_long.csv`**  
  Long-format annotation table that expands multi-assigned proteins into multiple rows to represent compartment and pathway memberships.

### Embedding metadata
- **`mitocarta_esm2_meta.csv`**  
  Metadata mapping protein headers to sequences (trimmed where applicable) and embedding records used in downstream analyses.

### Import scoring outputs
- **`table_import_scores.csv`**  
  Import-like scores per protein/header used to summarize targeting potential across the proteome and to quantify edit-induced shifts.

### Minimal-edit mutagenesis outputs
- **`table_edit_proposals_all.csv`**  
  Complete set of proposed minimal N-terminal edits with predicted effects on import-like score, including both activation and deactivation candidates.

- **`table_edit_proposals_activate_top25.csv`**  
  Top 25 proteins/windows predicted to gain import potential after minimal edits.

- **`table_edit_proposals_deactivate_top25.csv`**  
  Top 25 proteins/windows predicted to lose import potential after minimal edits.

### Enrichment and mapping outputs
- **`table_pathway_matrix_enrichment.csv`**  
  Pathway enrichment summaries across compartments and/or embedding-derived clusters.

- **`table_dual_loc_enrichment.csv`**  
  Summary table describing enrichment patterns among proteins annotated to multiple compartments.

## 6. Data collection process
1. Downloaded MitoCarta3.0 tables and FASTA sequences.
2. Parsed and normalized key identifiers (headers, symbols).
3. Embedded sequences using ESM2 to create a fixed-length representation per protein.
4. Built tidy annotation tables (including expansion of multi-assignments).
5. Computed import-like scores and performed minimal-edit perturbations on N-termini.
6. Generated enrichment summaries and ranked edit proposals.

## 7. Preprocessing and cleaning
Common transformations applied in the pipeline include:
- identifier normalization (header parsing, gene symbol extraction where possible),
- conversion to long format for multi-compartment/multi-pathway annotations,
- numeric coercion and imputation for length/import score fields,
- standardized scaling for PCA/UMAP/KMeans.

Any additional filters (e.g., removing very short sequences) should be explicitly documented in the notebook that produced the final files.

## 8. Labeling and annotations
Annotations include:
- **Sub-mitochondrial compartment** (e.g., outer membrane, inner membrane, matrix, intermembrane space),
- **MitoPathways** membership,
- derived labels including clusters (`cluster_k`) and edit-effect directionality (activate/deactivate).

Because some proteins have multiple valid annotations, multi-label behavior is represented by expansion in long-format tables.

## 9. Uses
### Intended uses
- Reproducing figures and tables from the accompanying notebook/manuscript.
- Exploratory analysis of mitochondrial proteome organization in embedding space.
- Hypothesis generation for mitochondrial targeting experiments and engineered targeting peptide designs.
- Extending mutagenesis scanning rules and ranking strategies.

### Out-of-scope uses
- Clinical diagnosis or treatment decisions.
- Definitive localization calls for proteins lacking experimental support.
- Absolute quantification of mitochondrial import efficiency.

## 10. Distribution and access
These datasets are distributed with the repository as CSV files for portability.  
Users should ensure they comply with the original MitoCarta3.0 terms of use when redistributing derived data.

## 11. Known limitations
- Import-like scores are computational proxies, not direct measurements of TOM/TIM import kinetics.
- Header parsing may not perfectly map all records to gene symbols across isoforms or RefSeq accessions.
- Dual localization can reflect biology, annotation complexity, or experimental ambiguity.
- Mutagenesis edits are minimal and heuristic, they prioritize interpretable physicochemical shifts, not exhaustive optimal designs.

## 12. Maintenance
- Regenerate datasets by re-running the notebooks in `notebooks/`.
- If upstream MitoCarta releases change identifiers or annotations, downstream parsing and mapping may require updates.
- Version datasets by tagging releases (e.g., v1.0.0) and recording package versions in the repository.

## 13. Citation
If you use these datasets, please cite:
- MitoCarta3.0 (human proteome and annotations).
- ESM/ESM2 primary publication(s).
- This repository and associated preprint (chemRxiv) if applicable.
