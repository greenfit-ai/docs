---
layout: default
title: Installation
nav_order: 2
---

# Installation
{: .no_toc }

The installation workflow is simple and mostly automatic, and relies entirely on Mamba and Conda
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Requirements and setup

To install the pipeline, you must meet the following requirements:

* Be on a POSIX or POSIX-like operating system (Ubuntu, Fedora, Debian, CentOS, macOS, etc.)
* Have [mamba](https://mamba.readthedocs.io/en/latest/installation/mamba-installation.html) and [conda](https://conda.io/projects/conda/en/latest/user-guide/install/index.html) installed on your local machine.

If you do not meet these two basic requirements, please adjust to them before installing and running the pipeline.

After verifying these requirements, complete the setup by running:

```bash
git clone https://github.com/nhmvienna/AmpliPiper.git
cd AmpliPiper
```

Now that you are in the pipeline's directory, run the following command to install it:

```bash
bash shell/setup.sh
```

This will create several conda environments in a subdirectory named `envs`. The process may take a while (sometimes it can even last hours, depending on your machine setup) and there might be problems. Please check out [troubleshooting](./search.md) and, if you cannot find a solution or the problem persists, feel free to [flag an issue on GitHub](https://github.com/nhmvienna/AmpliPiper/issues).

## Dependencies

These are the conda environments that will be created:

```
envs/
├── R
├── asap
├── astral
├── bcftools
├── blast
├── iqtree
├── mafft
├── minimap
├── nanofilt
├── parallel
├── pigz
├── python_dependencies
└── samtools
```

Here is the breakdown of the dependencies that are contained within these environemnts:

Here is the markdown list of created conda environments with all the dependencies installed within them:

#### 1. Nanofilt

* Dependencies: `nanofilt`, `pigz`

#### 2. Pigz

* Dependencies: `pigz`

#### 3. Samtools

* Dependencies: `samtools=1.17`

#### 4. Minimap

* Dependencies: `minimap2=2.26`, `samtools=1.17`

#### 5. BCFtools

* Dependencies: `bcftools`

#### 6. Python Dependencies

* Dependencies: `python=3.10`, `pandas`, `matplotlib`, `biopython`, `edlib`, `cairosvg`, `pymsaviz`, `ete3`, `pandas=2.1.1`, `beautifulsoup4`, `scipy`

#### 7. MAFFT

* Dependencies: `mafft=7.520`

#### 8. IQtree

* Dependencies: `iqtree=2.2.5`

#### 9. R

* Dependencies: `r-base=4.1.3`, `bioconductor-ggtree`, `r-tidyverse`, `bioconductor-treeio`, `r-phytools`, `r-ape`, `r-treedist`, `r-reshape2`

#### 10. Astral

* Dependencies: `astral-tree=5.7.8`

#### 11. ASAP

* Dependencies: `asap-v0.1.2-h14c3975_0`

#### 12. GNU Parallel

* Dependencies: `parallel`

#### 13. BLAST

* Dependencies: `blast`



