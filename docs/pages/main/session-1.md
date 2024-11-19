---
layout: doc
tool: true

title: Session 1-Set up HPC environment
---

In this session we will help you setting up VScode which will make it seamless to work on scripts and run the jobs on HPC via command line. We are assuming, you have acess to HPC and have installed VScode on your laptop.

If you do not have VScode installed, you can download it from [https://code.visualstudio.com/Download](https://code.visualstudio.com/Download).

## What is a HPC? ##
High-performance computing (HPC) is the use of parallel processing for running advanced application programs efficiently, reliably and quickly. Typically this refers to the practice of aggregating many computers together in such a way as to provide a single facility with more computational power than one could typically get out of a laptop, desktop computer or workstation. This aggregation of computers is often referred to as a high-performance computer (often abbreviated to HPC), a supercomputer or a compute cluster (i.e a cluster of computers). It is most benficial when a problem can be broken up into many smaller taskes which can be worked on simultanously (i.e. in parallel) where each task is handled by an individual computer. 

When connecting to the HPC you will have access to the terminal of a "login node" which is shared between many users. This is where you will navigate the RDS, run non-resource intensive tasks and submit jobs to the "compute nodes" which are the individual computers that will run your jobs. To do so we submit a special job script to a queueing system called PBS-pro (more on this later). It is **strictly forbidden** to run resource intensive tasks on the login node as this will slow down the system for all users.

    <img src="{{"/assets/img/HPC.png" | relative_url}}" width="500px" alt="HPC digram" />

## Getting started

1. **Install SSH extension**: First go to the extension tab by clicking on the extensions icon in the activity bar (cubes symbol on the left) and search for remote-ssh extension provided by Microsoft and install it.
    
    <img src="{{"/assets/img/install_extension.png" | relative_url }}" width="500px" alt="Install SSH extension" />  

2. **Configure remote connection**: Activate the command palette in the search box by clicking "show and run commands >" (ctrl + shift + p). Once activated type "remote-ssh" and select  "remote-SSH: Add new SSH host". In the command palette type `ssh username@login.hpc.ic.ac.uk` and once prompted for the password input your password. You should now be connected to your HPC home directory.

(You can quickly reconnect to the ssh session by using the "remote-SSH: Connect to host" command in the command palette.)

    <img src="{{"/assets/img/activate_cmd.png" | relative_url}}"  width="500px" alt="Activate CMD" />

    <img src="{{"/assets/img/add_host.png" | relative_url}}" width="500px" alt="Add Host" />

    <img src="{{"/assets/img/configure_ssh.png" | relative_url}}"  width="500px" alt="Configure SSH" />

3. **Activate cmd terminal**: Go to the view tab and select terminal which will now open the terminal at the bottom of the VScode panel. You can open files using the "`code`" command in the terminal. (e.g. `code filename.txt`)

4. **Using the exporer**: If you wish to use it you can open the explorer by clicking on the explorer icon in the activity bar and press "open folder" and then type in the top level folder you wish to access (by default this will be your home directory; it will require you to re-enter you password). You can then open files by clicking on the file name (single click to open in preview mode, double click to keep open).

New files/folders can also be created by right clicking in the explorer and selecting "new file/folder".

    <img src="{{"/assets/img/open_folder_1.png" | relative_url}}" width="500px" alt="Open Folder" />

    <img src="{{"/assets/img/open_folder_2.png" | relative_url}}" width="500px" alt="Open Folder" />





