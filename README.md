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

#### **Predictive Modeling with ATAC-seq**
- **Goal**: Reconstruct denoised chromatin accessibility profiles (ATAC-seq peaks) from raw counts using convolutional encoders.
- **Relevance**:
  - Chromatin accessibility profiles are inherently structured by genomic loci, making convolutional neural networks (CNNs) a natural choice for feature extraction and pattern detection.
  - Convolutions can effectively model both local and long-range dependencies by using dilated convolutions or wide kernels.
- **Applications**:
  - **Feature Interpretation**: Identify patterns in chromatin accessibility associated with transcription factor binding or regulatory elements.
  - **Denoising**: Predict clean, normalized peak accessibility profiles for downstream analyses.
  - **Latent Representation**: Learn biologically meaningful embeddings of chromatin states for clustering or trajectory inference.

#### **Alignment Tasks**
- **Goal**: Align chromatin profiles across batches, donors, or experimental conditions.
- **Applications**:
  - Normalize chromatin accessibility data while preserving biological signal.
  - Compare regulatory landscapes across cell types or conditions.

#### **Latent Representation Learning**
- **Goal**: Extract shared embeddings for chromatin profiles that capture regulatory activity across genomic regions.
- **Applications**:
  - Integrate ATAC-seq data with RNA-seq or other modalities.
  - Infer transcription factor activity and gene regulatory networks.

---

### **4. Key Considerations**
- **Challenges**:
  - Chromatin accessibility data is highly sparse and high-dimensional, requiring robust encoders like CNNs to capture meaningful signals.
  - Long-range regulatory dependencies necessitate models capable of expanding receptive fields, such as dilated or multi-scale convolutions.
- **Approach**:
  - Use **convolutional encoders** with dilated convolutions to balance local feature extraction and global context.
  - Incorporate pooling or attention mechanisms to reduce dimensionality without losing important biological signals.

For further details, consult the accompanying documentation in this repository.
