---
layout: doc
tool: true

title: Session 3-Run a test single cell RNA sequencing data with nf-core/scflow
---

## Learning objectives

* **Learn how to set up your own analysis**
* **Learn more about the analysis parameter file**
* **Inspect the structures of the input files**
* **Check the additional resource requirements**
* **Submit your own nf-core/scflow job on HPC**

Now that you have (successfully) managed to run the nf-core/scflow pipeline with test dataset, it's time to set up your own analysis. Let us first inspect the outputs from the test run.


## Create an analysis directory

Create a directory, (eg. my_analysis) within your scflow_workshop2024 directory. This is your **launch direcotry**

```bash
cd ~/scflow_workshop2024
mkdir my_analysis
```

## Getting your samplesheet and manifest files ready

Just to keep the launch directory neat and tidy we will create multiple sub-directories for different types of input files required for the pipeline. **Note: Every time you `cd` into a directory remember to go back to your launch directory.** Hint: Use `pwd`, `ls` & `cd` sparingly. 

```bash
mkdir ~/scflow_workshop2024/my_analysis/refs/
cd ~/scflow_workshop2024/my_analysis/refs/
```

Download the templates into your refs/ directory:

```bash 
wget https://raw.githubusercontent.com/nf-core/test-datasets/scflow/refs/SampleSheet.tsv
wget https://raw.githubusercontent.com/nf-core/test-datasets/scflow/refs/Manifest.txt
```
There is a typo in the Manifest.txt file, spot it and correct it by opening it on VScode explorer.

We will now download the raw input matrices that contains the gene expression count. In the manifest file, you will see they direct to a zipped directories and hosted on github, however, in most cases you will have them in the matrix format. Let's first create a directory to save the input matrices.

```bash
mkdir ~/scflow_workshop2024/my_analysis/input/
```

The following codes will download the zipped files and unzip them.

```bash
while read col1 col2; do wget $col2; done <  ~/scflow_workshop2024/my_analysis/refs/Manifest.txt
```

## Understanding the structure of the manifest and samplesheets

Once the input matrices are downloaded and unzipped, the manifest file should be in the following format, edit the template (Manifest.txt) you've just downloaded so that it matches where your input files are (the header should not be changed, should be in tab separated format):

```
key filepath    
rinuv   ~/scflow_workshop2024/my_analysis/input/individual_1/    
finov   ~/scflow_workshop2024/my_analysis/input/individual_2/    
jafap   ~/scflow_workshop2024/my_analysis/input/individual_3/    
rigap   ~/scflow_workshop2024/my_analysis/input/individual_4/ 
vihig   ~/scflow_workshop2024/my_analysis/input/individual_5/ 
bamum   ~/scflow_workshop2024/my_analysis/input/individual_6/    
```

The file path directories contain the output of cellranger in the sparse matrix format, the three following files, namely:
- barcodes.tsv.gz
- features.tsv.gz
- matrix.mtx.gz

Hint: Explore using `ls input/directory/path`.

You do not need to edit the samplesheet file, but it should be in the following format:

```
sample  manifest feature_1  feature_2   feature_3 ...
Sample_XXX1 rinuv   A1
Sample_XXX2 finov   A2
Sample_XXX3 jafap   A3
Sample_XXX4 rigap   A4
```

Note that your metadata and manifest files must have the same number of rows and the sample names and manifest/key entries must match, otherwise scFlow will fail.


## Setting up config files

This section will focus on setting up the config files for your analysis (they contain all the parameters needed for the pipeline to run).

Go back to the launch directory and make a directory for additional config files.

```bash
mkdir ~/scflow_workshop2024/my_analysis/conf
```

## Parameters for indidual steps within the pipeline

Copy the contents of the template here: [scflow_analysis.config](https://github.com/combiz/nf-core-scflow/blob/dev-nf/conf/scflow_analysis.config) and paste it into a new file (you can call it scflow_analysis.config) within the directory you have just created

~/scflow_workshop2024/my_analysis/conf/scflow_analysis.config

This config file contains the parameters required for each individual step contained in the pipeline. Once again, open the file and browse the different parameters, edit to suit your future analyses.

## Hardware requirements config file

Now we will look at another config file that will indicate to Nextflow the hardware resources required for each job. Copy the contents from the template here: [base.config](https://github.com/combiz/nf-core-scflow/blob/dev-nf/conf/base.config), create and paste its contents at the following location:

~/scflow_workshop2024/my_analysis/conf/resources.config

Add the following to the resources.config file:

In this file, you can see hardware and time allocations for groups of jobs that are categorized into:
- tiny
- low
- medium
- high
- long
- high_memory

And there are already values attributed to each categories of jobs. As you can see the jobs will be retried if failed, but with more resource allocations. Keep in mind though, that the job might not fail because of hardware/time requirements so increasing them might not solve everything!


This config file is especially important as it will be what nextflow requests from PBS for each individual job (the more memory intensive/lengthy a job is the more hardware/time resources you should allocate it).

What step of the pipeline would you say is most memory intensive? If it happens to fail because of a lack of RAM which line would you edit in the config file?

## Download and organize your resources

Create a resources folder:

```bash
mkdir ~/scflow_workshop2024/my_analysis/resources
cd ~/scflow_workshop2024/my_analysis/resources
```

Download the following files using wget

```bash
wget https://raw.githubusercontent.com/nf-core/test-datasets/scflow/assets/ensembl_mappings.tsv
wget https://raw.githubusercontent.com/combiz/scFlowData/dev-nf/assets/ctd.zip
wget https://raw.githubusercontent.com/nf-core/test-datasets/scflow/refs/reddim_genes.yml
```

Feel free to open them and browse their contents

Open the following file: ~/scflow_workshop2024/my_analysis/conf/resources.config

Add the following:

```
params {
  
  //Analysis Resource Params - general
  ctd_path = "/rds/general/user/$USER/scflow_workshop2024/my_analysis/resources/ctd.zip"
  ensembl_mappings = "/rds/general/user/$USER/scflow_workshop2024/my_analysis/resources/ensembl_mappings_human.tsv"
  reddim_genes_yml = "/rds/general/user/$USER/scflow_workshop2024/my_analysis/resources/reddim_genes.yml"
  
}

workDir = "/rds/general/ephemeral/user/$USER/ephemeral/tmp"
```

## Run pipeline with your new parameters

Now you can run scFlow with an additional config file, create your job submission file and name it `run_scflow.pbs.template`. Copy and paste the following codes and save the file.

```
#!/bin/bash

#PBS -l walltime=24:00:00
#PBS -l select=1:ncpus=8:mem=24gb
#PBS -N my_analysis
#PBS -o my_analysis.out
#PBS -e my_analysis.err

cd $PBS_O_WORKDIR
module load gcc/8.2.0
export NXF_OPTS='-Xms1g -Xmx4g'

export JAVA_HOME=~/scflow_workshop2024/bin/jdk-23.0.1
export PATH=~/scflow_workshop2024/bin/jdk-23.0.1/bin/:$PATH

mkdir -p /rds/general/ephemeral/user/$USER/ephemeral/tmp/

scflow_config=~/scflow_workshop2024/my_analysis/conf/scflow_analysis.config
resource_config=####
samplesheet=###
manifest=###

~/scflow_workshop2024/bin/nextflow run combiz/nf-core-scflow \
-r dev-nf \
--input $samplesheet \
--manifest $manifest \
-profile singularity,imperial \
-c $scflow_config \
-c $resource_config
```

Submit the job to the queue:

```bash
qsub run_scflow.pbs.template
```

This might take a while, whilst it is running you can check the status of the different jobs with the command below:

```bash
qstat -a
```

As the scFlow progresses, inspect the contents on the results/ directory:

```bash
ls -l ~/scflow_workshop2024/my_analysis/results/
```

Download the QC reports and go through the different metrics

## Running the pipeline using a singularity image

In order to make your analysis reproducible we are using the scFlow singularity image which nextflow automatically pulls down and saves in a cache direcotry. You can also do this manually prior to running your analysis in order to avoid downloading the same image multiple times and save time. 

1. Create a directory to store your image:

```bash
mkdir ~/scflow_workshop2024/singularity-cache/
cd ~/scflow_workshop2024/singularity-cache/
```

2. Download the image:

```bash
singularity pull --name "nfancy-scflow-0.7.2.img" docker://nfancy/scflow:0.7.2
```

3. Then add this snippet in your resource.config file:

```
singularity {
  enabled = true
  autoMounts = true
  cacheDir = "~/scflow_workshop2024/singularity-cache/"
  runOptions = "-B /rds/,/rdsgpfs/,/rds/general/ephemeral/user/$USER/ephemeral/tmp/:/tmp,/rds/general/ephemeral/user/$USER/ephemeral/tmp/:/var/tmp"
}
```

## Additional notes

## If the pipeline fails

In case the pipeline fails in the future (it will probably not happen in this workshop); Nextflow has a "resume" flag that you can add at the end of the command. It will resume the pipeline at the step where it failed. However, if any input file (eg. Samplesheet, manifest file), excluding config files; it will restart from the beginning.

Your submission script would therefore be as follows:

```
#!/bin/bash

#PBS -l walltime=24:00:00
#PBS -l select=1:ncpus=8:mem=24gb
#PBS -N my_analysis
#PBS -o my_analysis.out
#PBS -e my_analysis.err

cd $PBS_O_WORKDIR
module load gcc/8.2.0
export NXF_OPTS='-Xms1g -Xmx4g'

export JAVA_HOME=~/scflow_workshop2024/bin/jdk-23.0.1
export PATH=~/scflow_workshop2024/bin/jdk-23.0.1/bin/:$PATH

mkdir -p /rds/general/ephemeral/user/$USER/ephemeral/tmp/

scflow_config=~/scflow_workshop2024/my_analysis/conf/scflow_analysis.config
resource_config=####
samplesheet=###
manifest=###

~/scflow_workshop2024/bin/nextflow run combiz/nf-core-scflow \
-r dev-nf \
--input $samplesheet \
--manifest $manifest \
-profile singularity,imperial \
-c $scflow_config \
-c $resource_config -resume
```

