---
layout: doc
tool: true

title: Session 2-Getting started with nextflow
---

## Prepare workspace

It's important to prepare your workspace before runnning any analysis. We will first create the workshop directory and install all the required softwares. 

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
nextflow info
```

## Download Java 

```bash
wget https://download.oracle.com/java/23/latest/jdk-23_linux-x64_bin.tar.gz
tar -xvz jdk-23_linux-x64_bin.tar.gz
```

Let's inspect what we have in our directories so far:

```
pwd
ls .
cd ../
pwd
ls
```

## Run "Hello World" to continue the legacy

```bash
export JAVA_HOME=~/scflow_workshop2024/bin/jdk-23
export PATH=~/scflow_workshop2024/bin/jdk-23/bin/:$PATH
./bin/nextflow run hello
```

Let's also inspect the directory:

```
pwd
ls .
```

## Submit first nextflow job on HPC

```bash
mkdir test_nextflow
cd test_nextflow
touch test.pbs.template
```

Open the file in VScode and copy-paste the following code snippets:

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

export JAVA_HOME=~/scflow_workshop2024/bin/jdk-23
export PATH=~/scflow_workshop2024/bin/jdk-23/bin/:$PATH

~/scflow_workshop2024/bin/nextflow run hello
```

Then save the file and on your terminal

```
cat test.pbs.template
qsub test.pbs.template
```