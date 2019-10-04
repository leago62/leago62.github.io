---
layout: post
title:  "Cook Help Project"
date:   2019-6-14
permalink: /projects/Coursework-Bioinf
---

## Overview
### This provides a bit more detail about the coursework I completed studying bioinformatics at UCSD.

&nbsp;

---

&nbsp;

#### Bioinformatics Lab Work

In the bioinformatics lab class, we learned how to use bioinformatics tools to
analyze data in different situations with an emphasis on using genetic data.
Some of the topics we covered include alignment, variant calling, assembly,
GWAS, differential expression, motif finding, and protein sequencing.
Here is a partial list of tools we used in the class:
- samtools
- BWA-MEM
- Varscan
- sklearn (PCA)
- plink
- minima/Spades
- matplotlib (volcano plots)
- GO enrichment analysis
- HOMER
- ScanPy
- Pyteomics
- Mascot
- IGV

The end of the quarter project consisted of trying to replicate a couple of the
tables and figures in a published paper in small groups. We decided to use the
paper "Emergence of Hemagglutinin Mutations During the Course of Influenza
Infection" by A. Cushing et al. (doi:10.1038/srep16178) and got their data
online at the NCBI SRA. We followed their methods as closely as possible. We had
to switch some of the tools because we could find the variant caller (RVD) they
used. Instead of RVD we used Varscan. We had trouble downloading another tool,
PROVEAN, so we used an online tool to change the genetic code with mutations
into an amino acid sequence. I then wrote a short python script to manually
output the mutated amino acids so that we could input the data into the online
PROVEAN program. To create the phylogenetic trees, I wrote another short Python
script to create consensus sequences for each sequence from the genetic
mutations and original sequence. The consensus sequences were then fed into
Quick Tree at PhyML to generate the phylogenetic trees. The following is a
picture of the original vs our replicate.

![Image from my end of year project. Original vs Duplicate](/projects/CSE185-proj.jpg)

&nbsp;

---

&nbsp;

#### Bioinformatics Classes

I took two classes at UCSD that related more to the theoretical side of
bioinformatics. We went over the algorithms and structures behind common
bioinformatics analyses and tools.

In the first class we went over algorithms for motif finding, assembly,
alignment, rearragement, detecting mutations, and clustering. We covered graph
algorithms, randomize algorithms, HMM, and more. The class was supplemented by
coding problems on ROSALIND. These coding challenges could be done in any
language, but I chose to teach myself python in order to complete these
challenges because I had heard it was one of the primary languages for data
science.

In the second class, we went over more theory. Topics included BLAST (E-values,
p-values, scoring matrices, PAM, and BLOSUM), dictionary matching, regular
expressions, HMMs, expression matrix analysis and supervised classification, and
protein expression using mass spec.
