# LetA DMS
Python Scripts used for LetA DMS

Requirements:
Python 3+, with the required libraries argparse, pandas, and numpy.

Tested with the following verisons:
python v3.9
argparse v1.1
pandas v1.4.2
numpy v1.21.6

Installation:
All that is needed is an installation of python (https://www.python.org/downloads/) with the required libraries. 

Instructions on  running the scripts:

Inputs should be two multifasta files that contains read variants from a DMS experiment. You will need two conditions, for example a control condition (no selection) and an experimental condition (with selections). To run, set up a bash script to run both scripts sequentially. They are written to work with the LetA DMS experiment. As such, modification would be required for other datasets. 

Example Bash Script:
```
 #!/usr/bin/env bash
  #$ -cwd
  #$ -S /bin/bash

  sample_1=$1 #path to merged fastq file from selective condition
  sample_2=$2 #path to merged fastq file from non-selective condition
  p3=$3 #label to indicate which "sub-library" fastq files are from (In case of LetA, --library A (or B, C, D))

  python3.8 uniq_LetA.py -s ${sample_1} -l ${p3} &
  python3.8 uniq_LetA.py -s ${sample_2} -l ${p3}

  wait

  python3.8 logfitness_n.py -s1 ${sample_1} -s2 ${sample_2} -l ${p3}
```

Expected output:

If all runs well, you should expect the following files:
1. CSV file containing the sequence length distribution (quality check)
2. fasta file containing sequences flitered by length
3. fasta file containing sequences in the correct reading frame
4. fasta file containing amino acid translations
5. CSV file containing the raw counts of each amino acid variant at all positions in the LetA gene
6. CSV containing the read count normalized fitness scores of each amino acid variant at all positions in the LetA gene (or which ever gene, pending modifications).

For outputs 1–5 it is expected to have a file for each condition and only a single output 6.


