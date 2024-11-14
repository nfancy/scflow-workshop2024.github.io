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

Once again, open the file and browse the different parameters.

Now you can re-run scFlow with an additional config file, edit your job submission file as follows:

```
~/scflow_workshop2024/bin/nextflow run combiz/nf-core-scflow -r dev-nf -profile test,singularity,imperial -c conf/new_config.config
```