# Bi623 Lab Notebook

## ICA3

### Thu August 28

- Created a conda environment for this assignment and installed MUSCLE (newer vesion, 5.3.linux64 )
```
conda create -n ica3
conda activate ica3
conda install bioconda::muscle
```

- MUSCLE output needs to be in phylip format (interleaved or sequential)
    - Installed seqmagick to help convert output .fa to .phy
    ```
    seqmagick convert --output-format phylip --alphabet dna scn4a_genes.afa scn4a_genes.phy
    ```
    - Spits out an interleaved phylip file

- Installed and ran phyml (PhyML 3.3.20220408)
    - [Output log](phyml.out)
    - [PhyML Manual](https://github.com/stephaneguindon/phyml/blob/master/doc/phyml-manual.pdf)

- Installed figtree on my local machine and used the GUI to view/edit my tree


## Bi621 PS7 aka PS1 
### Part 1 (Bi621, blastp - June 2025):

#### Wed July 9  

- Downloaded 2 FASTAs to Talapas:
    1. https://ftp.ensembl.org/pub/release-114/fasta/homo_sapiens/pep/Homo_sapiens.GRCh38.pep.all.fa.gz
    2. https://ftp.ensembl.org/pub/release-114/fasta/danio_rerio/pep/Danio_rerio.GRCz11.pep.all.fa.gz
- Collaborated with Amanda and Daniella to build function to turn multiline FASTA records into 2 line FASTA records
- Downloaded tables of gene stable IDs, gene names, and protein stable ID for human and zebrafish using Ensembl biomart (Ensembl genes 114)

#### Tue July 15

- Finished part 1 with some help from Hope
- Activated new env in conda for part 2 called 'ps7' and installed BLAST v2.15.0
- Caught a bug with my blastp usage:
    For db flag, use name of db files, not name of folder containing them. For example,
    if db files are named DRE.pdb, DRE.phr, etc., reference just DRE in script for db
- Used output format 6 for blastp
- Commands I used for blast:
```
makeblastdb -in "data/longest_2l_Danio_rerio.GRCz11.pep.all.fa.fasta" \
    -dbtype prot -out "DRE" -parse_seqids

makeblastdb -in "data/longest_2l_Homo_sapiens.GRCh38.pep.all.fa.fasta" \
    -dbtype prot -out "HOMO" -parse_seqids

blastp -query "data/longest_2l_Danio_rerio.GRCz11.pep.all.fa.fasta" \
    -db "HOMO/HOMO"\
    -outfmt 6 -out DanRer_to_HomSapDB_alignment.tsv -num_threads 8 -evalue 1e-6 -use_sw_tback

blastp -query "data/longest_2l_Homo_sapiens.GRCh38.pep.all.fa.fasta" \
    -db "DRE/DRE" -outfmt 6 -out HomSap_to_DanRerDB_alignment.tsv -num_threads 8 -evalue 1e-6 -use_sw_tback
```

#### Wed July 16

##### BLAST output format 6 summary:
<img title="BLAST outfmt 6" src="Screenshot 2025-07-16 at 10.07.26 AM.png">

### Part 2 (Bi623, reciprocal best hits - August 2025)

#### Tue August 26

- got a game plan together for filtering down to reciprocal best hits, two parts:
    1. Identify best hit for each protein in both files
    2. Use those identified hits to check for reciprocals 

* sorted output from original assignment aka part 1 (by protein name then e-value) using the following commands:
    * this is needed for proper parsing by my RCB.py script
    ```{bash}
    sort -k1,1 -k11,11g DanRer_to_HomSapDB_alignment.tsv
    sort -k1,1 -k11,11g HomSap_to_DanRerDB_alignment.tsv
    ```

