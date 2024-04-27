---
layout: default
title: Pipeline
nav_order: 4
---

# Pipeline workflow
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Preprocessing

The preprocessing step consists of two independent parts:

* **Primers comparison**: primers are aligned, and their relative edit distance is calculated and represented as a heatmap. This serves as an informative step for the user, allowing them to adjust the k-threshold parameter based on the primers' distance.
* **Quality filtering**: accomplished using NanoFilt, this step excludes reads that fall below the minimum quality threshold (set by passing the `--quality` option).

## Demultiplexing
Demultiplexing is a complex and delicate task for the pipeline, accomplished by a custom Python script, and is highly dependent on the parameters (especially k-threshold and size range) provided to the pipeline:

1. First, the program reads the fastq file and the primers table.
2. Next, the program iterates through each read in the fastq file, looking for a matching pattern for each primer. If the analyzed sequence aligns with both the forward and reverse primers, and the mismatches in these alignments are less than k \* len(primer), it gets demultiplexed with the primer taken into account.
3. Once all reads have been demultiplexed, a quality selection takes place. If there are more reads than those specified with the `--nreads` option, only the top-quality ones are written to the final demultiplexed files. Otherwise, all reads are transcribed to the demultiplexed output file, but only if their number is higher than the one specified with `--minreads`.

This task is parallelized across multiple threads (when allowed).

## Consensus generation
Each demultiplexed file undergoes a round of consensus sequence generation, carried out by AmpliconSorter.

From the raw outputs by AmpliconSorter, the most supported consensus sequences (those with the highest read depth) are selected.

Amplicon Sorter outputs are summarized into a csv file, which will be employed for haplotype reconstruction.

## Haplotype reconstruction
Ploidy-based haplotype reconstruction is achieved with the help of the custom Python script ChooseConsensus.py, which tests the consistency of various ploidy assets within the reads (based on SNPs) and outputs the consensus sequences to be kept for downstream analysis, labeling them as haplotypes (with _1, _2, _3... as a series number in their header).

## Phylogenetic analysis
Phylogenetic analysis begins with haplotypes alignment, accomplished by MAFFT.

After the alignment, maximum-likelihood phylogenetic trees are inferred for each locus separately using IQtree (with 100 bootstrapping rounds). Once all the loci are concatenated, an overall tree is built by Astral.

All the trees are plotted by custom R scripts, and their relative Robinson-Foulds distance is calculated to assess their similarity.

## Species Delimitation
Species delimitation relies on ASAP, which identifies the best partitioning model for the haplotypes, delimiting putative species groups.

## Species Identification
Species identification is achieved through two API services:

* **BOLD API**: Based on a set of custom Python scripts, this API interface queries the BOLD database, but only for three loci: COX1, ITS, and MATK_RBCL (these must be explicitly specified as primer IDs in the primers csv). This process is generally slow (due to a self-imposed restriction to a maximum of 4 concurrent submissions to avoid overusing the API), but faster than BLAST. Moreover, being a curated database, BOLD's results are more reliable than BLAST's.
* **BLAST API**: Based on a custom script and provided by NCBI through Biopython, due to the request restriction policy by NCBI itself, the API is really slow in retrieving results, especially if there is a high number of query sequences (> 50). This species identification option is generally disabled.

## HTML Summarization
All the results are summarized in an HTML file by a custom Python script, making it easier to visualize the massive pipeline throughput.

Moreover, the MSA files output by MAFFT are turned into visual representations of the alignment thanks to a custom Python script.
