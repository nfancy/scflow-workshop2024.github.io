---
layout: doc
tool: true

title: Session 1-Set up HPC environment
---

In this session we will help you setting up VScode which will make it seamless to work on scripts and run the jobs on HPC via command line. We are assuming, you have acess to HPC and have installed VScode on your laptop.


## Getting started

1. **Install SSH extension**: First go to the extension tab by clicking on the cube signs on the left and search for remote-ssh extension provided by Microsoft and install it. 
    
    <img src="{{"/assets/img/install_extension.png" | relative_url }}" width="500px" alt="Install SSH extension" />  

2. **Configure remote conncetion**: Activate the command palette in the search box by clicking "show and run commands >". Once activated type "remote-ssh" and select  "remote-SSH: Add new SSH host". In the command palette type `ssh username@login.hpc.ic.ac.uk` and once prompted for the password input your password. You should now be connected to your HPC home directory.

    <img src="{{"/assets/img/activate_cmd.png" | relative_url}}"  width="500px" alt="Activate CMD" />

    <img src="{{"/assets/img/add_host.png" | relative_url}}" width="500px" alt="Add Host" />

    <img src="{{"/assets/img/configure_ssh.png" | relative_url}}"  width="500px" alt="Configure SSH" />


3. **Activate cmd terminal**: Go to the view tab and select terminal which will now open the terminal at the bottom of the VScode panel.