# Model Card, MitoCarta-ESM2 Embedding Analysis and Import-Tunability Scoring

## Model details
**Model name:** MitoCarta-ESM2 Embedding Analysis and Import-Tunability Scoring (analysis pipeline)  
**Version:** v1.0.0  
**Model type:** Representation learning, unsupervised clustering, and heuristic import-like scoring with in silico minimal-edit perturbation.  
**Primary components:**
1. **Protein language model embeddings:** `facebook/esm2_t6_8M_UR50D` produces **320-dimensional** embeddings per protein sequence.
2. **Feature augmentation:** Protein **length** and MitoCarta-derived **import score** appended to embeddings to form a **322-feature** vector per protein.
3. **Dimensionality reduction:** **IncrementalPCA (2D)** and **UMAP** for visualization.
4. **Clustering:** **MiniBatchKMeans (k=12)** for unsupervised partitioning of the proteome into modules.
5. **Biological mapping:** Cluster enrichment against **SubMitoLocalization** and **MitoPathway** annotations.
6. **Network analysis:** Bipartite **gene–pathway** graph, degree centrality to identify pathway hubs.
7. **Minimal-edit mutagenesis module:** Sliding N-terminal windows (18–21 aa) undergo minimal edits that alter **charge**, **hydropathy**, and **helix propensity**, then re-embedded and rescored to estimate **import-signal tunability**.

**What this “model” outputs:**
- 2D projections (PCA/UMAP) that organize proteins by embedding similarity.
- Cluster labels per protein (`cluster_k`) and their enrichment profiles.
- Ranked lists of proteins/windows predicted to **increase** or **decrease** import-like scores after minimal edits.
- Summary statistics (e.g., fraction of proteins with substantial score shifts, distribution of import-like scores).

## Intended use
### Primary intended use
- **Exploratory analysis** of mitochondrial proteome organization, to identify:
  - compartment-associated embedding structure,
  - pathway-associated modules,
  - hub pathways in gene–pathway networks,
  - candidate proteins whose N-termini appear **tunable** for mitochondrial targeting.

### Secondary intended use
- **Bioengineering design support** for mitochondrial targeting experiments:
  - shortlist N-terminal windows for reporter validation,
  - propose minimal sequence edits to tune targeting strength in silico before wet-lab testing.

### Out-of-scope use (not intended)
- Clinical decision-making, diagnosis, or therapeutic guidance.
- Definitive assignment of sub-mitochondrial localization for proteins lacking experimental annotation.
- Prediction of mitochondrial import efficiency as an absolute, quantitative biophysical measurement.
- Safety-critical applications where incorrect localization prediction would cause harm.

## Users
- Computational biologists, mitochondrial biologists, systems biologists.
- Bioengineers designing mitochondrial targeting peptides or mitochondrial protein delivery strategies.
- Students learning proteome-wide embedding analysis and annotation mapping.

## Data
### Training data for the embedding model
The ESM2 model is pre-trained by its authors on large-scale protein sequence corpora (general protein space). This repository **does not re-train ESM2**.

### Data used in this pipeline
- **MitoCarta3.0** human gene/protein list and annotations (sub-organelle localization and pathways).
- FASTA sequences associated with MitoCarta entries.
- Derived tables produced by the notebook (examples):
  - `MitoCarta3_processed_long.csv` (tidy long-format annotations),
  - `mitocarta_esm2_meta.csv` (headers + sequence metadata),
  - `table_import_scores.csv` (import-like scores per sequence/header),
  - `table_edit_proposals_all.csv` and top25 activate/deactivate subsets,
  - pathway enrichment and dual-localization enrichment tables.

### Data preprocessing
- Parsing/normalization of protein identifiers and symbols from headers.
- Expansion to long format for multi-assigned proteins (multiple compartments/pathways).
- Numeric coercion for length/import score, missing values imputed (typically zeros where needed for modeling/plotting).
- Standardization with `StandardScaler` prior to PCA/UMAP/KMeans.

## Evaluation and validation
### What was evaluated
- **Qualitative biological face validity:** Whether compartments/pathways show coherent structure in embedding space (PCA/UMAP) and cluster enrichment.
- **Internal consistency:** Robustness checks via alternate PCA settings (“variant PCA” figure).
- **Import-tunability sensitivity:** Distribution and magnitude of score shifts under minimal edits, including ranked candidates.

### What was not evaluated (limitations of evaluation)
- No direct experimental benchmarking of predicted import efficiency (e.g., in vitro import assays, reporter localization).
- No calibration of “import-like scores” to absolute import probability.
- No systematic comparison against dedicated mitochondrial targeting predictors as a ground-truth benchmark within this pipeline.

## Metrics
Because this is primarily an unsupervised analysis and design-suggestion pipeline, typical supervised metrics (accuracy, AUROC) are not central. Reported outputs include:
- Cluster composition and enrichment patterns (counts/fractions by compartment/pathway).
- Centrality measures in the gene–pathway graph (e.g., degree centrality rankings).
- Import-like score distributions, and **delta score** distributions after edits.
- Proportion of proteins exceeding a tunability threshold (e.g., top ~15% by |delta| in your analysis).

## Factors and performance considerations
Results can vary with:
- Sequence representation and header parsing (symbol mapping, isoform handling).
- Inclusion/exclusion of non-canonical sequences or mitochondrial-encoded proteins.
- Standardization choices, UMAP parameters, and the selected k for KMeans.
- Window length (18–21 aa) and the specific edit heuristics used (charge/hydropathy/helix changes).
- Underlying biases of large pLM embeddings (training-set biases and protein-family prevalence).

## Limitations
- **Embeddings are not ground truth:** Similarity in embedding space can reflect many factors (family, composition, motifs) and may not imply the same compartment in all cases.
- **Annotation noise and multiplicity:** MitoCarta assignments are curated but not error-free, and multi-localization reflects both biology and annotation complexity.
- **Import-like scoring is a proxy:** The mutagenesis “score” reflects modeled features and embedding changes, not measured TOM/TIM import kinetics.
- **Edge cases:** Proteins that use internal targeting sequences, non-canonical targeting, or signal-anchor mechanisms may not be captured by N-terminal window scanning.
- **Mitochondrial-encoded proteins:** Their targeting logic differs (not imported via TOM/TIM), and mixing them with nuclear-encoded proteins can complicate interpretations if not handled explicitly.

## Ethical considerations
- This work is intended for scientific and bioengineering research and should be used responsibly.
- Avoid overstating predictions as experimentally confirmed localization or pathogenicity.
- If integrating with human variant data, treat outputs as hypothesis-generating, not diagnostic.

## Recommendations for responsible use
- Validate top targeting edits experimentally (e.g., N-terminus–GFP reporter assays, microscopy, subcellular fractionation, in vitro import assays).
- Use multiple complementary predictors or evidence sources when making targeting claims.
- Document parameters (UMAP/KMeans/window rules) and keep analysis reproducible via pinned environments.

## Reproducibility
- Fix random seeds for UMAP/KMeans and record package versions.
- Save intermediate outputs (embeddings, cluster labels, tables) to enable exact regeneration of figures.
- Use CPU-compatible settings (IncrementalPCA, MiniBatchKMeans) for portability.

## Citation
If you use this repository, please cite:
- MitoCarta3.0 resource paper.
- ESM/ESM2 primary paper(s).
- UMAP and PCA references where appropriate.
- This repository (GitHub URL) and, if available, the chemRxiv preprint DOI.

## Contact
For questions or collaborations related to mitochondrial embedding analysis or targeting design workflows, open a GitHub issue in the repository.
