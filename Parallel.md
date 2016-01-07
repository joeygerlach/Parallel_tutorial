
This tutorial will give a very quick overview of launchin and accessing a virtual machine (VM) in NeCTAR. These processes are covered in depth on the NeCTAR website Training Modules <http://training.nectar.org.au/> , and Support Pages <https://support.nectar.org.au/support/home>

This guide will show you how to **use** your NeCTAR VM to perform jobs, including tips on making efficient use of cloud capabilities.


## Launch a Virtual Machine (Instance)

1. Log on to the NeCTAR dashboard <https://dashboard.rc.nectar.org.au/project>
1. Select the correct project allocation in the top bar (if it starts with "pt- ", it is your default trial allocation)
1. Select "**Access & Security**" under the "Compute subheading in the left side main menu.
    1. Click "Create Security Group"
    1. Name the security group 'SSH', with the description "port 22 for SSH". Click "Create Security Group"
    1. Click "**Manage Rules**" in the "Actions" drop-down menu. Click "**Add Rule**".
    1. Type "22" under "**Port**". Under "**CIDR**", you choose a range of IP adresses that can access your VM through port 22. If you keep it as "0.0.0.0/0", you can access your VM from any computer, but anyone else night find a way to access it also. Here are the IP ranges for SA University networks. Use one of these, and the VM can only be accessed form a University computer (including an external computer using a VPN to access the University network).
        - 129.127.0.0/16 - University of Adelaide
        - 129.96.0.0/16 - Flinders
        - 130.220.0.0/16 - UniSA 
    1. Select "**Key Pairs**" and use "**Import Key Pair**" to upload the public key of a keypair you have created in Putty (Windows users) or with `ssh-keygen -f ~/.ssh/keyname`. See <https://support.nectar.org.au/support/solutions/articles/6000055376-launching-virtual-machines> for instructions on setting up keypairs.
1. Select "**Instances**" in the left side main menu. Click "**Launch Instance**".
1. Give your VM a name, choose a "**Flavor**" (size of the VM), and select an Image (pre-loaded operating system, e.g. NeCTAR Ubuntu 14.04).
1. Select the "**Access & Security**" tab. Select your Key Pair name, and select the "**SSH**" security group.
1. If you are going to use a volume storage attachment, select the "**Availability Zone**" and choose "**sa**" from the drop-down menu.
1. Click "**Launch**". It may take a few minutes for the instance to boot.

## Connect to your VM

See the support page:  
<https://support.nectar.org.au/support/solutions/articles/6000055446-accessing-instances>  

1. Copy the IP Address of the instance.
  - Windows - Set up a PuTTY connection with the IP address
  - Mac/Linux - In the terminal app, enter `ssh -i <keyname> ubuntu@<IPaddress>`
1. Many users like to use FileZilla to copy data between their computer and the VM, to visualise the file structure on the VM, and to edit text documents on the VM using a GUI application.
    1. Use the IP address to create a connection with the VM in FileZilla


## Downloading and Installing

- Try some commands to look around the VM
  - `ps` ; `df -hT` ; `top`
- Connect to the VM with FileZilla to visualise the file structure and contents.  
  <https://support.nectar.org.au/support/solutions/articles/6000085114-transferring-data-to-your-vm#filezilla>
- Install "htop"
  - `apt-cache search htop`
  - `sudo apt-get install htop`
  - `htop`

---

- Run these commands periodically to update the package manager and the installed packages.
  - `sudo apt-get update`
  - `sudo apt-get upgrade`  

---

- To download "Structure", copy the URL for the latest release <http://pritchardlab.stanford.edu/structure_software/release_versions/>
  - Then use the "wget" command to download the tar.gz file. e.g.  
  `wget http://pritchardlab.stanford.edu/structure_software/release_versions/v2.3.4/release/structure_linux_console.tar.gz`

  - Decompress the archived folder with:  
  `tar -xvzf structure_linux_console.tar.gz`  

  - Download today's tutorial file:  
  `wget https://github.com/joeygerlach/structure_parallel/archive/master.zip`  
  - Use the *unzip* command to decompress the tutorial file  
  `sudo apt-get install unzip`   
  `unzip master.zip`  
  `ls`  `ls -lh structure_parallel-master`
  
  - Always perform analyses in a larger storage disc, not the primary (root) disc. In this tutorial we will use the secondary (ephemeral) disc, but it is recommended that users request volume storage to use for running analyses instead because the data will be more secure.
  - First, we make the mounted storage disc writable:  
  `sudo chown ubuntu /mnt`  
  - Copy the tutorial files to the mounted disc:  
  `cp -r ~/structure_parallel-master/ /mnt/structure_run1/`  
  `cd /mnt/structure_run1`  
  - Look at the files (you may wish to use FileZilla for this)  
  
  - Look at the support page on running jobs on a VM:  
  <https://support.nectar.org.au/support/solutions/articles/6000089713-tips-for-running-jobs-on-your-vm>
  
  - Install the task manager "GNU Parallel"  
  `sudo apt-get install parallel`
  
  - Run Structure using "GNU parallel" by calling the shell script:  
  `bash Parallel_Structure.sh`  
  
  - `htop`  

<http://pritchardlab.stanford.edu/structure_software/release_versions/v2.3.4/release/structure_linux_console.tar.gz>
https://github.com/joeygerlach/structure_parallel/archive/master.zip



Launching and Connecting tutorials  
BASH tutorial  
FileZilla tutorial  
Parallel and NoHup guide
Quick commands table


## Quick reference for shell commands

Connecting from the terminal app on your Mac:  
`ssh -i Nectar_Key ubuntu@XX.XX.XX.XX`



### Commands for your Linux VM

| Command  | Action |
| ------------- | ------------- |
| `sudo passwd ubuntu` | set a password for user 'ubuntu' |
| `sudo chown ubuntu /mnt` | make the ephemeral disk writable |
| `lsblk -l` | list the block storage |
| `df -hT` | display the disk usage |
| `du -h <path/to/directory>` | display directory and file sizes |
| `top` | activity monitor for your VM |
| `ps` | list the running processes on your VM (with PID#) |
| `kill <PID#>` | terminate the process by PID number  |
| control + 'c' | stops a process running in your terminal |
| `sudo apt-get update` | updates the list of packages available to install |
| `sudo apt-get upgrade` | upgrades the installed packages |  
| `apt-cache search <name>` | search for a package to install |
| `sudo apt-get install <name>` | install a package |
| `nohup <normal commands go here> 2>&1 &` | keep a job running in the background |
| `jobs` | list the active jobs (with job numbers) |
| control + 'z' | pause a job running in the foreground |
| `disown %n` | detach a (paused) job from the terminal session (n=job number) |
| `bg %n ` | move a (paused) job to the background (n=job number) |


[codecademy]: https://www.codecademy.com/learn/learn-the-command-line
[long]: http://cli.learncodethehardway.org/bash_cheat_sheet.pdf
[git]: https://gist.github.com/LeCoupa/122b12050f5fb267e75f
[simple]: https://ubuntudanmark.dk/filer/fwunixref.pdf

