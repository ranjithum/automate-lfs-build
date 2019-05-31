# automate-lfs-build
set of script which can be used to automate LFS build.

### Requirements

* apt install build-essential bison texinfo gawk git python2.7 kpartx -y
* passes version-check.sh

### scripts

Scripts must be executed in the same order as shown below

* version-check.sh
    - This should be first script which should be executed, make sure there are no errors before proceding further.

* setup_lfs_user
    - This script is responsible for creating lfs user and setting up bash for lfs.

* build_toolchain
    - This script is responsible for setting up toolchain, must be executed with lfs user permission.

* build_packages
    - This script is responsible for building minimum linux with busybox

* create_image
    - This is optional script for creating an ISO image.


### Notes

* scripts has been tested only on Ubuntu 18.04.1 LTS

* iso image has been tested with qemu using below command
qemu-system-x86_64 -drive format=raw,$LFS/iso/lfs.iso -nographic -enable-kvm -m 512M

* Below procedure should work seemlessly
mkdir -p $LFS/lfs-source; wget --input-file=$LFS/lfs-packages.txt --continue --directory-prefix=$LFS/lfs-source
mkdir -p $LFS/automate-lfs-build
git clone https://github.com/ranjithum/automate-lfs-build $LFS/automate-lfs-build
cd $LFS/automate-lfs-build/
./version-check.sh
./setup_lfs_user
su - lfs
cd automate-lfs-build/
./build_toolchain
exit
./build_packages
./create_image
