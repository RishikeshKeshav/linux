Linux kernel
============

There are several guides for kernel developers and users. These guides can
be rendered in a number of formats, like HTML and PDF. Please read
Documentation/admin-guide/README.rst first.

In order to build the documentation, use ``make htmldocs`` or
``make pdfdocs``.  The formatted documentation can also be read online at:

    https://www.kernel.org/doc/html/latest/

There are various text files in the Documentation/ subdirectory,
several of them using the Restructured Text markup notation.

Please read the Documentation/process/changes.rst file, as it contains the
requirements for building and running the kernel, and information about
the problems which may result by upgrading your kernel.




Work Distribution:
Rishikesh Andhare
•	Creation of the eax = 0x4FFFFFFC on gcp
•	Alteration of cpuid.c for the implementation of the feature as per the specification of the assignment.
•	cpuid package installation for the nested VM
•	Verification and Validation of Results. (Collaboration with Pramatha )
Pramatha Nadig
•	Creation of eax = 0x4FFFFFFD on gcp
•	Alteration of vmx.c for implementing the feature as per the specification of the assignment.
•	Verification of Results.
•	Documentation (Collaboration with Rishikesh).
Note: Before performing the below steps, this assignment requires the configuration of the 1st assignment of Virtualization Technologies as a pre-requesits. 
Performed on Google Cloud Platform
STEPS TO BE FOLLOWED:
At first, we need to create an instance using the gcp console and run and instance using  the commands:
cloud compute instances create cmpe283assignment2 \
  --enable-nested-virtualization \
  --zone=us-central1-a \
  --min-cpu-platform="Intel Haswell"
Once this is performed, the instance will start running. Then stop the instance and navigate under ‘Disk’ section and change it to 200gb. Upon this, start the substance again So the gcloud will be configured with 200gb.
The file vmx.c and cpuid.c should be altered as per the specification required in the assignment.
•	Alter the code in the file “vmx.c” at KVM with the path /linux/arch/x86/kvm/vmx/vmx.c.
•	Alter the code in the file “cpuid.c” at KVM with the path /linux/arch/x86/kvm/cpuid.c.

In the cpuid.c file for the node eax = 0x4FFFFFFC:
•	The total number of exits should be returned in the eax.
In the cpuid.c file for the node eax = 0x4FFFFFFD:
•	The exit number provided for the count of exits in ecx. Then the output will be returned to  eax.
Upon the alterations, for the effect of the changes made we should run the commands that is shown below for the kernel.
 make -j 8 modules 
Then we should run this command.
 make  -j 8
After that, we should run the below command.
 sudo make INSTALL_MOD_STRIP=1 modules_install.
As a next step check if the kvm module is already loaded with the command:
lsmod | grep kvm
If it’s already loaded remove kvm using the below command:
 sudo rmmod kvm_intel
 sudo rmmod kvm

Verification Process:
For verifying the code, installation of the package cpuid should be performed on the nested VM.
Use the below commands:
	Sudo apt-get update
	Sudo apt-get  install cpuid

Verification of 0x4FFFFFFC:
	cpuid  -1 -l 0x4ffffffc
Verification of 0x4FFFFFFD:
	cpuid -1 -l 0x4ffffffd

Assignment 3 Questions

1. Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?
There are many exits when a VM is booting. And we noticed that the exit frequency increases. However, it alters for every boot. Upon testing we found that the total number of exits at the time of full VM boot is 860422.
  
2. Of the exit types defined in the SDM, which are the most frequent? Least?
There are many exit types which are not executed and their exit type is 0. However, of them the most frequent exit type is 48 and the least exit type is 47.
<img width="449" alt="image" src="https://user-images.githubusercontent.com/111613476/207249619-410dbe02-b373-4dcd-a52b-1cc7fecdc408.png">
<img width="494" alt="image" src="https://user-images.githubusercontent.com/111613476/207249926-ff238d0e-8c6b-4a03-8496-78b3d56dd6cb.png">
