<!---
(Modelled after Will-Rowe's beautiful SEAR README https://github.com/will-rowe/SEAR/blob/master/README.md)
-->

[![Gitter chat](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/nextflow-tychus/Lobby?utm_source=share-link&utm_medium=link&utm_campaign=share-link)

Tychus: A tool to characterize the bacterial genome.
====================================================

Table of Contents
-----------------

[Overview](https://github.com/cdeanj/nextflow-tychus#overview)

[Requirements](https://github.com/cdeanj/nextflow-tychus#requirements)

[Quickstart](https://github.com/cdeanj/nextflow-tychus#quickstart)

[Run a Test](https://github.com/cdeanj/nextflow-tychus#run-a-test)

[Options](https://github.com/cdeanj/nextflow-tychus#pipeline-options)

[Example Usage](https://github.com/cdeanj/nextflow-tychus#example-usage)

[Results](https://github.com/cdeanj/nextflow-tychus#results)

[Dependencies](https://github.com/cdeanj/nextflow-tychus#dependencies)

[Contact](https://github.com/cdeanj/nextflow-tychus#contact)

-----------------

Overview
========
Tychus is a tool that allows researchers to perform massively parallel sequence data analysis with the goal of producing a high confidence and comprehensive description of the bacterial genome. Key features of the Tychus pipeline include the assembly, annotation, and phylogenetic inference of large numbers of WGS isolates in parallel using open-source bioinformatics tools and virtualization technology. The Tychus pipeline relies on two methods to characterize your bacterial sequence data.

The first method is assembly based. The assembly module attempts to produce a comprehensive reconstruction of the genome by relying on the results of multiple *de novo* genome assemblies through the use of multiple assemblers. These assemblies are then used to produce a hybrid or consensus assembly with fewer and longer contigs that can be used as a draft genome for further downstream processes such as annotation, a process by which genomic features of interest are identified and appropriately labelled. Assemblies are then evaluated based on common scoring metrics, such as number of contigs, contig size, and N50.

The second method is alignment based. The alignment module attempts to produce a thorough description of your bacterial sequence data by identifying related single nucleotide polymorphisms (SNPs) with the goal of producing SNP phylogenies that can aid in inferring the relatedness and origin of your samples. In addition, information about the types of genes, whether they be antimicrobial, virulence, or plasmids are also identified and can be used for further analysis and interrogation.

These two modules are not completely independent. Contigs produced from the `assembly` module can be used as input to the `alignment` module. In addition to the user-input reference genome and raw read sequences, these draft genomes can be used by the module's downstream processes to identify SNPs and build phylogenetic trees.

-------

Requirements
============

Minimum Hardware Requirements
-----------------------------
  - 16+ gigabytes (GB) of RAM.
  - 125+ gigabytes of hard drive (HDD) space.

The Tychus pipeline is intended to be utilized on Linux servers with large amounts of RAM and disk space with multiple computing cores. The requirements listed above are a must for demonstration purposes.


Software Requirements
---------------------
  - Java 7+
  - Docker
    - Windows users should download the [Stable channel](https://docs.docker.com/docker-for-windows/) release.
    - MAC users should download the [Stable channel](https://docs.docker.com/docker-for-mac/) release.
    - Linux users can [download](https://docs.docker.com/engine/installation/) the most appropriate version for their Linux distribution.

To check your Java version, type the following command into a terminal:
```
$ java -version
```
-----------

Quickstart
==========
Install Nextflow
----------------
Open a terminal and type the following commands (omitting the '$' sign):
```
$ mkdir tychus
$ cd tychus/
$ curl -fsSL get.nextflow.io | bash
$ ./nextflow
```

If installing Nextflow behind a proxy server, you may encounter the following `error` message:
```
$ Unable to initialize nextflow environment
```
In this case, you can type the following commands to obtain the Nextflow executable.
```
$ wget -O nextflow http://www.nextflow.io/releases/v0.23.0/nextflow-0.23.0-all
$ chmod u+x nextflow
$ ./nextflow
```

### Add To Path
Add the Nextflow executable to your system path. You can accomplish this by typing one of the two commands:
```
$ mv nextflow /usr/local/bin
```
or
```
$ export PATH=$PATH:$PWD
```

Install Tychus Pipeline
-----------------------
The Tychus pipeline can be pulled and installed from Github with the following command:
```
$ git clone https://github.com/cdeanj/nextflow-tychus.git
$ cd nextflow-tychus/
```

Install Docker Images
---------------------
Depending on which Tychus module you would like to run, you will need to download the appropriate Docker image in order to resolve the module's tool dependencies. These can be easilly downloaded by typing the following command(s):
```
$ docker pull abdolab/tychus-alignment
$ docker pull abdolab/tychus-assembly
```
The download time will take between 5 and 10 minutes depending on your connection speed.

----------

Run a Test
==========
It is `recommended` that you run these tests for both the `alignment` and `assembly` modules before doing any large-scale analysis. This serves the purpose of getting you comfortable with running each Tychus module, as well as providing you with real output, which you can look back upon later when you get to the [Results](https://github.com/cdeanj/nextflow-tychus#results) section. The reads used in each test were produced with [Art](https://www.niehs.nih.gov/research/resources/software/biostatistics/art/), an artificial read simulator, and constructed with 20x-30x coverage.

Alignment Module
----------------
Included in the `alignment` module are four databases: a resistance, virulence, plasmid, and reference. These are used by default when running data through this module. The simulated reads mentioned above are also used and can be found in the `tutorial/raw_sequence_data/` directory. To get started, run the following command within the `nextflow-tychus/` directory:
```
$ nextflow alignment.nf -profile alignment --threads 2 --output my_alignment_output
```

Results should be produced shortly, and you will see the following message:
```
Nextflow Version:	0.23.0
Command Line:		nextflow run alignment.nf -profile alignment --threads 2 --output my_alignment_output
Container:			abdolab/tychus-alignment
Duration:			5m 28s
Output Directory:	/home/username/nextflow-tychus/my_alignment_output
```

Assembly Module
---------------
Included in the `assembly` module is a reference to the simulated reads mentioned above. You will not need to specify the location of any reads in this example. To get started, run the following command within the `nextflow-tychus/` directory:
```
$ nextflow assembly.nf -profile assembly --threads 2 --output my_assembly_output
```

Since we are doing *de novo* assemblies, this could take a while, but hopefully not too long! When everything is said and done, you should see the following message:
```
Nextflow Version:       0.23.0
Command Line:           nextflow run assembly.nf -profile assembly --threads 2 --output my_assembly_output
Container:              abdolab/tychus-assembly
Duration:               15m 28s
Output Directory:       /home/username/nextflow-tychus/my_assembly_output
```

See below for a list of available options included in each Tychus module.

Pipeline Options
================
To view available pipeline options for each of the Tychus modules, you can type the following command(s) into a terminal:

Alignment Module
----------------
```
$ nextflow run alignment.nf --help

N E X T F L O W  ~  version 0.23.0
Launching `alignment.nf` [tender_wing] - revision: aa90f777d3

Tychus - Alignment Pipeline

Usage: 
    nextflow alignment.nf -profile alignment [options]

General Options: 
    --read_pairs      DIR		Directory of paired FASTQ files
    --genome          FILE		Path to the FASTA formatted reference database
    --amr_db          FILE		Path to the FASTA formatted resistance database
    --vf_db           FILE		Path to the FASTA formatted virulence database
    --plasmid_db      FILE		Path to the FASTA formatted plasmid database
    --threads         INT		Number of threads to use for each process
    --out_dir         DIR		Directory to write output files to

Trimmomatic Options: 
    --leading         INT		Remove leading low quality or N bases
    --trailing        INT		Remove trailing low quality or N bases
    --slidingwindow   INT		Scan read with a sliding window
    --minlen          INT		Drop reads below INT bases long
    --adapters        FILE		FASTA formatted adapter sequences

kSNP Options: 
    --ML              BOOL		Estimate maximum likelihood tree
    --NJ              BOOL		Estimate neighbor joining tree
    --min_frac        DECIMAL	Minimum fraction of genomes with locus

Figtree Options: 
    --JPEG            BOOL		Convert newick tree to annotated JPEG
    --PDF             BOOL		Convert newick tree to annotated PDF
    --PNG             BOOL		Convert newick tree to annotated PNG
    --SVG             BOOL		Convert newick tree to annotated SVG
```

Assembly Module
---------------
```
$ nextflow run assembly.nf --help

N E X T F L O W  ~  version 0.23.0
Launching `assembly.nf` [sleepy_bohr] - revision: 05adc382a5

Tychus - Assembly Pipeline

Usage: 
    nextflow assembly.nf -profile assembly [options]

General Options: 
    --read_pairs      DIR		Directory of paired FASTQ files
    --threads         INT       Number of threads to use for each process
    --output          DIR       Directory to write output files to

Trimmomatic Options: 
    --leading         INT		Remove leading low quality or N bases
    --trailing        INT		Remove trailing low quality or N bases
    --slidingwindow   STR		Scan read with a sliding window
    --minlen          INT		Drop reads below INT bases long
    --adapters        FILE		FASTA formatted adapter sequences	
    
Prokka Options:
    --genus           STR		Target genus
    --species         STR		Target species
```

----------------

Example Usage
=============
Some pipeline options can be used by both Tychus modules. Example usages for identical parameters are provided side-by-side where applicable.


FASTQ Input
-----------
The most useful command for both modules will be to read in your sequence data. With Nextflow, we can specify a command line glob to provide a directory of FASTQ files as input. Doing so will allow Nextflow to process data in parallel, using multiple processors. For example, a typical command may look like the following:

```
$ nextflow alignment.nf -profile alignment --read_pairs "tutorial/raw_sequence_data/*_R{1,2}_001.fastq.gz"
```
or
```
$ nextflow assembly.nf -profile assembly --read_pairs "tutorial/raw_sequence_data/*_R{1,2}_001.fastq.gz"
```

Here, we are using the `*` wildcard to grab all files within the `tutorial/raw_sequence_data/` directory. The `{1,2}` wildcards allows us to further group the files based on the presence of an `_R1` or `_R2` substring. What is returned is a sorted list of files that Nextflow can group together and process appropriately.

Trimmomatic Operations
----------------------
Trimmomatic comes with four FASTA formatted adapter files (NexteraPE-PE.fa, TruSeq2-PE.fa, TruSeq3-PE.fa, TruSeq3-PE-2.fa). To remove adapter specific sequences or modify the default trimming operations, you can enter the following command:

```
$ nextflow alignment.nf -profile alignment --read_pairs "tutorial/raw_sequence_data/*_R{1,2}_001.fastq.gz" --leading 5 --trailing 5 --slidingwindow 5:16 --minlen 45 --adapters NexteraPE-PE.fa
```
or
```
$ nextflow assembly.nf -profile assembly --read_pairs "tutorial/raw_sequence_data/*_R{1,2}_001.fastq.gz" --leading 5 --trailing 5 --slidingwindow 5:16 --minlen 45 --adapters NexteraPE-PE.fa
```

kSNP Operations
---------------
By default, maximum likelihood (ML) trees are computed with kSNP. Although this is the `recommended` tree format to produce, you can specify the neighbor joining (NJ) method by including the `--NJ` option. Furthermore, you can enter a decimal number between 0 and 1 specifying the fraction of loci that must be present in all genomes to be included in the resulting SNP phylogeny.
```
$ nextflow alignment.nf -profile alignment --read_pairs "tutorial/raw_sequence_data/*_R{1,2}_001.fastq.gz" --NJ --min_frac 0.85
```

Figtree Options
---------------
By deafult the SNP phylogenies produced by kSNP are written to a [Newick](https://en.wikipedia.org/wiki/Newick_format) formatted `.tre` file. Figtree is used to produce phylogenies in the image format of your choosing. By default, SNP phylognies are annotated and saved as PNG images. To change this, simply specify an alternative image format (JPEG,PDF,SVG).
```
$ nextflow alignment.nf -profile alignment --read_pairs "tutorial/raw_sequence_data/*_R{1,2}_001.fastq.gz" --JPEG
```

Prokka Options
--------------
We allow users to annotate contigs using BLAST specific databases. To do this, you must specify both the `genus` and `species` parameters. The default annotation method is to not use a BLAST specific database.
```
$ nextflow assembly.nf -profile assembly --read_pairs "tutorial/raw_sequence_data/*_R{1,2}_001.fastq.gz" --genus Listeria --species monocytogenes
```

Database Options
----------------
If you would like to specify an alternative `reference`, `virulence`, `plasmid` or `resistance` database than the ones provided, you can do that as well.
```
$ nextflow alignment.nf -profile alignment --read_pairs "tutorial/raw_sequence_data/*_R{1,2}_001.fastq.gz" --ref_db "path/to/your/reference/db/ref.fa" --vf_db "path/to/your/virulence/db/vf.fa" --plasmid_db "path/to/your/plasmid/db/plasmid.fa" --amr_db "path/to/your/resistance/db/resistance.fa"
```

Other Options
-------------
Here are some more options. The `threads` parameter allows you to control how many threads each process will use. By default, this value is set to 1. The `output` directory allows you to specify where outputs will be stored.
```
$ nextflow alignment.nf -profile alignment --read_pairs "tutorial/raw_sequence_data/*_R{1,2}_001.fastq.gz" --threads 4 --output dir
```
or
```
$ nextflow assembly.nf -profile assembly --read_pairs "tutorial/raw_sequence_data/*_R{1,2}_001.fastq.gz" --threads 4 --output dir
```

----------------

Results
=======
Alignment Module
----------------
Directory | Description
--------- | -----------
Alignment | `Contains all BAM formatted alignment files produced by the alignment of reads against the user-input reference, plasmid, resistance, and virulence databases.`
Consensus| `Contains all FASTA formatted consensus sequences produced by the VCF formatted SNPs called by FreeBayes.`
PreProcessing | `Contains all FASTQ formatted trimmed sequence files produced by Trimmomatic.`
Resistome | `Contains all TSV formatted resistome files.`
SNPsAndPhylogenies | `Contains all SNPs and Newick formatted Phylogenies produced by kSNP3. The SNP files can be found in the SNPs/ subdirectory. The Newick formatted phylogenies can be found in the Trees/ directory. The Newick formatted image files can be found in the TreeImages/ directory.`

Assembly Module
---------------
Directory | Description
--------- | -----------
AbyssContigs | `Contains all FASTA formatted contigs produced by the Abyss assembler.`
IDBAContigs| `Contains all FASTA formatted contigs produced by the IDBA-UD assembler.`
SPAdesContigs | `Contains all FASTA formatted contigs produced by the SPAdes assembler.`
VelvetContigs | `Contains all FASTA formatted contigs produced by the Velvet assembler.`
IntegratedContigs | `Contains all super assembly contigs produced by the CISA contig integrator.`
AnnotatedContigs | `Contains all annotation files produced by Prokka.`
AssemblyReport | `Contains all assembly evaulation files produced by QUAST.`
PreProcessing | `Contains all FASTQ formatted trimmed sequence files produced by Trimmomatic.`

-------

Dependencies
============

Tychus utilizes a number of open-source bioinformatics tools to run. Please click on the tool names below to learn more about each tool. Keep in mind that all of these dependencies are resolved by Docker.

Software | Function
--------- | --------
[Abyss](https://github.com/bcgsc/abyss) | `Used to produce assembly contigs.`
[BCFtools](https://github.com/samtools/bcftools) | `Used to generate consensus sequences from VCF formatted SNPs.`
[Bowtie2](https://github.com/BenLangmead/bowtie2) | `Used to align short fragments of DNA to a reference genome.`
[CISA](http://sb.nhri.org.tw/CISA/en/CISA) | `Used to integrate assembly contigs into a super assembly.`
[Docker](https://www.docker.com/what-docker) | `Software containerization platform used to resolve the dependencies listed here.`
[Figtree](http://tree.bio.ed.ac.uk/software/figtree/) | `Used to create images from Newick formatted phylogenies.`
[IDBA-UD](https://github.com/loneknightpy/idba) | `Used to produce assembly contigs.`
[KmerGenie](http://kmergenie.bx.psu.edu) | `Used for optimizing chosen values of k (kmer) for non-iterative genome assemblers.`
[kSNP3](https://sourceforge.net/projects/ksnp/files/) | `Used to generate SNPs and SNP phylogenies.`
[Nextflow](https://www.nextflow.io) | `Used as the backend framework for the Tychus pipeline.`
[Prokka](https://github.com/tseemann/prokka) | `Used to identify genomic features of interest.`
[QUAST](https://github.com/ablab/quast) | `Used for the evaluation and interrogation of assembly contigs.`
[Samtools](https://github.com/samtools/samtools) | `Used for manipulating SAM/BAM formatted alignment files.`
[SPAdes](https://github.com/ablab/spades) | `Used to produce assembly contigs.`
[Trimmomatic](https://github.com/timflutre/trimmomatic) | `Used for the removal of adapter sequences and low quality base pairs.`
[Velvet](https://github.com/dzerbino/velvet) | `Used to produce assembly contigs.`

------------

Contact
=======
Questions, bugs, or feature requests should be directed to Chris Dean at cdean11 AT rams DOT colostate DOT edu. Alternatively, you can [Submit an Issue](https://github.com/cdeanj/nextflow-tychus/issues) on Github.
