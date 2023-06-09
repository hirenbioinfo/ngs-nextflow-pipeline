# NGG-nextflow-pipeline
A collection of modular and customizable Nextflow pipelines for Next-Generation Sequencing (NGS) data analysis, enabling to process and analyze NGS data from illumina and ONT plotform with scalability and reproducibility.

## Introduction

Nextflow simplifies the creation, management, and execution of complex bioinformatics workflows, providing a scalable and reproducible framework for NGS data analysis. Thes pipeline was adapted and modified from several published pipeline e.g. from [NF Core Github page](https://github.com/nf-core/modules/blob/master/modules/nf-core/). A nice Nextflow tutorial can be found here [Nextflow-Tutorial](https://training.seqera.io/).

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
   - [On Linux/macOS](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)
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
     
     nextflow self-update
     ```
   - If the installation was successful, you should see the Nextflow version information printed in the console.

Congratulations! You have successfully installed Nextflow using Conda. You can now create and execute Nextflow pipelines for NGS data analysis. Please note that using Conda allows for managing dependencies and creating isolated environments for Nextflow, ensuring smooth execution of workflows.
For more information on how to use Nextflow and create workflows, you can refer to the [Nextflow documentation](https://www.nextflow.io/docs.html).

 
## Sample command
An example of a command to run this pipeline is:

```
nextflow run main.nf --reads "Sample_files/*.fastq.gz" --output_dir "test2"
```



