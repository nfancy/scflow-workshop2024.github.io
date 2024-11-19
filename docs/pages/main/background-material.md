---
layout: doc
tool: true

title: Background and Reference Material
---

## Installing VSCode.

VSCode is a powerful programming environment for writing code and running analyses.  It can be installed on all platforms from <a href="https://code.visualstudio.com/docs/setup/setup-overview">here</a>. 

## What is a HPC?
High-performance computing (HPC) is the use of parallel processing for running advanced application programs efficiently, reliably and quickly. Typically this refers to the practice of aggregating many computers together in such a way as to provide a single facility with more computational power than one could typically get out of a laptop, desktop computer or workstation. This aggregation of computers is often referred to as a high-performance computer (often abbreviated to HPC), a supercomputer or a compute cluster (i.e a cluster of computers). It is most beneficial when a problem can be broken up into many smaller tasks which can be worked on simultaneously (i.e. in parallel) where each task is handled by an individual computer. 

When connecting to the HPC you will have access to the terminal of a "login node" which is shared between many users. This is where you will navigate the RDS, run non-resource intensive tasks and submit jobs to the "compute nodes" which are the individual computers that will run your jobs. To do so we submit a special job script to a queueing system called PBS-pro (more on this later). It is **strictly forbidden** to run resource intensive tasks on the login node as this will slow down the system for all users.

<div style="text-align: center; padding: 20px">
    <img src="{{"/assets/img/HPC.png" | relative_url}}" width="500px" alt="HPC digram" />
</div>

It is important to understand the basics of using a High Performance Computing (HPC)system. An overview of Imperial's specific HPC can be found <a href="https://imperialcollegelondon.app.box.com/s/kwjxbd5bc87w296wo0m7fdwo9jct5vvs">here</a>.

## Linux Scripting

Imperial provides a <a href="https://github.com/ImperialCollegeLondon/RCDS-comm-line">detailed introduction</a>, and various <a href="https://cheatography.com/davechild/cheat-sheets/linux-command-line/">cheat-sheets are available</a>.

## Nextflow

Nextflow is a workflow system for automating bioinformatic analyses on a HPC. You might look at a <a href="https://workflows.community/stories/2022/09/28/nextflow/">quick overview</a> or a <a href="https://workflows.community/stories/2022/09/28/nextflow/">detailed introduction</a>.