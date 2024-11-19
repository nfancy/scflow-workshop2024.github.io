---
layout: doc
tool: true

title: Session 1-Set up the HPC environment
---

In this session we will help you setting up VScode which will make it seamless to work on scripts and run the jobs on HPC via the command line. We are assuming, you have acess to HPC and have <a href="https://code.visualstudio.com/docs/setup/setup-overview">installed VScode</a> on your laptop.

## What is a HPC?
High-performance computing (HPC) is the use of parallel processing for running advanced application programs efficiently, reliably and quickly. Typically this refers to the practice of aggregating many computers together in such a way as to provide a single facility with more computational power than one could typically get out of a laptop, desktop computer or workstation. This aggregation of computers is often referred to as a high-performance computer (often abbreviated to HPC), a supercomputer or a compute cluster (i.e a cluster of computers). It is most beneficial when a problem can be broken up into many smaller tasks which can be worked on simultaneously (i.e. in parallel) where each task is handled by an individual computer. 

When connecting to the HPC you will have access to the terminal of a "login node" which is shared between many users. This is where you will navigate the RDS, run non-resource intensive tasks and submit jobs to the "compute nodes" which are the individual computers that will run your jobs. To do so we submit a special job script to a queueing system called PBS-pro (more on this later). It is **strictly forbidden** to run resource intensive tasks on the login node as this will slow down the system for all users.

<div style="text-align: center; padding: 20px">
    <img src="{{"/assets/img/HPC.png" | relative_url}}" width="500px" alt="HPC digram" />
</div>

## Why VScode?
VScode is a powerful and modern code editor that allows you to work on your scripts and submit jobs to the HPC directly from your laptop from within the same program. This is particularly useful as it allows you to work on your scripts in a more user-friendly environment (unless you're Eugene who prefers to code as if it is 1991) and submit jobs to the HPC without having to switch between the HPC terminal and your laptop. It also supports a vast array of community sourced extensions that can help you with your work.

## Getting started

1. **Install SSH extension**: First go to the extension tab by clicking on the extensions icon in the activity bar (cubes symbol on the left) and search for remote-ssh extension provided by Microsoft and install it.
    
<div style="text-align: center; padding: 20px">
    <img src="{{"/assets/img/install_extension.png" | relative_url }}" width="500px" alt="Install SSH extension" />  
</div>

2. **Configure remote connection**: Activate the command palette in the search box by clicking "show and run commands >" (shortcut: ctrl + shift + p [or Mac equivalent]). Once activated type "remote-ssh" and select  "remote-SSH: Add new SSH host". In the command palette type `ssh username@login.hpc.ic.ac.uk` and once prompted for the password input your password. You should now be connected to your HPC home directory.

    (You can quickly reconnect to the ssh session by using the "remote-SSH: Connect to host" command in the command palette.)

<div style="text-align: center; padding: 20px">
    <img src="{{"/assets/img/activate_cmd.png" | relative_url}}" width="500px" alt="Activate CMD" />
    <img src="{{"/assets/img/add_host.png" | relative_url}}" width="500px" alt="Add Host" />
    <img src="{{"/assets/img/configure_ssh.png" | relative_url}}"  width="500px" alt="Configure SSH" />
</div>

3. **Activate cmd terminal**: Go to the view tab and select terminal which will now open the terminal at the bottom of the VScode panel. You can open files into the editor using the "`code`" command in the terminal. (e.g. `code filename.txt`)

4. **Using the explorer (optional)**: If you wish to use a simpler navigation you can open the explorer by clicking on the explorer icon in the activity bar and press "open folder" and then type in the top level folder you wish to access (by default this will be your home directory; it will require you to re-enter you password and re-open a terminal). You can then open files by clicking on the file name (single click to open in preview mode, double click to keep open).

    New files/folders can also be created by right clicking in the explorer and selecting "new file/folder".

<div style="text-align: center; padding: 20px">
    <img src="{{"/assets/img/open_folder_1.png" | relative_url}}" width="500px" alt="Open Folder" />
    <img src="{{"/assets/img/open_folder_2.png" | relative_url}}" width="500px" alt="Open Folder" />
</div>  

5. **Finished** You should now have a working VScode environment that is connected to the HPC. You can now start working on your scripts and submit jobs to the HPC directly from your laptop (see session 2).