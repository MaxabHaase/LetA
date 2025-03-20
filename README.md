# LetA DMS
Python Scripts used for LetA DMS

Requirements:
Python 3+, with the required libraries 

Installation:
All that is needed is an installation of python (https://www.python.org/downloads/) with the required libraries. 

Instructions on  running the scripts:

Input should be a multifasta file that represents each variant from a DMS experiment. To run, set up a bash script to run both scripts sequentially. They are written to work with the LetA DMS experiment. As such, modification would be required for other datasets. 

Example Bash Script:
```
 #!/usr/bin/env bash
  #$ -cwd
  #$ -S /bin/bash

  sample_1=$1 #path to merged fastq file from selective condition
  sample_2=$2 #path to merged fastq file from non-selective condition
  p3=$3 #label to indicate which "sub-library" fastq files are from

  python3.8 uniq_LetA.py -s ${sample_1} -l ${p3} &
  python3.8 uniq_LetA.py -s ${sample_2} -l ${p3}

  wait

  python3.8 logfitness_n.py -s1 ${sample_1} -s2 ${sample_2} -l ${p3}
```


Expected output:

If all runs well, you should expect a single csv file containing the read count normalized fitness scores of each amino acid variant at all positions in the LetA gene (or which ever gene, pending modifications).


