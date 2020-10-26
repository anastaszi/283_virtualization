1. [Download](https://store-us.vmware.com/fusion_buy_dual_new?utm_source=DWA_google&utm_medium=Banner&utm_campaign=DesktopLaunch_2020_US&utm_content=Fusion&gclid=Cj0KCQjwxNT8BRD9ARIsAJ8S5xY7fVYj2akBCXzLqPIl6bivFVuo_tqZRbuNg62zqF_WFIu5blW_lDoaAgDVEALw_wcB) and install VMWare Fusion 12 Player (free for Personal use)
2. [Download](https://ubuntu.com/download/desktop) Ubuntu image (20.1 Desktop)
3. Install a new VM in VMWare Fusion using an image from step 2
4. Shutdown newly install VM and update its settings:
![Settings](https://github.com/anastaszi/283_virtualization/blob/master/assignment1/imgs/settings.png)
* Step 1: Allow nested virtualization and increase the number of CPUs
![PM](https://github.com/anastaszi/283_virtualization/blob/master/assignment1/imgs/pm.png)
* Step 2: Increase the virtual disk volume
![Volume](https://github.com/anastaszi/283_virtualization/blob/master/assignment1/imgs/disk.png)
5. Start VM
In VM’s terminal check if it can manage virtualization \
Run `cat /proc/cpuinfo | more`
![VMX](https://github.com/anastaszi/283_virtualization/blob/master/assignment1/imgs/vmx.png)
6. Intall modules: make, gcc
```
sudo apt install gcc
sudo apt install make
```
7. Create a directory and copy there 2 files (Makefile and cmpe283-1.c) or git clone the repository and go the cloned directory
8. Compile and then execute kernel module
```
make all
sudo insmod ./cmpe283-1.ko
```
9. Check the result by running `dmesg`
Sample output:
![Pinbased](https://github.com/anastaszi/283_virtualization/blob/master/assignment1/imgs/pinbased.png)
![Secondary](https://github.com/anastaszi/283_virtualization/blob/master/assignment1/imgs/secondary.png)
![Entry_exit](https://github.com/anastaszi/283_virtualization/blob/master/assignment1/imgs/entry_exit.png)
