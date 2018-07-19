# RCRT-Linux-Preempt-RT-Patch

RCRT is an enhanced, full Preempt RT implementation for Ubuntu-based Linux systems.


*This is a kernel patch. If you don't want to compile your own kernel, install our [kernel binaries](https://launchpad.net/~rcrt-linux/circe). This patch is for manipulating Vanilla Linux kernel source version 4.16.15*

#### Extra Features
  * Ureadahead support 
  * SPL-ZFS support (working with full preempt rt enabled)
  * Aufs support
  * full, 2-stage patch and build strategy for advanced kernel build output


#### Stage 1 Patch instructions
  *Open a shell. Before you start, it's recommended that you install some support for building modules against custom kernels. To do so, run this command.
```bash
sudo apt-get install --no-install-recommends dkms
```
---
  * Install some dependencies
```bash
sudo apt-get install -y git fakeroot build-essential xz-utils libssl-dev bc kernel-package libncurses5-dev ccache wget dh-make devscripts subversion perl gawk libelf-dev bison flex qt4-qmake libqt4-dev pkg-config
```
---
  * Create and move to build folder
```bash
mkdir ~/src
cd ~/src
```
---
  * Clone RCRT git to get the patch and config
```bash
git clone https://github.com/randychrisrealtime/RCRT-Linux-Preempt-RT-Patch rcrt
```
---
  * Download the linux source, unpack the tarball, and move to the linux-4.16.8 directory.

```bash
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v4.x/linux-4.16.15.tar.gz 
tar -xzf linux-4.16.8.tar.gz
cd linux-4.16.8
```
---
  * Apply the patch.
```bash
patch -p1 -i ../rcrt/linux-4.16.15-circe-4.patch
```
---
 * if you want to change the tag at the end of the kernel. NOTE: it has to follow these two rules it starts with a '-' and has to be all lowercase. Just like it is now.
```bash
nano localversion-rc
```
---
  * Apply the config.
```bash
cp ../rcrt/x86_64RT_defconfig .config
```
---
  * Make clean, and make your menuconfig.
```bash
make clean && make mrproper
make menuconfig
```
![example kernel menuconfig screen](https://linuxhint.com/wp-content/uploads/2018/02/s7.png)
  * Edit the menuconfig settings to your liking, save, and exit.
---
  * Compile your kernel as a deb package.
```bash
make -j `getconf _NPROCESSORS_ONLN` bindeb-pkg &> ../kernelwarnings.txt
```
---
  * Go up one directory, and install your new kernel
```bash
cd ..
sudo dpkg -i *.deb
```
---
  * Download the linux-firmware package.
```bash
wget https://sourceforge.net/projects/rcrt-ubuntu-preemptrt/files/linux-firmware-1.178.deb
```
  * restart
---
  * remove all other kernel files and old firmware package.
---
  * install new firmware package
```bash
cd ~/src
sudo dpkg -i linux-firmware-1.178.deb
```
---
## Stage 2 Patch
This patch will upgrade the kernel build to a meta package model that allows you to create indivifual flavour builds. If oyu are familiar with these type of builds, patch your stage 1 patch output with the linux-4.15.16-circe-4.8.1-debuild.patch file, apply your changes, and use debuild to output the full Ubuntu linux package set.
