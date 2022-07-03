# TransSSVs
detecting somatic small variants in paired tumor and normal sequencing data with attention-based neural network

TransSSVs takes as input a mixed pileup file generated by Samtools from tumor and normal BAM files. We first select the candidate somatic sites from the mixed pileup file based on the criteria. Next it encodes the mapping information around the candidate somatic sites. Each array is a spatial representation of mapping information adapted for convolutional architecture. For each candidate site, we generate a feature matrix for the content context sequence. Then, the multi-head attention captures the interactions of the genomic sites in the context sequence to achieve a new feature representation of the genomic context. Next,   Finally, these features are used to predict the somatic probability for the candidate somatic sites. Finally, potential somatic small variants determined by the TransSSVs model are generated in the variant call format (VCF).

TransSSVs was tested on Debian GNU/Linux 11 (bullseye) and requires Python 3.

Prerequisites
----------
TensorFlow 2.7.0
sklearn 0.19.2
pandas 1.3.5
numpy 1.21.6
keras 2.7.0


Getting started
----------
1. Run samtools (tested version: 1.8) to convert tumor and normal BAM files to a mixed pileup file required by TransSSVs:\<br>
 `
 samtools mpileup -B -d 100 -f /path/to/ref.fasta [-l] [-r] -q 10 -O -s -a /path/to/tumor.bam /path/to/normal.bam | bgzip > /path/to/mixed_pileup_file
 `
 
 Note: For the case of applying TransSSVs on a part of the whole genome, increase the BED entry by n (the number of flanking genomic sites to the left or right of the candidate somatic site) base pairs in each direction, and specify the genomic region via the option -l or -r.

2.Run identi_candi_sites.py to identify candidate somatic small variants from the mixed pileup file:
