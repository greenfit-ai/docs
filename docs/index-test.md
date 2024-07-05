---
layout: default
title: Prepare for the pipeline
nav_order: 3
---

# Prepare for the pipeline
{:.no_toc }
You must supply the raw basecalled reads as fastq files, either gzipped or not, with fastq or fq extension. This will then be followed by some simple steps that are described below.
{:.fs-6.fw-300 }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {:.text-delta }
- TOC
{:toc}
</details>

## Primers
First, you need to provide a csv file containing primers and their features, specifically:

* **ID**: The name of the primer/locus
* **FWD**: The forward primer sequence (5'-3')
* **REV**: The reverse primer sequence (5'-3')
* **PLOIDY**: The expected ploidy for the locus (supported ploidies: 1 for haploid/monoploid, 2 for diploid)
* **SIZE**: The expected size of the sequence associated with the locus

You need to pass all the required data in this exact format, and the file will look like this:

```csv
ID,FWD,REV,PLOIDY,SIZE
sl_an,CAAGCCCTCCTAGTGCTCAA,ATGATTTTCACAAGCATACCTCAA,2,780
sl_hue,CAAGCCCTCCTAGTGCTCAA,AAGATTTCCACGAGCATACCTC,2,780
sl_can,GGATGATGTCTCAAGCCCTTC,TTTTCACGAGCATACCTCAATG,2,780
sl_6n_per,GATGCCTCAAGCCCTCCTA,AAGATTTCCACGAGCATACCTC,2,780
```

> _⚠️BE CAREFUL!⚠️: If you are working with Cytochrome c oxidase subunit I, you should set the ID to **COX1**; if you are working with Internal transcribed spacer, you should set the ID to **ITS**;  If you are working with maturase K and/or ribulose 1,5-biphosphate carboxylase, you should set the ID to **MATK_RBCL**. This is needed for the BOLD species identification analysis to start!_

There is no need to explicitly trim adapters from your fastq files, as the demultiplexing steps take into account their presence. However, make sure not to provide adapters+primers if you have already removed adapters.

## Samples
The input data for the pipeline are fastq files, but to allow multiple fastq files input, you have to list them into a csv document. This document should not have a header and should have the first column representing the name of the sample and the second column reporting the path to the fastq file.

In the end, your samples file will look like this:

```csv
ON_A29_2,/media/user/projects/reads/ON_A29_2.fq.gz
ON_A30_28,/media/user/projects/reads/ON_A30_28.fq.gz
ON_A4_2h,/media/user/projects/reads/ON_A4_2h.fq.gz
ON_A5_29,/media/user/projects/reads/ON_A5_29.fq.gz
ON_A5_3,/media/user/projects/reads/ON_A5_3.fq.gz
```
There is no need for the sample names to match the basenames of the fastq files.

## Usage

To run the pipeline, use the following command:
```
bash AmpliPiper.sh -s <samples_csv> -p <primers_csv> -o <output_folder> [options]
```
**Required Arguments**

* `-s` or `--samples`: Provide the path to a CSV file containing the names and paths of the raw fastq files for each sample.
* `-p` or `--primers`: Provide the path to a CSV file containing the IDs, forward and reverse sequences, and ploidy (1 for haploid, 2 for diploid) of each primer.
* `-o` or `--output`: Specify the path to the output folder.

**Optional Arguments**

* `-b` or `--blast`: Enable BLAST search for species identification (default: disabled).
* `-e` or `--exclude`: Provide a text file with samples and loci to exclude from the analysis.
* `-f` or `--force`: Force overwrite of the previous output folder (default: cowardly refusing to overwrite).
* `-k` or `--kthreshold`: Define the threshold k for the maximum allowed mismatches for primer alignment during demultiplexing (default: 0.05).
* `-m` or `--minreads`: Set the minimum number of reads required for consensus sequence reconstruction (default: 100).
* `-n` or `--nreads`: Provide the absolute number or percentage of top-quality reads to consider for consensus sequence generation and variant calling (default: 500).
* `-q` or `--quality`: Specify the minimum quality score for read filtering (default: 10).
* `-r` or `--sizerange`: Define the allowed size buffer around the expected locus length (default: 100).
* `-t` or `--threads`: Specify the number of threads to use for parallel processing (default: 10).
* `-i` or `--partition`: Use partition model for iqtree with combined dataset (default: disabled)

## Best practices

The following section provides some advice on how to set the parameters for the pipeline:

* `-k` or `--kthreshold` should be set as quite strict (< 0.1) to ensure improved adherence of the demultiplexed sequences to the desired ones.
* `-r` or `--sizerange` can be set liberally (> 200), but be cautious when defining it, as we want to avoid including very short PCR byproducts or very long chimeras.
* `-n` or `--nreads`: 500 reads is generally a sweet spot for the underlying consensus-generating software. However, consider exploring other possibilities if this does not produce the desired output.
* `-m` or `--minreads`: a minimum threshold of less than 100 reads can lead to a lack of output from the underlying consensus generation software.
* `-b` or `--blast`: BLAST search takes a long time when implemented.

If you are having a hard time with the pipeline's output, always try experimenting with the parameter settings. If the problems persist, check out [troubleshooting](./search.md) or [flag an issue on GitHub](https://github.com/nhmvienna/AmpliPiper/issues).
