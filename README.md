# CSCI-3753-Operating-Systems-Programming-Assignment-One-solution

Download Here: [CSCI 3753: Operating Systems Programming Assignment One solution](https://jarviscodinghub.com/assignment/csci-3753-operating-systems-programming-assignment-one-solution/)

For Custom/Original Work email jarviscodinghub@gmail.com/whatsapp +1(541)423-7793

Welcome to the first programming assignment for CSCI 3753 – Operating Systems. In this assignment we
will go over how to compile and install a modern Linux kernel, as well as how to add a custom system
call. Most importantly you will gain skills and set up an environment that you will use in future
assignments.
You will need the following software:
1. CSCI 3753 Fall 2016 Virtual Machine
All other software and files should be installed on the virtual machine image. It is important that you do
not update the virtual machine image as all the assignments are designed and written using the software
versions that the VM is distributed with. Also make note that this assignment will require you to recompile
the kernel at least twice, and you can expect each compilation to take at least half an hour and up
to 5 hours on older machines.
After installing virtualbox, run sudo apt-get install cu-cs-csci-3753 to install all the necessary packages
required for the course.
Assignment Steps
1. Configure the Grub
2. Compile the kernel
3. Add a system call
4. Write a test program that uses the system call
Configuring the Grub
Grub is the boot loader installed with Ubuntu 14.04. It provides configuration options to boot from a list
of different kernels available on the machine. By default Ubuntu 14.04 suppresses much of the boot
process from users; as a result, we will need to update our Grub configuration to allow booting from
multiple kernel versions and to recover from a corrupt installation. Perform the following:
Figure 1: Linux Grub Boot Menu
Step 1:
From the command line, load the grub configuration file:
sudo emacs /etc/default/grub
(feel free to replace emacs with the editor of your choice).
Step 2:
Make the following changes to the configuration file:
1. Comment out:
GRUB_HIDDEN_TIMEOUT=0
2. Comment out:
GRUB_HIDDEN_TIMEOUT_QUIET=true
3. Comment out:
GRUB_CMDLINE_LINUX_DEFAULT=”quiet splash”
and add the following new line directly below it:
GRUB_CMDLINE_LINUX_DEFAULT=””
4. Save updates
Step 3:
From the command line, update Grub: sudo update-grub.
Step 4:
Reboot your virtual machine and verify you see a boot menu as shown in Figure 1.
Downloading Linux Source Code
First, you have to download the linux source code that you will be using for this assignment where you
will be adding your new system call, compiling the source and then running the kernel with the added
system call. For this execute the following commands in the terminal.
1. cd /home
2. sudo mkdir kernel
3. cd kernel
4. sudo apt-get source linux-image-$(uname -r)
5. sudo apt-get install cu-cs-csci-3753 (command to install all the packages necessary for this
course)
The command uname –r gives you the current version of the kernel you are using. So that means you
are downloading the same kernel as your current kernel. The command uname –a will give you the
system architecture information (for example if your OS is 32 or 64 bit).
Compiling the Kernel
Step 1:
Typically it takes a long time to compile (2-3) hours to compile the kernel source code. To get
around this, install ccache by running the following command
sudo apt-get install ccache
ccache is a compiler cache. It is used to speed up recompilation by caching previous compilations
and detecting when the same compilation is being done again. The first compilation will take the
usual long time but after you make the necessary changes, the recompilations will be much faster
after you install ccache.
Step 2:
Now you have to change permission of the debian scripts required to compile the source code.
Just run the following commands in the linux source code directory.
sudo chmod a+x debian/scripts/*
sudo chmod a+x debian/scripts/misc/*
or alternatively you can execute all the commands using sudo.
Step 3:
The next step is to create the config file. sudo make localmodconfig command will create the
config file. Run the command in the linux source directory you downloaded and extracted.
Step 4:
In the same folder, execute the command sudo make menuconfig . A pop up will be generated.
Select the general set up (Figure 2) and then select the option append to the release (Figure 3)
and enter the name you want to give to the kernel you will be compiling ,installing and running
(for example revision1). This name will appear when you reboot and from the grub you can
select it and check your new system call.
Figure 2: Configuring the menuconfig
Figure 3: Configuring the menuconfig
Step 5:
Now just run the following commands.
sudo make -j2 CC=”ccache gcc”
sudo make -j2 modules_install
sudo make -j2 install
sudo reboot
When the grub option shows up, select the version you have installed with the new name
appended.
Adding System Call
Step 1:
Now you have to write the actual system call. First you will see that the linux source code is
downloaded in the kernel folder. Go to that folder (for example linux-3.13.0) from the terminal.
Then follow these steps.
1. sudo gedit arch/x86/kernel/helloworld.c and copy paste the following code snippet and
save it
2.
#include
