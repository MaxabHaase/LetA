# LetA DMS  

Python scripts for analyzing Deep Mutational Scanning (DMS) data in the LetA experiment.  

## Requirements  

Ensure you have **Python 3+** installed along with the required libraries:  

- `argparse`
- `pandas`
- `numpy`  

### Tested Versions  

| Software  | Version  |  
|-----------|----------|  
| Python    | 3.9      |  
| argparse  | 1.1      |  
| pandas    | 1.4.2    |  
| numpy     | 1.21.6   |  

## Installation  

1. Install Python from [python.org](https://www.python.org/downloads/).

## Usage  

These scripts process **two multi-FASTA files** containing read variants from a **DMS experiment** under two conditions:  

- **Control condition** (no selection)  
- **Experimental condition** (with selection)  

### Running the Scripts  

Create a **Bash script** to execute the scripts sequentially:  

```bash
#!/usr/bin/env bash
#$ -cwd
#$ -S /bin/bash

sample_1=$1  # Path to merged FASTQ file from selective condition
sample_2=$2  # Path to merged FASTQ file from non-selective condition
p3=$3        # Label for the sub-library (e.g., --library A, B, C, D)

python3.8 uniq_LetA.py -s ${sample_1} -l ${p3} &
python3.8 uniq_LetA.py -s ${sample_2} -l ${p3}

wait

python3.8 logfitness_n.py -s1 ${sample_1} -s2 ${sample_2} -l ${p3}
```

## Expected Output  

Upon successful execution, the following output files will be generated:  

1. **Sequence Length Distribution (CSV)** – Quality check  
2. **Filtered Sequences (FASTA)** – Length-filtered sequences  
3. **Reading Frame Adjusted Sequences (FASTA)**  
4. **Amino Acid Translations (FASTA)**  
5. **Raw Amino Acid Variant Counts (CSV)** – Per-position counts in the LetA gene  
6. **Fitness Scores (CSV)** – Read count normalized fitness scores per amino acid variant  

⚠️ **Note:** Outputs **1–5** are generated for **each condition**, while **output 6** is a single combined result.  
