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
```bash
#!/bin/bash
# Download all SRR files from PRJNA759579
# esearch -db sra -query PRJNA759579 | efetch -format runinfo > runinfo.csv
# cut -d ',' -f 1 runinfo.csv | grep SRR > srr_accessions.txt

# Use prefetch to download all files
module load sra-toolkit
while read -r SRR; do
   echo "Downloading $SRR..."
   prefetch --max-size 100G $SRR 
   fastq-dump --gzip $SRR --split-files -O fastq_files/
done < srr_accessions.txt
```
