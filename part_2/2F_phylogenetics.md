# Introduction to Phylogenetics
##  3rd Annual CPGME Symposium Pathogen Genomics Workshop

## April 23, 2024

_Ramon Lorenzo-Redondo, Ph.D._ (<ramon.lorenzo@northwestern.edu>)


### Phylogenetic analyses

During this part of the workshop we are going to introduce basic concepts needed to perform phylogenetic analysis. We will run various steps to complete a single exercise that will help us understand the general steps needed to complete phylogenetic and phylodynamics analysis.

### Exercise

Imagine that you are studying the molecular evolution of SARS-CoV-2. After a huge surge of SARS-CoV-2 cases during the winter of 2021-2022 due to the appearence of the SARS-CoV-2 Omicron lineages, reports indicate that new lineages of this Variant are spreading. These lineages, BA.4 and BA.5, seem to be spreading rapidly and you want to analyze their characteristics, population dynamics, and geographical origin. We will use full genome sequences from all viruses designated as BA.4 until April 30th 2022, to study this lineage.

![image](https://github.com/ramonlorenzo/CPGME_Workshop2024/assets/73665195/8926d95d-7d2a-48fb-8535-f569ec1553e9)


#### 1. Alignments
After understanding the basic concepts of Multiple Sequence Alignment, we are going to generate an alignment as a first stepo of our analysis. We will use all BA.4 SARS-CoV-2 full genome sequences that were publicly available by April 30th 2022 from **GISAID**(https://gisaid.org/). We will use the SARS-CoV-2 reference genome to speed-up the process.

```
mafft --auto --thread -1 --keeplength --addfragments SARSCoV2_BA4.fasta CoV2RefSeq.fasta > SARSCoV2_BA4.sequences_aligned.fasta
```

To compare well-resolved alignments with more difficult ones, we will also generate another alignment with a highly variable virus, HIV-1, using sequences dowloaded from the HIV sequence database (https://www.hiv.lanl.gov/content/index). We will analyze the highly variable Envelope (receptor binding protein) gene.


**High variability alignment**
```
mafft --auto --thread -1 HIV1_CON_2021_env_DNA.fasta > HIV1_CON_2021_env_DNA_aligned.fasta
```
Visualize each alignment with **seaview**.

<img width="80" alt="image" src="https://github.com/ramonlorenzo/CPGME_Workshop2024/assets/73665195/d214425c-86a0-48d4-9858-2bc4c7aaf029">


#### 2. Tree building (ML Phylogenies)

Now that we have covered the theoretical background behind phylogenetic tree inference, we are going to practice using the ML method. 

Generate an ML tree using the alignment from **Exercise 1**
```
iqtree2 -s SARSCoV2_BA4.sequences_aligned.fasta -T AUTO -B 100
```
We will visualize the tree with **FigTree**

<img width="69" alt="image" src="https://github.com/ramonlorenzo/CPGME_Workshop2024/assets/73665195/6d7e91bc-2514-4484-a986-50786efa67d8">


* If it takes too long we will run this instead (this command will provide the model and not run bootstrap):

```
  iqtree2 -s SARSCoV2_BA4.sequences_aligned.fasta -T AUTO -m GTR+F+R3 
```

#### 3. Phylodynamics

We will apply simple concepts of coalescence and phylodynamics to analyze population dynamics and the origin of the BA.4 lineage of SARS-CoV-2. We will use the tree inferred in **Exercise 2**. We will use TreeTime to perform this analysis.


  1. Use the ML tree and metadata including dates to generate a temporal tree and coalescence analysis using ML approach.
  
    ```
    treetime --confidence --relax 1.0 0.5 --aln SARSCoV2_BA4.sequences_aligned.fasta --tree SARSCoV2_BA4.sequences_aligned.fasta.treefile --dates SARSCoV2_BA4.metadata.tsv --coalescent skyline --clock-filter 4 --clock-rate 0.0008 --clock-std-dev 0.0004 --branch-length-mode marginal --outdir timetree_out
    ```
  2. Perform ML ancestral state reconstruction to analyze phylogeography.

    ```
    treetime mugration --tree timetree_out/timetree.nexus --states SARSCoV2_BA4.metadata.tsv --attribute country
    ```

We will visualize the tree with **FigTree**

<img width="69" alt="image" src="https://github.com/ramonlorenzo/CPGME_Workshop2024/assets/73665195/6d7e91bc-2514-4484-a986-50786efa67d8">

**With this final visualization we can estimate the most likely origin and expansion pattern of BA.4.**

---

# [Back to table of contents](../README.md)

---

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.