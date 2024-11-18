# moSC-Dataloader-for-Interdependent-ELBO-VAE

### **Dataset Documentation**
- **[Dataset Documentation](https://openproblems.bio/events/2021-09_neurips/documentation/data/dataset)**
- **[Download Dataset](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE194122)**
- **[Related Paper](https://openreview.net/forum?id=gN35BGa1Rt)**

---

## **README: Available Datasets and Functional DataLoaders**

---

### **Introduction**
This project involves processing and analyzing multi-modal single-cell datasets to enable advanced machine learning applications. Currently, we have datasets related to **RNA sequencing (GEX)** and **chromatin accessibility sequencing (ATAC-seq)** derived from the **Multiome protocol**. These datasets are designed to facilitate predictions, alignments, and representations of cellular states. Each dataset is integrated with custom-built DataLoaders to allow seamless interaction with PyTorch models.

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

---

### **2. Functional DataLoaders**

#### **2.1 Multiome GEX DataLoader**
- **Initialization Output**:
  - Dense matrix for GEX found locally.
  - Batch sizes and shuffling supported.
- **Sample Output**:
  ```plaintext
  Batch 0:
  Data Shape: torch.Size([32, 129921])
  Metadata Sample:
  {
    'GEX_pct_counts_mt': tensor([...]),
    'GEX_n_counts': tensor([...]),
    'GEX_n_genes': tensor([...]),
    'cell_type': ['CD4+ T', 'B1 B', ...],
    ...
  }
  ```
- **Utility**:
  - Enables training of models to predict RNA expression profiles.
  - Useful for tasks such as cell clustering, developmental trajectory inference, and transcriptomic prediction.

#### **2.2 Multiome ATAC DataLoader**
- **Initialization Output**:
  - Dense matrix for ATAC found locally.
  - Batch sizes and shuffling supported.
- **Sample Output**:
  ```plaintext
  Batch 0:
  Data Shape: torch.Size([32, 129921])
  Metadata Sample:
  {
    'ATAC_nCount_peaks': tensor([...]),
    'reads_in_peaks_frac': tensor([...]),
    'nucleosome_signal': tensor([...]),
    'cell_type': ['NK', 'CD14+ Mono', ...],
    ...
  }
  ```
- **Utility**:
  - Enables training of models to predict chromatin accessibility profiles.
  - Useful for tasks such as peak accessibility prediction, transcription factor binding inference, and epigenetic state modeling.

---

### **3. Task Design**

#### **Predictive Modeling**
- **Goal**: Use one modality (e.g., GEX) to predict another modality (e.g., ATAC).
- **Applications**:
  - Uncover relationships between RNA expression and chromatin accessibility.
  - Infer chromatin state from transcriptomic data.

#### **Alignment Tasks**
- **Goal**: Match cells profiled in different modalities to create a unified representation.
- **Applications**:
  - Map RNA-seq and ATAC-seq data for shared cellular states.
  - Identify cross-modal relationships in biological conditions.

#### **Latent Representation Learning**
- **Goal**: Integrate GEX and ATAC into unified embeddings to study cellular states and transitions.
- **Applications**:
  - Reduce dimensionality of multi-modal data for visualization and clustering.
  - Learn shared features for downstream classification tasks.

---

### **4. Key Considerations**
- **DataLoader Workflow**:
  - DataLoaders pull from dense matrices (pre-stored as `.h5` files) for efficient loading.
  - Metadata is directly aligned with feature matrices to avoid indexing errors.
- **Preprocessing**:
  - GEX: Log-normalized expression values stored in `adata.X`.
  - ATAC: Binarized accessibility data stored in `adata.X`.

For further details, consult the accompanying preprocessing scripts and documentation in this repository.
