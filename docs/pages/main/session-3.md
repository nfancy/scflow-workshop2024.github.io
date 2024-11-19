---
layout: doc
tool: true

title: Session 3-Run a test single cell RNA sequencing data with nf-core/scflow
---

First things first, at any time during this session you can look at this page for guidance:

[nf-co.re](https://nf-co.re/scflow/dev/)

## Understanding the PBS parameters

As you may be aware it is **STRICTLY FORBIDDEN** to run long/resource intensive commands on the login node of the HPC (you use will use this node mostly for pwd, cd, ls, echo ... which cannot be considered resource intensive)

The advantage of a HPC is that you have a whole range of very powerful nodes connected to the login node that you can request for resource intensive activities. On the RCS at Imperial we use PBS to request the latter. As you have seen from the previous session, your submission script contains the following:

```
#!/bin/bash

#PBS -l walltime=01:00:00
#PBS -l select=1:ncpus=2:mem=8gb
#PBS -N test_nextflow
#PBS -o hello.out
#PBS -e hello.err
```

That is the information you give to PBS for the command you want to run, the each mean the following:
- walltime: the amount of time that you request for the job you want to run
- select: the amount of nodes you require (will always be 1)
- ncpus: the amount of CPUs you require
- mem: the amount of RAM 
- N: is the name of your job
- o: will be the path to your output log of your job (whatever your command(s) print on the terminal will appear here)
- e: the path to your error log

There are a lot of parameters for PBS that you can explore here: [PBS Manual](https://albertsk.org/wp-content/uploads/2011/12/pbs.pdf)

## Create an analysis directory

Create a directory, (eg. my_analysis) within your scflow_workshop2024 directory

```bash
cd scflow_workshop2024
mkdir my_analysis
```

## Understanding the inputs required for scFlow

Download the following into your analysis directory:

```bash 
wget https://raw.githubusercontent.com/nf-core/test-datasets/scflow/refs/SampleSheet.tsv
wget https://raw.githubusercontent.com/nf-core/test-datasets/scflow/refs/Manifest.txt
```

Look at the contents, what do you think each row refers to? What is the link between the two files?

## Setting up config files

Make a directory for additional config files.

```bash
mkdir ~/scflow_workshop2024/my_analysis/conf
```

## Parameters for indidual steps within the pipeline

Copy the contents of the template here: [scflow_analysis.config](https://github.com/combiz/nf-core-scflow/blob/dev/conf/scflow_analysis.config) and paste it into a new file (you can call it scflow_analysis.config) within the directory you have just created

~/scflow_workshop2024/my_analysis/conf/scflow_analysis.config

This config file contains the parameters required for each individual step contained in the pipeline. Once again, open the file and browse the different parameters, edit to suit your future analyses.

## Hardware requirments config file

In this file, you can see hardware and time allocations for groups of jobs that are categorized into:
- tiny
- low
- medium
- high
- long
- high_memory

And there are already values attributed to each categories of jobs. As you can see the jobs will be retried if failed, but with more resource allocations. Keep in mind though, that the job might not fail because of hardware/time requirements so increasing them might not solve everything!

Now we will look at another config file that will indicate to Nextflow the hardware resources required for each job. Copy the contents from the template here: [base.config](https://github.com/combiz/nf-core-scflow/blob/dev/conf/base.config)

Open a new file in the following location:
~/scflow_workshop2024/my_analysis/conf/resources.config

This config file is especially important as it will be what nextflow requests from PBS for each individual job (the more memory intensive/lengthy a job is the more hardware/time resources you should allocate it).

Add the following to the resources.config file:

```
params {

  // Resources
  max_memory = 640.GB
  max_cpus = 40
  max_time = 24.h

}

```

What step of the pipeline would you say is most memory intensive? If it happens to fail because of a lack of RAM which line would you edit in the config file?

## Download and organize your resources

Create a resources folder:

```bash
mkdir ~/scflow_workshop2024/my_analysis/resources
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

  // Resources
  max_memory = 640.GB
  max_cpus = 40
  max_time = 24.h
  
  //Analysis Resource Params - general
  ctd_path = "~/scflow_workshop2024/my_analysis//ctd.zip"
  ensembl_mappings = "~/scflow_workshop2024/my_analysis/ensembl_mappings_human.tsv"
  reddim_genes_yml = "~/scflow_workshop2024/my_analysis/reddim_genes.yml"
  
}
```

## Re-run pipeline with your new parameters

Now you can re-run scFlow with an additional config file, edit your job submission file as follows:

```
#!/bin/bash

#PBS -l walltime=03:00:00
#PBS -l select=1:ncpus=8:mem=8gb
#PBS -N my_analysis
#PBS -o my_analysis.out
#PBS -e my_analysis.err

cd $PBS_O_WORKDIR
module load gcc/8.2.0
export NXF_OPTS='-Xms1g -Xmx4g'

export JAVA_HOME=~/scflow_workshop2024/bin/jdk-23.0.1
export PATH=~/scflow_workshop2024/bin/jdk-23.0.1/bin/:$PATH

scflow_config=~/scflow_workshop2024/my_analysis/conf/scflow_analysis.config
resource_config=####

~/scflow_workshop2024/bin/nextflow run combiz/nf-core-scflow \
-r dev-nf \
-c $scflow_config \
-c $resource_config
```

This might take a while, whilst it is running you can check the status of the different jobs with the command below:

```bash
qstat -a
```

As the scFlow progresses, inspect the contents on the results/ directory:

```bash
ll ~/scflow_workshop2024/my_analysis/results/
```

Download on of the QC reports and go through the different metrics

## Bonus task

## Running the pipeline using a singularity image

In order to make your analysis more reproducible you can use a singularity image for scFlow, it can be done in the following way:

1. Create a directory to store your image:

```bash
mkdir ~/singularity-cache/
cd ~/singularity-cache/
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
  cacheDir = "~/singularity-cache/"
  runOptions = "-B /rds/,/rdsgpfs/,/rds/general/ephemeral/user/$USER/ephemeral/tmp/:/tmp,/rds/general/ephemeral/user/$USER/ephemeral/tmp/:/var/tmp"
  
}

workDir = "/rds/general/ephemeral/user/$USER/ephemeral/tmp"
```

## Generating your own input files

If you have some time, try and generate a simple manifest file, that will be required when you use the pipeline with your own dataset (without the "test" profile). It is recommended to use R or python for that task.

If you don't have a particular dataset in mind, generate a mock one.

A manifest file should be in the following format (header should not be changed, should be in tab separated format):

```
key filepath    sample
ajhxf   /rds/general/project/$your_project/.../cellbender_feature_bc_matrix/    Sample_XXX1
bhjfv   /rds/general/project/$your_project/.../cellbender_feature_bc_matrix/    Sample_XXX2
kjngh   /rds/general/project/$your_project/.../cellbender_feature_bc_matrix/    Sample_XXX3
lopmn   /rds/general/project/$your_project/.../cellbender_feature_bc_matrix/    Sample_XXX4
```

The file path directories need to contain the output of cellbender, the three following files, namely:
- barcodes.tsv.gz
- features.tsv.gz
- matrix.mtx.gz

Finally, you need an additional metadata file, which can contain as many column as you wish but have to include a sample and manifest column:

```
sample  manifest feature_1  feature_2   feature_3 ...
Sample_XXX1 ajhxf   A1
Sample_XXX2 bhjvf   A2
Sample_XXX3 kjngh   A3
Sample_XXX4 lopmn   A4
```

Note that your metadata and manifest files must have the same number of rows and the sample names and manifest/key entries must match, otherwise the scFlow will fail.

## Additional notes

## If the pipeline fails

In case the pipeline fails in the future (it will probably not happen in this workshop); Nextflow has a "resume" flag that you can add at the end of the command. It will resume the pipeline at the step where it failed. However, if any input file (eg. Samplesheet, manifest file), excluding config files; it will restart from the beginning.

Your submission script would therefore be as follows:

```
#!/bin/bash

#PBS -l walltime=03:00:00
#PBS -l select=1:ncpus=8:mem=8gb
#PBS -N my_analysis
#PBS -o my_analysis.out
#PBS -e my_analysis.err

cd $PBS_O_WORKDIR
module load gcc/8.2.0
export NXF_OPTS='-Xms1g -Xmx4g'

export JAVA_HOME=~/scflow_workshop2024/bin/jdk-23.0.1
export PATH=~/scflow_workshop2024/bin/jdk-23.0.1/bin/:$PATH

scflow_config=~/scflow_workshop2024/my_analysis/conf/scflow_analysis.config
$samplesheet=###
$manifest=###
$celltype_mappings=###

~/scflow_workshop2024/bin/nextflow run combiz/nf-core-scflow \
-r dev-nf \
-c $scflow_config \
--input $samplesheet \
--manifest $manifest \
--celltype_mappings $celltype_mappings \
--resume
```

