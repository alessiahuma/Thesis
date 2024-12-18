This repository contains a data processing pipeline designed to handle and analyze microbial genome and protein data, focusing on identifying and working with mcy (microcystin-producing) proteins and their genomic contexts for the example, however can be used for any protein genes of interest.  The ultimate goal is to generate cleaned, aligned sequence files, being operon, prot, or genes of interest and 16S gene, for downstream phylogenetic tree constructions to compare the evolution of these genes.


Before running this pipeline, ensure the following files are available in the same directory:

clusters.txt (from BLAST) — Cluster file for each mcy prot (or prots of interest), in its own directory (e.g A/clusters.txt, B/clusters.txt, and C/clusters.txt represent the 3 mcy prots)
new.gcf2rRNA — Script for generating rRNA data.
conc_mafft — Script for concatenating aligned protein sequences.
awk_step3 — Custom awk script for filtering protein sequences.
updt_operon_config_code — Configuration file for operon updates. 

Note: If operon_config_code is used instead of updt_operon_config_code it requires create_dupl_fna. This is because it includes all genomes (gcfs) that have match for all prot of interest or match within all prot directories (e.g A,B, and C), that means dif genomes can have exact same prots, could lead to duplicate entries in fasta file, use at your discretion.

Extract TaxIDs and Proteins
For each prot directory (named with a capital letter), the script extracts tax IDs and protein sequences from the clusters.txt file. It generates the following output files for each directory:
prot.taxids: List of unique protein tax IDs.
taxids: List of unique tax IDs.
prot: List of unique proteins.

Obtain Fasta Files
The script fetches protein sequence data in FASTA format for each protein listed in the prot file. It performs various filtering steps to remove redundant sequences and match protein sequences with their respective tax IDs.

Obtain GCF Files
For each tax ID, the script queries the GenBank dataset for genome information and retrieves the corresponding Genome Coordination Files (GCFs). It filters GCFs based on matching tax IDs and removes redundant sequences.

Filter GCFs with 16S rRNA
The pipeline eliminates GCFs without 16S rRNA sequences or those with multiple 16S rRNA sequences. It checks the presence of 16S rRNA in the GCF files and records the ones with exactly one 16S rRNA sequence.

Extract Highest PI Protein Sequences
The pipeline calculates the highest percent identity (PI) for proteins within each GCF and selects the sequences with the highest PI, since GCF can have multiple versions of same prot within it.

Perform Sequence Alignment
Using the mafft tool, the pipeline aligns protein sequences for each GCF. The sequences are concatenated across different directories, and the final aligned sequences are saved for further analysis.

16S Data Processing
The script handles 16S rRNA data by processing genomic GFF files and generating edited FASTA files with taxonomic information for 16S rRNA sequences.

Output Files
Protein Sequence Files: Aligned protein sequences in FASTA format (e.g., the.mafft).
GCF and Taxonomy Files: Filtered GCF files based on taxonomic criteria and the presence of 16S rRNA.
Aligned Data: MAFFT-aligned protein and 16S sequences ready for further downstream analyses.

After executing this scrpt, the mafft files allow user to be ready to then build IQTREES in one simple line on command line without further steps.
