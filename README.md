## Assignment 2 docs
### Google Cloud Set-up
1. Login to Google Cloud Console.
1. Create a VM with enabled nested virtualization
  * Open Gcloud Terminal
  * run `gcloud compute images list --show-deprecate` that will output all possible images 
  * To create a custom image choose the OS from above and run `gcloud compute images create nested-vm-anastasia-2004-focal --source-image=ubuntu-2004-focal-v20201028 --source-image-project=ubuntu-os-cloud --boot-disk-size=200GB --licenses="https://www.googleapis.com/compute/v1/projects/vm-options/global/licenses/enable-vmx"`
  * To create a VM from that image run `gcloud compute instances create nested-vm-ubuntu --zone us-central1-b --image nested-vm-anastasia-2004-focal`
  * After creation it will appear in the list of your VMs. Stop it.
  * Update its settings: more CPUs and add extra disk.
  * Connect to it using ssh
  * Check nested virtualization is enabled: `cat /proc/cpuinfo | grep vmx`
1. Mount additional disk to the VM (int the vm terminal) (optional):  
  * run `sudo lsblk` it'll show info about available disk and the one you need to mount
  * format that disk: `sudo mkfs.ext4 -m 0 -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/sdb`
  * create a directory to serve as a mount point for the new disk `sudo mkdir -p /mnt/disks/dir`
  * mount the disk `sudo mount -o discard,defaults /dev/sdb /mnt/disks/dir`
  * add read and write permissions `sudo chmod a+w /mnt/disks/dir`
  * create a backup of the fstab file `sudo cp /etc/fstab /etc/fstab.backup`
  * check the uuid for the disk running this command: `sudo blkid /dev/sdb`
  * add info about the dist with terminal command \
  `echo UUID=`sudo blkid -s UUID -o value /dev/sdb` /mnt/disks/dir ext4 discard,defaults,nofail 0 2 | sudo tee -a /etc/fstab`
  * than check that it's mounted `cat /etc/fstab`
1. Generate and add ssh key for your github account
  * `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`
  * `eval "$(ssh-agent -s)"`
  * `ssh-add ~/.ssh/id_rsa`
  * `cat < ~/.ssh/id_rsa.pub` and copy ssh-key and then add it your github account (Settings => SSH and GPG keys => New SSH key)
 1. Clone previously forked linux repository: `git clone git@github.com:YOUR_NAME/linux.git`
 1. Build the Kernel (in the linux folder!)
  * `sudo bash`
  * `apt-get install build-essential fakeroot libncurses5-dev libssl-dev ccache bison flex libelf-dev dwarves`
  * check your kernel version with `uname -a`
  * make a copy of config to the current directory (should be the one created by git clone) `cp /boot/config-YOUR_KERNEL_VERSION` ./.config
  * run `make oldconfig` (press Enter for all questions)
  * build it `make && make modules && make install && make modules_install`
  * reboot
 1. Verify that the kernel is updated: `uname -a`
 
 ### Research
 - The main goal of the assigment was to calculate exits from the VM and time spent processing them in the VMM. \
 To calculate exits we need to add data structure to hold exits to vmx.c , update it everytime when the exit occurs (vmx_handle_exit function) and then put in into %eax register while emulating cpuid (kvm_emulate_cpuid function in cpuid.c )
  
  
  
  Resources:
  - [How to enable nested virtualization of Google Cloud](https://www.cloudkb.net/how-to-enable-nested-virtualization-on-google-cloud/)
  - [Adding or resizing zonal persistent disks](https://cloud.google.com/compute/docs/disks/add-persistent-disk)
  - [Atomic variables in C](https://www.kernel.org/doc/Documentation/atomic_t.txt)
