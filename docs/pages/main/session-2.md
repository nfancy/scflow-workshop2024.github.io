---
layout: doc
tool: true

title: Session 2-Getting started with Nextflow
---

## Learning objectives

* **Prepare workspace in the HPC home directory**
* **Run test workflow using nextflow**
* **Learn to submit job on HPC queue**
* **Submit your first nf-core/scflow job on HPC**


## The most important thing to remember

Every time you `cd` into a directory remember to go back to your workshop directory. Hint: Use `pwd`, `ls` & `cd` sparingly. Once a wise person told me "If you are lost, go home", so, if you are unsure where you are, go to your home directory by `cd ~`. The directory structure is very important for all our future tasks, otherwise the pipeline won't be able to locate the necessary files. One other important thing is to use the absolute paths for any file in the script. In the context of the HPC the absolute path starts from `~` which is equivalent to `/rds/general/user/$USER/home`.

## Prepare workspace

You need to install Nextflow and prepare your workspace before runnning any analyses. We will first create the workshop directory and install all the required softwares. 

```bash
mkdir -p scflow_workshop2024
cd scflow_workshop2024
mkdir -p bin
cd bin
```

## Download Java 

Nextflow depends on Java. Although our HPC has Java modules or one can install it in the conda environment, the most straight forward way is to download the binaries and export the path to your environment. The downside is if you forget to export the path, you will get an error saying Java is required and not available. Download Java following the codes below:

```bash
wget https://download.oracle.com/java/23/latest/jdk-23_linux-x64_bin.tar.gz
tar -xvzf jdk-23_linux-x64_bin.tar.gz
rm jdk-23_linux-x64_bin.tar.gz 

export JAVA_HOME=~/scflow_workshop2024/bin/jdk-23.0.1
export PATH=~/scflow_workshop2024/bin/jdk-23.0.1/bin/:$PATH
```

## Install nextflow

The latest version of nextflow executable can be installed from [nextflow](https://www.nextflow.io/docs/latest/install.html) website.

```bash
curl -s https://get.nextflow.io | bash
```

Use the following code to make nextflow executable

```bash
chmod +x nextflow
```

Let's inspect what we have in our directories so far:

```
pwd
ls
cd ../
pwd
ls
```

## Run "Hello World" Nextflow pipeline to continue the legacy

It's a crime not to run "Hello world" when using a new programming language (I am not joking!).

```bash
./bin/nextflow run hello
```

Let's also inspect the directory:

```
pwd
ls
```

Working files produced during the pipeline processing are put into  directories inside "work". 

## Submit first nextflow job on HPC

Next we will submit a nextflow job to the cluster, using the working directory test_nextflow:

```bash
mkdir test_nextflow
cd test_nextflow
touch test.pbs.template
```

Open the file test.pbs.template in VScode and copy-paste the following code snippet:

```
#!/bin/bash

#PBS -l walltime=01:00:00
#PBS -l select=1:ncpus=2:mem=8gb
#PBS -N test_nextflow
#PBS -o hello.out
#PBS -e hello.err

cd $PBS_O_WORKDIR
module load gcc/8.2.0
export NXF_OPTS='-Xms1g -Xmx4g'

export JAVA_HOME=~/scflow_workshop2024/bin/jdk-23.0.1
export PATH=~/scflow_workshop2024/bin/jdk-23.0.1/bin/:$PATH

~/scflow_workshop2024/bin/nextflow run hello
```

Then save the file and on your terminal and submit it to the queue:

```
cat test.pbs.template
qsub test.pbs.template
```
This job has now been submitted to the HCP queue.  To check the progress of the job on the queue you can type:

```
qstat
```
## Outputs of a queue submission

Once qstat indicates the job run above has finished you can view the Nextflow output:

```
ls -la
cat hello.out

```

Additionally, you can check the log file created by nextflow which has more detailed information of the run by **cat .nextflow.log**. You can see the file starts with a dot meaning it's a hidden file and can't be seen using `ls` alone. If you want to cancel any run at any point, you can do so by 

```
qdel jobid
```
To run all submitted jobs use `qselect -u $USER | xargs qdel`


## Understanding the PBS parameters

While waiting for the test job to complete, we can discuss the queuing system (PBS) in more detail. As you may be aware it is **STRICTLY FORBIDDEN** [try running a big job if you don't mind receiving an email 😉 ] to run long/resource intensive commands on the login node of the HPC (you use will use this node mostly for pwd, cd, ls, echo ... which cannot be considered resource intensive)

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
- **walltime**: the amount of time that you request for the job you want to run
- **select**: the amount of nodes you require (will always be 1)
- **ncpus**: the amount of CPUs you require
- **mem**: the amount of RAM 
- **N**: is the name of your job
- **o**: will be the path to your output log of your job (whatever your command(s) print on the terminal will appear here)
- **e**: the path to your error log

There are a lot of parameters for PBS that you can explore [here]( https://albertsk.org/wp-content/uploads/2011/12/pbs.pdf )

## Submit first nf-core/scflow job on HPC

Let us submit the first nf-core/scflow job on HPC with the test profile before we break for lunch. 

```bash
cd ~/scflow_workshop2024
mkdir test_scflow
cd test_scflow
touch test_scflow.pbs.template
```
Open the test_scflow.pbs.template file in VScode and copy-paste the following code snippets:

```
#!/bin/bash
#PBS -l walltime=24:00:00
#PBS -l select=1:ncpus=2:mem=24gb
#PBS -N test_scflow
#PBS -o test_scflow.out
#PBS -e test_scflow.err

cd $PBS_O_WORKDIR
module load gcc/8.2.0
export NXF_OPTS='-Xms1g -Xmx4g'

export JAVA_HOME=~/scflow_workshop2024/bin/jdk-23.0.1
export PATH=~/scflow_workshop2024/bin/jdk-23.0.1/bin/:$PATH

mkdir -p /rds/general/ephemeral/user/$USER/ephemeral/tmp/

~/scflow_workshop2024/bin/nextflow run combiz/nf-core-scflow -r dev-nf -profile test,singularity,imperial
```

Then save the file and on your terminal run the following:

```
cat test_scflow.pbs.template
qsub test_scflow.pbs.template
```