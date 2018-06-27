## Table of Contents

- [Users' Guide](#uguide)
  - [Installation](#install)
  - [Functions](#functions)
    - [readprep](#readprep)
    - [annotation](#annotation)
    - [mapping](#mapping)
  - [Getting help](#help)
  - [Citing MetaT](#cite)
- [Limitations](#limit)

## <a name="uguide"></a>Users' Guide

MetaT is a wrapper tool for fast and simple preprocessing of NGS transcriptomics 
data, combining the most recent and fastest software currently available in a 
user-friendly format. The general workflow of the MetaT wrapper tool is:

```sh
# QC of reads.
MetaT readprep -i samples -t 10

# Annotate assembly.
MetaT annotation -a assembly.fasta -o annotation.fasta -t 10

# Map reads to annotated assembly.
MetaT mapping -i RNAreads -a anotation.fasta -o counts.txt -t 10
```

**Importantly**, the pipeline is only setup to handle transcriptomics data from
a 50bp SR illumina library. Furhtermore, the annotation database are only setup 
for Bacteria at the moment.

### <a name="install"></a>Installation

The source code for MetaT can be aquired by:
```sh
git clone https://github.com/TYMichaelsen/MetaT
```

### <a name="functions"></a>Functions

#### <a name="readprep"></a>readprep

Prepare RNA reads for mapping, by performing adapter trimming, Q-score filtering, and rRNA removal of 50bp SR. 

**MetaT readprep [-h] [-d dir -i file -o dir -q value -t value]**

Arguments:

    -h  Show this help text.
    -d  Directory to search for raw Illumina SR sequencing data.
    -i  List of prefixes for files to search for.
    -o  Output directory to put QC'ed reads. Defaults to 'RNAreads' in cd.
    -q  Q-score threshold. Default: 20.
    -t  Number of threads. Defaults: 10.

Output:

 1) .fasta files of curated reads in -o directory.
 2) A file 'seqstat.txt' containing count statistics of reads during each step. Dumped in cd.
 3) A file 'rRNAreads.fa' containing all rRNA reads found. Dumped in cd.

Note: 

The -i option relies on the typical naming convention of demultiplexed Illumina output files, meaning that the prefix is consistent and unique for all files (read no., lane) for a particular sample. Make sure this is the case. The code will concatenate all files with same prefix before downstream processing.

Requirements:

- BBMap

Details:

The `readprep` function utilizes [BBmap](https://jgi.doe.gov/data-and-tools/bbtools/bb-tools-user-guide/bbmap-guide/) 
tools to perform the actual adapter trimming, Q-score filtering and rRNA removal. 
The `BBMap` tool has build-in adapters which covers basically all NGS adapters. 
The SILVA [Life Tree Project](https://www.arb-silva.de/projects/living-tree) (LTP) 
database version 132 is used as reference for rRNA removal.

#### <a name="annotation"></a>annotation

**MetaT annotation [-h] [-a file -g file -d dir -o file -t value]**

Arguments:

    -h   Show this help text.
    -a   Assembly to be annotated.
    -g   Genome(s) to be annotated. Matching reads in the assembly (if provided) are filtered away before annotation. 
    -d   Folder, containing .gbff files for custom database.
    -o   Output file. Defaults to 'annotation.fasta'.
    -t   Number of threads. Default: 10.

Output:

A fasta file with header as follows;  
ID contig|ftype|EC_number|gene|product|locus_tag|function|inference

Requirements:

- prokka

Details:

The `annotation` function utilizes [prokka](https://github.com/tseemann/prokka) 
to perform the search for ORFs and annotation. 

#### <a name="mapping"></a>mapping

STUFF

### <a name="help"></a>Getting help

If you encounter bugs or have further questions or requests, you can raise an issue 
at the [issue page](https://github.com/TYMichaelsen/MetaT/issues).

### <a name="cite"></a>Citing MetaT

As MetaT still is in development, no citing is available.

## <a name="limit"></a>Limitations

MetaT is developed for a very specific purpose: 

* Mapping 50bp SR illumina reads to annotated microbial metagenomes.

All other usecases you're on your own!
