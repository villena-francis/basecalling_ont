# Oxford Nanopore Basecalling Pipeline

## Overview

This Snakemake workflow processes Oxford Nanopore Technologies (ONT) raw reads (POD5 format) through basecalling and quality control.
> This pipeline is based on the workflow originally developed by [@tdido](https://github.com/tdido) in [cnio-bu/myeloma-epi-sv](https://github.com/cnio-bu/myeloma-epi-sv).

## Features

- **Basecalling**: Uses Dorado for basecalling with modified base detection.
- **Quality Control**: NanoPlot quality assessment from Dorado summary output.

## Requirements

- Snakemake (>=8.0)
- Conda or Mamba (for environment management)
- CUDA-compatible GPU

## Installation

1. Clone this repository:
```bash
git clone https://github.com/villena-francis/basecalling_ont.git
cd basecalling_ont
```
2. Create your config file:
```bash
cp config/config.yaml.example config/config.yaml
```
3. Edit the configuration file to match your environment, data locations, and analysis parameters.

## Configuration

The pipeline is configured through the `config/config.yaml` file, which includes:

- **Reference files**: Paths to genome reference, annotations, and other required files
- **Tool paths**: Locations of external tools like dorado and Clair3 models
- **Analysis parameters**: Quality thresholds and analysis options
- **Samples structure**: Hierarchical organization of samples with associated metadata
- **Resource specifications**: Resource allocation for different workflow steps

See `config/config.yaml.example` for a detailed example of the configuration structure.

## Usage

### Basic Execution

Run the full pipeline with:
```bash
snakemake --use-conda --cores <N>
```

### Execution with Slurm

For cluster environments using Slurm:
```bash
snakemake --use-conda --profile slurm
```

## Pipeline Steps

1. **Basecalling**: Convert raw POD5 files to BAM format using Dorado, including modified base detection
2. **Dorado Summary**: Generate per-read summary file from BAM output
3. **Quality Control**: Generate QC metrics and reports with NanoPlot from summary file

## Output Structure

The pipeline generates results in a hierarchical directory structure:

```
results/
├── basecall_dorado/       # Basecalled reads
├── summary_dorado/        # Per-read summary TXT files
└── nanoplot/              # Quality control reports
```