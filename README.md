# rcrt
full preempt rt patch for Ubuntu.

Full Preempt RT patch with Ureadahead Tracer patch, SPL-ZFS (working with full preempt rt enable) patch, and Aufs.

Patch for Vanilla kernel version 4.16.8

sudo apt-get install -y git fakeroot build-essential xz-utils libssl-dev bc kernel-package libncurses5-dev ccache wget dh-make devscripts subversion perl gawk libelf-dev bison flex qt4-qmake libqt4-dev pkg-config

mkdir ~/src
cd ~/src

#Note: put patch and config in this directory

wget https://mirrors.edge.kernel.org/pub/linux/kernel/v4.x/linux-4.16.8.tar.gz 
tar -xzf linux-4.16.8.tar.gz
cd linux-4.16.8
patch -p1 -i ../patch-4.16.8-circe.patch
cp ../x86_64RT_defconfig .config
make clean
make menuconfig

#note: edit settings to your liking

make -j `getconf _NPROCESSORS_ONLN` bindeb-pkg &> ../kernelwarnings.txt
cd ..
sudo dpkg -i *.deb

Download firmware package.
restart. remove all other kernel files and old firmware package.
sudo dpkg -i linux-firmware-1.178.deb
