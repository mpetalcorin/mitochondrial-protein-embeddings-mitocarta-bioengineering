# mitochondrial-protein-embeddings-mitocarta-bioengineering
A reproducible pipeline that integrates MitoCarta3.0 with protein language model embeddings, machine learning, and mutagenesis analysis to reveal tunable mitochondrial import signals and modular proteome design.

# Mitochondrial Protein Embeddings: Tunable Import Signals and Modular Design

This repository contains code, data, and analyses for the study  
**"Mitochondrial Protein Embeddings Reveal Tunable Import Signals and Modular Design Principles"**.  
The project leverages **protein language model (ESM2) embeddings** applied to the human mitochondrial proteome (MitoCarta3.0) to uncover modular design principles, functional hubs, and tunable N-terminal import sequences.

## Features
- Embedding of **1136 mitochondrial proteins** from MitoCarta3.0 using `facebook/esm2_t6_8M_UR50D`.
- Dimensionality reduction with PCA and UMAP for visualization of compartmental clustering.
- Network analysis of gene–pathway connectivity to identify functional hubs.
- Minimal-edit mutagenesis of N-terminal targeting sequences to explore tunability of import signals.
- Reproducible notebook (`MitoCarta Analysis-2.ipynb`) with all analysis steps.

## Key dependencies:
	•	Python 3.9+
	•	PyTorch
	•	HuggingFace transformers
	•	scikit-learn
	•	seaborn, matplotlib
	•	umap-learn
	•	networkx

# Datasheet for Dataset: MitoCarta3.0 Human Proteome

## Motivation
The dataset provides curated annotations of the human mitochondrial proteome for computational and bioengineering analyses.

## Composition
- **1136 proteins** annotated as mitochondrial in humans.
- Data includes:
  - FASTA amino acid sequences
  - Sub-mitochondrial compartment annotations
  - Pathway memberships
  - Import prediction scores

## Collection Process
- Proteins sourced from **MitoCarta3.0** (Rath et al., 2021).
- Integrates experimental proteomics, microscopy, and computational evidence.

## Recommended Uses
- Training and testing machine learning models for protein targeting.
- Analysis of sub-organelle compartmentalization.
- Synthetic biology design of mitochondrial proteins.

## Limitations
- Annotations may not capture all dynamic or context-dependent localizations.
- Some proteins are multifunctional and poorly characterized.

## Distribution
- Freely available from the Broad Institute MitoCarta resource.

## Maintenance
- Curated and updated periodically by the Mootha Lab, Broad Institute.

# Model Card: ESM2-based Embeddings for Mitochondrial Proteins

## Model Details
- **Model name**: facebook/esm2_t6_8M_UR50D
- **Developed by**: Meta AI
- **Type**: Transformer-based protein language model
- **Input**: Protein amino acid sequence
- **Output**: 320-dimensional embedding vector

## Intended Use
- Exploration of protein localization and function.
- Visualization of mitochondrial proteome organization.
- Testing mutagenesis and tunability of targeting sequences.

## Performance
- Captures biologically relevant clustering of proteins by compartment.
- Identifies modular hubs in gene–pathway networks.
- Detects shifts in import signals with minimal amino acid edits.

## Ethical Considerations
- Predictions are computational and **not substitutes for experimental validation**.
- Should not be directly used for clinical decision-making.

## Limitations
- Limited resolution for disordered or multifunctional proteins.
- Embeddings depend on training data bias from UniRef50.

## Citation
- **Petalcorin, M. I. R. (2025).** Mitochondrial Protein Embeddings Reveal Tunable Import Signals and Modular Design Principles. bioRxiv. https://doi.org/xxxx/xxxx
- Lin et al. (2023). *Science, 379(6637), 1123–1130.*
- Rives et al. (2021). *PNAS, 118(15), e2016239118.*
