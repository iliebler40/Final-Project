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
```
 nano download_sra_job.sh
```
```
#!/bin/bash
#SBATCH --job-name=download_sra
#SBATCH --output=logs/download_%j.out
#SBATCH --error=logs/download_%j.err
#SBATCH --time=12:00:00
#SBATCH --mem=100G
#SBATCH --cpus-per-task=4
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=jrander3@svsu.edu

# Ensure log and output directories exist
mkdir -p logs
mkdir -p fastq_files

# Load the SRA Toolkit module
module load sra-toolkit

# Read SRR accession numbers and download each
while read -r SRR; do
   if [[ -n "$SRR" ]]; then
      echo "Downloading $SRR..."
      prefetch --max-size 100G "$SRR"

      if [[ $? -eq 0 ]]; then
         echo "Converting $SRR to FASTQ..."
         fastq-dump --gzip --split-files -O fastq_files/ "$SRR"
      else
         echo "Failed to download $SRR" >&2
      fi
   fi
done < srr_accessions.txt
```
```
sbatch download_sra_job.sh
```
