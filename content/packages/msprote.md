---
title: "msProteo"
description: "Comprehensive mass spectrometry proteomics data processing pipeline"
date: 2024-01-15
---

# msProteo

**msProteo** is a comprehensive mass spectrometry proteomics data processing pipeline that integrates multiple analysis steps into a unified workflow. It provides researchers with a complete solution for processing raw MS data through to biological interpretation.

## Key Features

- **End-to-End Pipeline**: Complete workflow from raw data to biological insights
- **Multiple Search Engines**: Integration with popular search engines (Comet, X!Tandem, MSGF+)
- **Quality Control**: Comprehensive QC metrics and visualization
- **Statistical Analysis**: Built-in statistical methods for differential analysis
- **Pathway Analysis**: Integration with pathway databases and enrichment tools
- **Reproducible Workflows**: Version-controlled, reproducible analysis pipelines

## Installation

### System Requirements

- Python 3.8 or higher
- R 4.0 or higher (for statistical analysis)
- 16GB RAM minimum (32GB recommended for large datasets)
- 50GB free disk space

### Quick Install

```bash
# Install from PyPI
pip install msprote

# Install with all dependencies
pip install msprote[full]
```

### Docker Installation

```bash
# Pull Docker image
docker pull alphax/msprote:latest

# Run container
docker run -v /path/to/data:/data alphax/msprote:latest
```

## Quick Start

### Basic Pipeline

```python
import msprote

# Initialize pipeline
pipeline = msprote.Pipeline()

# Configure analysis
config = msprote.Config(
    raw_data_dir="/path/to/raw/",
    output_dir="/path/to/output/",
    database="/path/to/database.fasta",
    experimental_design="design.txt"
)

# Run complete analysis
results = pipeline.run(config)
```

### Command Line Interface

```bash
# Run complete pipeline
msprote analyze \
    --raw-data /path/to/raw/ \
    --database database.fasta \
    --design experimental_design.txt \
    --output results/

# Run specific steps
msprote search --raw-data /path/to/raw/ --database database.fasta
msprote quantify --search-results search_results/
msprote stats --quant-results quant_results/ --design design.txt
```

## Workflow Components

### 1. Data Preprocessing

```python
# Raw data conversion and filtering
preprocessor = msprote.Preprocessor()

# Convert vendor formats to mzML
preprocessor.convert_raw_data(
    input_dir="raw_data/",
    output_dir="mzml_data/",
    format="mzML"
)

# Filter and validate data
preprocessor.filter_data(
    min_scan_count=1000,
    min_ms2_count=500
)
```

### 2. Database Search

```python
# Multi-engine search
search_engine = msprote.SearchEngine()

# Configure search parameters
search_params = msprote.SearchParams(
    enzyme="trypsin",
    missed_cleavages=2,
    precursor_tolerance="10 ppm",
    fragment_tolerance="0.02 Da",
    modifications={
        "fixed": ["Carbamidomethyl (C)"],
        "variable": ["Oxidation (M)", "Acetyl (Protein N-term)"]
    }
)

# Run search with multiple engines
search_results = search_engine.multi_search(
    data_files=mzml_files,
    database="database.fasta",
    engines=["comet", "xtandem", "msgf"],
    params=search_params
)
```

### 3. Result Integration and Validation

```python
# Combine search results
integrator = msprote.ResultIntegrator()

combined_results = integrator.combine_engines(
    search_results,
    method="percolator"  # Options: percolator, peptideprophet, fdr
)

# Apply FDR filtering
validated_results = integrator.apply_fdr(
    combined_results,
    protein_fdr=0.01,
    peptide_fdr=0.01
)
```

### 4. Quantification

```python
# Label-free quantification
quantifier = msprote.Quantifier()

# Extract ion chromatograms
quant_results = quantifier.extract_features(
    mzml_files=mzml_files,
    identifications=validated_results,
    method="area_under_curve"
)

# Normalize and impute
normalized_data = quantifier.normalize(
    quant_results,
    method="median_normalization"
)

imputed_data = quantifier.impute_missing(
    normalized_data,
    method="knn"
)
```

### 5. Statistical Analysis

```python
# Differential expression analysis
stats_analyzer = msprote.StatisticalAnalyzer()

# Load experimental design
design = msprote.load_design("experimental_design.txt")

# Perform statistical testing
de_results = stats_analyzer.differential_analysis(
    data=imputed_data,
    design=design,
    contrast="Treatment_vs_Control",
    method="limma"
)

# Multiple testing correction
corrected_results = stats_analyzer.multiple_testing_correction(
    de_results,
    method="fdr_bh"
)
```

## Advanced Features

### Custom Workflows

```python
# Define custom workflow
class CustomWorkflow(msprote.BaseWorkflow):
    def __init__(self):
        super().__init__()
        
    def custom_preprocessing(self, data):
        # Custom preprocessing steps
        return processed_data
        
    def custom_analysis(self, data):
        # Custom analysis methods
        return results

# Use custom workflow
workflow = CustomWorkflow()
results = workflow.run(data)
```

### Batch Processing

```python
# Process multiple experiments
batch_processor = msprote.BatchProcessor()

experiments = [
    {"name": "exp1", "data": "exp1_data/", "design": "exp1_design.txt"},
    {"name": "exp2", "data": "exp2_data/", "design": "exp2_design.txt"},
    {"name": "exp3", "data": "exp3_data/", "design": "exp3_design.txt"}
]

batch_results = batch_processor.process_experiments(
    experiments,
    output_dir="batch_results/",
    parallel=True,
    n_jobs=4
)
```

### Quality Control Dashboard

```python
# Generate comprehensive QC report
qc_generator = msprote.QCGenerator()

qc_report = qc_generator.generate_report(
    raw_data=raw_files,
    search_results=search_results,
    quant_data=quant_results,
    output="qc_report.html"
)

# Real-time QC monitoring
qc_monitor = msprote.QCMonitor()
qc_monitor.start_monitoring(
    data_dir="incoming_data/",
    update_interval=60  # seconds
)
```

## Configuration Files

### Pipeline Configuration

```yaml
# msprote_config.yaml
pipeline:
  name: "my_proteomics_analysis"
  version: "1.0"
  
preprocessing:
  convert_raw: true
  filter_scans: true
  min_scan_count: 1000
  
search:
  engines: ["comet", "xtandem"]
  database: "uniprot_human.fasta"
  enzyme: "trypsin"
  missed_cleavages: 2
  precursor_tolerance: "10 ppm"
  fragment_tolerance: "0.02 Da"
  
quantification:
  method: "area_under_curve"
  normalization: "median"
  imputation: "knn"
  
statistics:
  test_method: "limma"
  p_value_adjustment: "fdr_bh"
  significance_threshold: 0.05
```

### Experimental Design

```
Sample	Condition	Replicate	Batch
S001	Control	1	1
S002	Control	2	1
S003	Control	3	1
S004	Treatment	1	1
S005	Treatment	2	1
S006	Treatment	3	1
```

## Output and Reporting

### Results Structure

```
results/
├── preprocessing/
│   ├── converted_data/
│   └── qc_metrics.json
├── search/
│   ├── comet_results/
│   ├── xtandem_results/
│   └── combined_results.tsv
├── quantification/
│   ├── protein_intensities.csv
│   ├── peptide_intensities.csv
│   └── missing_value_report.html
├── statistics/
│   ├── differential_expression.csv
│   ├── volcano_plots/
│   └── pathway_analysis/
└── reports/
    ├── summary_report.html
    └── methods_report.md
```

### Interactive Reports

```python
# Generate interactive HTML report
reporter = msprote.Reporter()

interactive_report = reporter.create_interactive_report(
    results=pipeline_results,
    template="comprehensive",
    include_plots=True,
    output="interactive_report.html"
)
```

## Integration with External Tools

### R Integration

```python
# Use R packages for advanced statistics
r_interface = msprote.RInterface()

# Run DESeq2 analysis
deseq_results = r_interface.run_deseq2(
    count_data=protein_data,
    design_formula="~ condition"
)

# Pathway analysis with clusterProfiler
pathway_results = r_interface.run_clusterprofiler(
    gene_list=significant_proteins,
    organism="human"
)
```

### Database Integration

```python
# Connect to proteomics databases
db_connector = msprote.DatabaseConnector()

# Query UniProt
uniprot_info = db_connector.query_uniprot(
    protein_ids=protein_list,
    fields=["gene_name", "function", "pathway"]
)

# Retrieve pathway information
pathway_info = db_connector.query_kegg(
    gene_list=gene_list
)
```

## Performance Optimization

### Memory Management

```python
# Configure memory usage
msprote.set_memory_limit("16GB")

# Use chunked processing for large datasets
chunked_processor = msprote.ChunkedProcessor(
    chunk_size=1000,
    overlap=100
)
```

### Parallel Processing

```python
# Configure parallel execution
msprote.set_parallel_config(
    n_jobs=8,
    backend="multiprocessing"
)

# GPU acceleration (if available)
msprote.enable_gpu_acceleration()
```

## Troubleshooting

### Common Issues

1. **Memory errors**: Reduce chunk size or increase available memory
2. **Search engine failures**: Check database format and search parameters
3. **Missing dependencies**: Run `msprote check-dependencies`

### Debug Mode

```python
# Enable debug logging
msprote.set_log_level("DEBUG")

# Run with verbose output
pipeline.run(config, verbose=True)
```

## Citation

If you use msProteo in your research, please cite:

```
Williams, M. et al. (2024). msProteo: A comprehensive pipeline for 
mass spectrometry-based proteomics data analysis. Bioinformatics, 
40(8), 1123-1135.
```

## Support

- **Documentation**: [Full documentation](https://msprote.readthedocs.io)
- **Tutorials**: [Video tutorials](https://msprote.readthedocs.io/tutorials)
- **Support Forum**: [Community support](https://github.com/alphax-team/msprote/discussions)
- **Bug Reports**: [GitHub Issues](https://github.com/alphax-team/msprote/issues)
