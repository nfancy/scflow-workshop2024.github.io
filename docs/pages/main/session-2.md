---
layout: doc
tool: true

title: Session 2-Getting started with Nextflow
---

## Prepare workspace

We need to install Nextflow and prepare your workspace before runnning any analyses. We will first create the workshop directory and install all the required software. 

```bash
mkdir -p scflow_workshop2024
cd scflow_workshop2024
mkdir -p bin
cd bin
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

## Download Java 

We need to install Java:

```bash
wget https://download.oracle.com/java/23/latest/jdk-23_linux-x64_bin.tar.gz
tar -xvzf jdk-23_linux-x64_bin.tar.gz
rm jdk-23_linux-x64_bin.tar.gz
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

```bash
export JAVA_HOME=~/scflow_workshop2024/bin/jdk-23.0.1
export PATH=~/scflow_workshop2024/bin/jdk-23.0.1/bin/:$PATH
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

## Understanding the PBS parameters

While waiting for the test job to complete, we can discuss the queuing system (PBS) in more detail. As you may be aware it is **STRICTLY FORBIDDEN** to run long/resource intensive commands on the login node of the HPC (you use will use this node mostly for pwd, cd, ls, echo ... which cannot be considered resource intensive)

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

There are a lot of parameters for PBS that you can explore here: ( https://albertsk.org/wp-content/uploads/2011/12/pbs.pdf )

## Outputs of a queue submission

Once qstat indicates the job run above has finished you can view the Nextflow output:

```
ls
cat hello.out

```