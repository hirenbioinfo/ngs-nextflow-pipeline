# NGG-nextflow-pipeline
A collection of modular and customizable Nextflow pipelines for Next-Generation Sequencing (NGS) data analysis, enabling to process and analyze NGS data from illumina and ONT plotform with scalability and reproducibility.

## Introduction

Nextflow simplifies the creation, management, and execution of complex bioinformatics workflows, providing a scalable and reproducible framework for NGS data analysis. Thes pipeline was adapted and modified from several published pipeline e.g. from [[NF Core Github page](https://github.com/nf-core/modules/blob/master/modules/nf-core/)]

 ## Installing Nextflow with Conda

1. **Prerequisites:**
   - Make sure you have Conda installed on your system. If you don't have Conda installed, you can download and install Miniconda or Anaconda. Visit the [Conda website](https://docs.conda.io/en/latest/miniconda.html) for installation instructions.

2. **Create a Conda Environment:**
   - Open a terminal or command prompt.
   - Run the following command to create a new Conda environment for Nextflow:
     ```
     conda create -n nextflow_env
     ```
   - Activate the newly created environment:
     - On Linux/macOS:
       ```
       conda activate nextflow_env
       ```
     - [On Windows](https://www.nextflow.io/blog/2021/setup-nextflow-on-windows.html)
       ```
       activate nextflow_env
       ```

3. **Install Nextflow with Conda:**
   - Visit the [netflow conda instalation website](https://anaconda.org/intake/intake-netflow)
   
   - While inside the activated `nextflow_env` environment, run the following command to install Nextflow using Conda:
     ```
     conda install -c bioconda nextflow
     ```

4. **Verify Nextflow Installation:**
   - Run the following command to check if Nextflow is installed correctly:
     ```
     nextflow -version
     ```
   - If the installation was successful, you should see the Nextflow version information printed in the console.

Congratulations! You have successfully installed Nextflow using Conda. You can now create and execute Nextflow pipelines for NGS data analysis.

Please note that using Conda allows for managing dependencies and creating isolated environments for Nextflow, ensuring smooth execution of workflows.

For more information on how to use Nextflow and create workflows, you can refer to the [Nextflow documentation](https://www.nextflow.io/docs.html).

 

## Sample command
An example of a command to run this pipeline is:

```
nextflow run main.nf --reads "Sample_files/*.fastq.gz" --output_dir "test2"
```

# Installation
The only dependancy is Python3. Assuming you have python 3.3+ and pip installed, just run:
```
pip3 install krocus
```

or if you wish to install the latest development version:
```
pip3 install git+git://github.com/andrewjpage/krocus.git
```

On some systems pip3 may be just called pip.

## Debian/Ubuntu (Trusty/Xenial)
To install Python3 on Ubuntu, as root run:
```
apt-get update -qq
apt-get install -y git python3 python3-setuptools python3-biopython python3-pip
pip3 install krocus
```

## Conda
First install Conda (Python3), then run:
```
conda install  krocus
```

## Windows
Like virtually all Bioinformatics software, this software is unlikely to work on Windows. Try using a Linux virtual machine.

# Usage
## krocus_database_downloader script
First of all you need MLST databases. There is a snapshot bundled with this repository for your convenience, or alternatively you can use the downloader script to get the latest data. You will need internet access for this step.

```
usage: krocus_database_downloader [options]

Download

optional arguments:
  -h, --help            show this help message and exit
  --list_species, -l    List all available species (default: False)
  --species SPECIES, -s SPECIES
                        Species to download (default: None)
  --output_directory OUTPUT_DIRECTORY, -o OUTPUT_DIRECTORY
                        Output directory (default: mlst_files)
  --verbose, -v         Turn on debugging (default: False)
  --version             show program's version number and exit

```
First of all you can get a list of available databases by running:
```
krocus_database_downloader -l
```

From this list choose one of the species and use it for the next step:
```
krocus_database_downloader  --species "Salmonella enterica" --output_directory Salmonella_enterica
```
You will now have a directory called __Salmonella_enterica___ which can be provided to the main script.

## krocus script
This is the main script of the application. The manditory inputs are a directory containing an MLST database (from the previous step), and a FASTQ file, which can be optionally gzipped.
```
usage: krocus [options] allele_directory input.fastq

multi-locus sequence typing (MLST) from uncorrected long reads

positional arguments:
  allele_directory      Allele directory
  input_fastq           Input FASTQ file (optionally gzipped)

optional arguments:
  -h, --help            show this help message and exit
  --filtered_reads_file FILTERED_READS_FILE, -f FILTERED_READS_FILE
                        Filename to save matching reads to (default: None)
  --output_file OUTPUT_FILE, -o OUTPUT_FILE
                        Output file [STDOUT] (default: None)
  --max_gap MAX_GAP     Maximum gap for blocks to be contigous, measured in
                        multiples of the k-mer size (default: 4)
  --margin MARGIN       Flanking region around a block to use for mapping
                        (default: 100)
  --min_block_size MIN_BLOCK_SIZE
                        Minimum block size in bases (default: 150)
  --min_fasta_hits MIN_FASTA_HITS, -m MIN_FASTA_HITS
                        Minimum No. of kmers matching a read (default: 10)
  --print_interval PRINT_INTERVAL, -p PRINT_INTERVAL
                        Print ST every this number of reads (default: 200)
  --kmer KMER, -k KMER  k-mer size (default: 11)
  --target_st TARGET_ST
                        For performance testing print time to find given ST
                        (default: None)
  --verbose, -v         Turn on debugging [0]
  --version             show program's version number and exit
```

### Required
__allele_directory__: The directory containing the MLST database you wish to query against. This is generated by the krocus_database_downloader script and just contains copies of the allele sequences in FASTA format and the profile.txt file linking allele numbers to STs.

__input_fastq__: This is a single FASTQ file. It can be optionally gzipped. Alternatively input can be read from stdin by using the dash character (-) as the input file name.

### Options
__kmer__:  The most important parameter. Long reads have a high error rate, so if you set this too high, nothing will match (because it will contain errors). If you set it too low, everything will match, which isnt much use to you. Thinking about your data, on average how long of a stretch of bases can you get in your read without errors? This is what you should set your kmer to. For example, if you have an average of 1 error every 10 bases, then the ideal kmer would be 9.

__min_fasta_hits__: This is the minimum number of matching kmers in a read, for the read to be considered for analysis. It is a hard minimum threshold which is really there to speed things up. If you set this too high, then nothing will be returned.

__filtered_reads_file__: If you provide a filename for this option, all of the reads which are estimated to match one of the MLST genes are saved to a file. Only the region predicted to contain the MLST gene is saved. This can be used for downstream analysis, such as de novo assembly. This file should not already exist. 

__output_file__: By default the predicted sequence types are printed to screen (STDOUT). If a filename is provided, the predicted sequence types are instead printed to this file.  This file should not already exist. 

__print_interval__: Print out the predicted sequence type every X number of reads. This is where you are performing analysis in real time and want a quick result.

# Streaming data in real-time
If you wish to analyse data in real-time as its produced (for example as it is sequenced from a minION) you can pipe the data in, using the dash as the input file name. This is an example command:
```
tail -f reads.fastq | krocus dir -
```
If you are impatient you can get it to print results at shorter intervals by setting the parameter '-p 10'.


# Output
The output format is: the predicted sequence type (ST) number (column 1), the percentage k-mer coverage of the alleles (0-100) (column 2), the specific alleles identified. For each allele the name of the gene is noted and the allele number, which corresponds to a unique sequence, is given in brackets. If there is only a partial match a star (*) is appended.

```
323	97.23	infB(1)	pgi(1)	phoE(9)*	tonB(93)	rpoB(1)*	gapA(2)	mdh(1)
```
In the above example the sequence type is 323,and 97.23% of k-mers making up 323 are covered by reads. 2 of the alleles have k-mers which were not identified in the reads, possibly due to errors in the reads encountered or no reads covering that region were found. 



# Resource usage
For an 550Mbyte FASTQ file (unzipped) of long reads from a Pacbio RSII containing Salmonella required 550MB of RAM.


