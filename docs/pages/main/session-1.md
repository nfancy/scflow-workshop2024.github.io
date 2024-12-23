---
layout: doc
tool: true

title: Session 1-Set up the HPC environment
---

## Learning objectives

* **How to connect to HPC via VScode**
* **Interacting with HPC via VScode**

In this session we will help you setting up VScode which will make it seamless to work on scripts and run the jobs on HPC via the command line. We are assuming, you have access to HPC and have <a href="https://code.visualstudio.com/docs/setup/setup-overview">installed VScode</a> on your laptop.

## Why VScode?
VScode is a powerful and modern code editor that allows you to work on your scripts and submit jobs to the HPC directly from your laptop from within the same program. This is particularly useful as it allows you to work on your scripts in a more user-friendly environment (unless you're Eugene who prefers to code as if it is 1991) and submit jobs to the HPC without having to switch between the HPC terminal and your laptop. It also supports a vast array of community sourced extensions that can help you with your work.

## Getting started

* **Install SSH extension**: First go to the extension tab by clicking on the extensions icon in the activity bar (cubes symbol on the left) and search for remote-ssh extension provided by Microsoft and install it.
    
<div style="text-align: center; padding: 20px">
    <img src="{{"/assets/img/install_extension.png" | relative_url }}" width="500px" alt="Install SSH extension" />  
</div>

 * **Configure remote connection**: Activate the command palette in the search box by clicking "show and run commands >" (shortcut: ctrl + shift + p [or Mac equivalent]). Once activated type "remote-ssh" and select  "remote-SSH: Add new SSH host". In the command palette type `ssh username@login.hpc.ic.ac.uk` and once prompted for the password input your password. You should now be connected to your HPC home directory.

    (You can quickly reconnect to the ssh session by using the "remote-SSH: Connect to host" command in the command palette.)

<div style="text-align: center; padding: 20px">
    <img src="{{"/assets/img/activate_cmd.png" | relative_url}}" width="500px" alt="Activate CMD" />
    <img src="{{"/assets/img/add_host.png" | relative_url}}" width="500px" alt="Add Host" />
    <img src="{{"/assets/img/configure_ssh.png" | relative_url}}"  width="500px" alt="Configure SSH" />
</div>

* **Activate cmd terminal**: Go to the view tab and select terminal which will now open the terminal at the bottom of the VScode panel. You can open files into the editor using the "`code`" command in the terminal. (e.g. `code filename.txt`)

<div style="text-align: center; padding: 20px">
    <img src="{{"/assets/img/cmd_terminal.png" | relative_url}}" width="500px" alt="cmd terminal" />
</div>  

* **Using the explorer (optional)**: If you wish to use a simpler navigation you can open the explorer by clicking on the explorer icon in the activity bar and press "open folder" and then type in the top level folder you wish to access (by default this will be your home directory; it will require you to re-enter you password and re-open a terminal). You can then open files by clicking on the file name (single click to open in preview mode, double click to keep open).

    New files/folders can also be created by right clicking in the explorer and selecting "new file/folder".

<div style="text-align: center; padding: 20px">
    <img src="{{"/assets/img/open_folder_1.png" | relative_url}}" width="500px" alt="Open Folder" />
    <img src="{{"/assets/img/open_folder_2.png" | relative_url}}" width="500px" alt="Open Folder" />
</div>  

* **Finished** You should now have a working VScode environment that is connected to the HPC. You can now start working on your scripts and submit jobs to the HPC directly from your laptop (see session 2).

## Bonus tasks

If you manage to finish setting up VScode and connect to HPC, feel free to explore the environment by 

* Create a text file in your home directory (i.e. test.txt)
* Create a directory in your home directory
* List all the items in your home directory
* Learn to identify the item types (i.e. file vs directories)
* Explore the paths of your home directory

For command hints, look at the cheat sheets.