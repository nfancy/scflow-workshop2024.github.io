---
layout: doc
tool: true

title: Session 3-Run a test single cell RNA sequencing data with nf-core/scflow
---

## Setting up config file

The last command you ran in session 2 must have created two config files, open them with a text editor, it can be:
- nano (in the command line)
- using VSCode

Explore the parameters and reflect on their function, edit at your convenience

```bash
nano /rds/general/user/$username/home/.nextflow/config
nano /rds/general/user/$username/home/.nextflow/assets/combiz/nf-core-scflow/nextflow.config
```

Make a directory for additional config files.

```bash
mkdir conf
```

Copy the contents of the template here: https://github.com/nf-core/scflow/blob/c2c97284e609b116b857949efa256cccf308420b/conf/scflow_analysis.config and paste it into a new file within the directory you have just created

Once again, open the file and browse the different parameters, edit to suit your future analyses.

## Re-run pipeline with your new parameters

Now you can re-run scFlow with an additional config file, edit your job submission file as follows:

```bash
~/scflow_workshop2024/bin/nextflow run combiz/nf-core-scflow -r dev-nf -profile test,singularity,imperial -c conf/new_config.config
```

This might take a while, whilst it is running you can check the status of the different jobs with the command below:

```bash
qstat -a
```

As the scFlow progresses, inspect the contents on the results/ directory:

```bash
ll results/
```

Download on of the QC reports and go through the different metrics

## Bonus task
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

