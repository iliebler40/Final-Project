# Final-Project

## Problem
### Antibiotic resistance is an increasing problem and more individuals are predicted to die from antibiotic resistance than cancer in 20250.

## Goal
### Identify and classify antibiotic resistance genes in Klebsiella pneumoniae

## Tool Required
- Comprehensive Antibiotic-Resistant Database (Detect antibiotic resistant genes) 
- FastQC (Read Quality Assesment)
- SPAdes (Genome Assembly)
- Prokka (Bacterial genome annotation
- Kraken2 (Classification of resistance genes)
- MAFFT (Multiple sequence alignment)
- RAxML (Phylogenetic tree)

# We need Jorge's dataset for the Antibiotic resistant genes (Will obtain SRR numbers then)

**Need to install**
- CARD
- ResFinder
  
3. Download FASTQ files of paired-end reads.
    -  Create a file named `download_sra.sh` with the following content:
    -  Type
```
vi download_sra.sh
```
   - Type `I` and enter the following workflow:
```#!/bin/bash
#SBATCH --job-name=download_sra
#SBATCH --output=download_sra_%j.out
#SBATCH --error=download_sra_%j.err
#SBATCH --time=12:00:00           # Adjust as needed
#SBATCH --partition=standard      # Adjust based on your HPC configuration
#SBATCH --ntasks=102
#SBATCH --cpus-per-task=4         # Optional: fastq-dump can benefit from multithreading
#SBATCH --mem=100G                 # Adjust memory as needed
#SBATCH --mail-type=END,FAIL      # Optional: email on job end/failure
#SBATCH --mail-user= irliebler@svsu.edu

# Load necessary modules
module load sra-toolkit

# Ensure the output directory exists
mkdir -p fastq_files

# Download each SRR
while read -r SRR; do
    echo "Downloading $SRR..."
    prefetch --max-size 100G "$SRR"
    fastq-dump --gzip "$SRR" --split-files -O fastq_files/
done < srr_accessions.txt
```
