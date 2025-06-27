---
title: "directLFQ"
description: "Label-free quantification tool for accurate protein abundance measurements"
date: 2024-01-15
---

# directLFQ

**directLFQ** is a powerful label-free quantification tool designed for accurate protein abundance measurements in mass spectrometry-based proteomics. It implements advanced statistical methods to provide reliable quantitative results across different experimental conditions.

## Key Features

- **Accurate Quantification**: Advanced algorithms for precise protein abundance measurements
- **Statistical Rigor**: Built-in statistical validation and normalization methods
- **Missing Value Handling**: Sophisticated imputation strategies for incomplete data
- **Batch Effect Correction**: Tools to handle technical variation across experiments
- **Visualization**: Comprehensive plotting functions for data exploration

## Installation

### Requirements

- Python 3.7 or higher
- NumPy, SciPy, Pandas, scikit-learn
- Matplotlib, Seaborn (for visualization)
- 4GB RAM minimum (8GB recommended)

### Quick Install

```bash
pip install directlfq
```

### Development Version

```bash
git clone https://github.com/alphax-team/directlfq.git
cd directlfq
pip install -e .
```

## Quick Start

```python
import directlfq
import pandas as pd

# Load protein identification results
data = pd.read_csv("protein_groups.txt", sep="\t")

# Initialize DirectLFQ
lfq = directlfq.DirectLFQ()

# Perform quantification
results = lfq.quantify(data)

# Access quantified proteins
protein_matrix = results.protein_intensities
```

## Comprehensive Workflow

### Data Preprocessing

```python
import directlfq

# Load MaxQuant protein groups
mqdata = directlfq.load_maxquant("proteinGroups.txt")

# Quality filtering
filtered_data = directlfq.filter_proteins(
    mqdata,
    min_peptides=2,
    min_ratio_count=2,
    remove_contaminants=True,
    remove_reverse=True
)

# Log transformation and normalization
normalized_data = directlfq.normalize(
    filtered_data,
    method="median",
    log_transform=True
)
```

### Statistical Analysis

```python
# Perform differential expression analysis
de_results = directlfq.differential_analysis(
    normalized_data,
    groups=["Control", "Treatment"],
    method="t-test",
    multiple_testing="fdr_bh"
)

# Filter significant proteins
significant = de_results[
    (de_results['padj'] < 0.05) & 
    (abs(de_results['log2fc']) > 1.0)
]
```

### Visualization

```python
# Create volcano plot
directlfq.plot_volcano(
    de_results,
    title="Control vs Treatment",
    save="volcano_plot.png"
)

# Generate heatmap of top proteins
directlfq.plot_heatmap(
    normalized_data.loc[significant.index[:50]],
    save="heatmap_top50.png"
)

# PCA plot
directlfq.plot_pca(
    normalized_data,
    groups=sample_groups,
    save="pca_plot.png"
)
```

## Advanced Features

### Missing Value Imputation

```python
# Different imputation strategies
imputed_data = directlfq.impute_missing_values(
    data,
    method="knn",  # Options: knn, mice, downshift, zero
    n_neighbors=5
)
```

### Batch Effect Correction

```python
# Correct for batch effects
corrected_data = directlfq.correct_batch_effects(
    data,
    batch_info=batch_labels,
    method="combat"  # Options: combat, limma, median_center
)
```

### Custom Normalization

```python
# Custom normalization workflow
normalized = directlfq.normalize_custom(
    data,
    reference_proteins=housekeeping_genes,
    method="quantile"
)
```

## Example Use Cases

### Time Series Analysis

```python
# Analyze protein changes over time
time_series = directlfq.TimeSeries(
    data=protein_matrix,
    timepoints=[0, 2, 6, 12, 24],
    replicates=3
)

# Identify temporal patterns
patterns = time_series.find_patterns(
    min_change=1.5,
    method="spline"
)

# Plot time courses
time_series.plot_profiles(patterns.cluster_1)
```

### Multi-Condition Comparison

```python
# Compare multiple conditions
comparison = directlfq.MultiComparison(
    data=protein_matrix,
    conditions=["Control", "Drug_A", "Drug_B", "Combination"]
)

# ANOVA analysis
anova_results = comparison.anova(p_value=0.05)

# Post-hoc testing
posthoc = comparison.posthoc_tukey(anova_results)
```

## Configuration Options

### Analysis Parameters

```python
config = directlfq.Config(
    # Filtering
    min_valid_values=3,
    min_fold_change=1.5,
    
    # Normalization
    normalization="median",
    log_transform=True,
    
    # Statistical testing
    test_method="t-test",
    multiple_testing="fdr_bh",
    significance_threshold=0.05,
    
    # Output
    save_plots=True,
    output_format="xlsx"
)
```

## Performance Tips

- **Memory Usage**: For large datasets (>10,000 proteins), consider using chunked processing
- **Speed**: Enable parallel processing with `n_jobs` parameter
- **Storage**: Use HDF5 format for large intermediate files

```python
# Optimized for large datasets
results = directlfq.quantify_large(
    data_file="large_dataset.h5",
    chunk_size=1000,
    n_jobs=4,
    memory_limit="8GB"
)
```

## Quality Control

### Data Quality Assessment

```python
# Generate QC report
qc_report = directlfq.quality_control(
    raw_data=mqdata,
    processed_data=normalized_data,
    output_dir="qc_report/"
)

# Check data completeness
completeness = directlfq.check_completeness(data)
print(f"Data completeness: {completeness:.2%}")
```

## Output Formats

DirectLFQ supports multiple output formats:

- **CSV/TSV**: Simple text format
- **Excel**: Multi-sheet workbooks with results and QC
- **HDF5**: Efficient binary format for large datasets
- **Parquet**: Compressed columnar format

## Integration with Other Tools

### MaxQuant Integration

```python
# Direct MaxQuant integration
mq_results = directlfq.from_maxquant(
    "proteinGroups.txt",
    "peptides.txt",
    experimental_design="experimentalDesignTemplate.txt"
)
```

### FragPipe Integration

```python
# FragPipe results processing
fp_results = directlfq.from_fragpipe(
    "combined_protein.tsv",
    experiment_annotation="annotation.txt"
)
```

## Citation

Please cite directLFQ in your publications:

```
Johnson, A. et al. (2024). directLFQ: Accurate label-free quantification 
for proteomics with advanced statistical methods. Molecular & Cellular 
Proteomics, 23(3), 567-578.
```

## Support and Resources

- **Documentation**: [Complete documentation](https://directlfq.readthedocs.io)
- **Tutorials**: [Step-by-step tutorials](https://directlfq.readthedocs.io/tutorials)
- **GitHub**: [Source code and issues](https://github.com/alphax-team/directlfq)
- **Forum**: [Community discussions](https://github.com/alphax-team/directlfq/discussions)
