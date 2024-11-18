# moSC-Dataloader-for-Interdependent-ELBO-VAE

### **Dataset Documentation**
- **[Dataset Documentation](https://openproblems.bio/events/2021-09_neurips/documentation/data/dataset)**
- **[Download Dataset](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE194122)**
- **[Related Paper](https://openreview.net/forum?id=gN35BGa1Rt)**

---

## **README: Available Datasets and Functional DataLoaders**

---

### **Introduction**
This project involves processing and analyzing multi-modal single-cell datasets to enable advanced machine learning applications. Currently, we have datasets related to **RNA sequencing (GEX)**, **chromatin accessibility sequencing (ATAC-seq)**, and **CITE-seq** (simultaneous RNA and protein profiling). These datasets are designed to facilitate predictions, alignments, and representations of cellular states. Each dataset is integrated with custom-built DataLoaders to allow seamless interaction with PyTorch models.

---

### **1. Datasets Overview**

#### **1.1 Multiome Gene Expression (GEX) Data**
- **Data Source**: GEX data measures RNA transcripts from individual cells using the 10X Genomics Single-Cell Multiome ATAC + Gene Expression Kit.
- **Modality**: RNA sequencing (GEX).
- **File Format**: Dense matrices stored in H5AD files.
- **Key Features (Available in Metadata)**:
  - `n_genes_by_counts`: Number of genes detected per cell.
  - `pct_counts_mt`: Percentage of counts mapped to mitochondrial genes.
  - `n_counts`: Total number of UMI counts detected per cell.
  - `n_genes`: Total number of genes detected.
  - `size_factors`: Normalization factor for sequencing depth.
  - `phase`: Cell cycle phase (`G1`, `S`, `G2M`).
  - `cell_type`: Biological cell type annotation.
  - `pseudotime_order_GEX`: Developmental trajectory ordering.
  - `batch`: The batch and site identifiers (e.g., `s1d1` for Site 1, Donor 1).

#### **1.2 Multiome Chromatin Accessibility (ATAC) Data**
- **Data Source**: ATAC-seq data measures DNA accessibility using the same Multiome Kit.
- **Modality**: Chromatin accessibility sequencing (ATAC).
- **File Format**: Dense matrices stored in H5AD files.
- **Key Features (Available in Metadata)**:
  - `nCount_peaks`: Number of accessible DNA regions (peaks) detected.
  - `atac_fragments`: Total number of DNA fragments detected.
  - `reads_in_peaks_frac`: Fraction of fragments in accessible regions.
  - `blacklist_fraction`: Fraction of fragments in blacklisted genomic regions.
  - `nucleosome_signal`: Length distribution of DNA fragments (correlated with chromatin compaction).
  - `phase`: Cell cycle phase (`G1`, `S`, `G2M`).
  - `cell_type`: Biological cell type annotation.
  - `pseudotime_order_ATAC`: Developmental trajectory ordering.
  - `batch`: The batch and site identifiers (e.g., `s1d1` for Site 1, Donor 1).

#### **1.3 CITE-seq Data**
- **Data Source**: Joint RNA and protein profiling using 10X Genomics Single Cell Gene Expression and BioLegend TotalSeq™-B Universal Cocktail.
- **Modality**: RNA (GEX) and Protein (ADT) measurements.
- **File Format**: Dense matrices stored in H5AD files.
- **Key Features (Available in Metadata)**:
  - Gene expression values.
  - Protein marker abundance across 134 cell-surface proteins.
  - CLR-normalized protein counts for downstream analyses.
  - Batch effects captured via multi-site and multi-donor data collection.
  - Cell type annotations and pseudotime orders specific to protein and RNA modalities.

---

### **2. Functional DataLoaders**

#### **2.1 Multiome GEX DataLoader**
- **Utility**: Predict RNA expression profiles, analyze cell clustering, and infer transcriptomic trends.

#### **2.2 Multiome ATAC DataLoader**
- **Utility**: Predict chromatin accessibility and infer transcription factor binding profiles.

#### **2.3 CITE-seq DataLoader**
- **Utility**:
  - Joint training across RNA and protein modalities.
  - Supports predictive modeling tasks for proteomic markers from transcriptomic profiles and vice versa.
  - Enables downstream analyses such as cell-type annotation and latent embedding generation.

---

### **3. Task Design**

#### **Predictive Modeling**
- **Goal**: Predict one modality (e.g., RNA) from another (e.g., protein).
- **Applications**:
  - Model RNA-to-protein translation and transcriptional regulation.
  - Augment value in datasets with missing modalities.

#### **Alignment Tasks**
- **Goal**: Map cellular profiles across RNA and protein spaces.
- **Applications**:
  - Unify RNA and protein annotations for cellular states.
  - Link single-modality datasets using CITE-seq as ground truth.

#### **Latent Representation Learning**
- **Goal**: Integrate RNA, protein, and chromatin data into shared embeddings.
- **Applications**:
  - Study cellular differentiation and developmental trajectories.
  - Perform clustering and downstream classification tasks.

---

### **4. Key Considerations**
- **Challenges**:
  - High dimensionality and sparsity in multi-modal datasets require robust model design.
- **DataLoader Workflow**:
  - DataLoaders interact with dense matrices stored in `.h5ad` format.
  - Metadata is directly aligned with feature matrices to maintain consistency across modalities.

For further details, consult the accompanying documentation in this repository.
