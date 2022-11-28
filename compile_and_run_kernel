#!/bin/bash

BUILTROOT_PATH=/home/lda/Workspace/buildroot-2022.02.6/
ROOTFS_OVERLAY_PATH=/home/lda/Workspace/root_qemu/root/

# build and send the .ko file to overlay rootfs folder
MODULE_PATH=/home/lda/Workspace/ldd3/Chapter$1
if [ ! `find $MODULE_PATH -name '*.ko'` ];
then 
  cd $MODULE_PATH
  make
  cd -
else
  cd $MODULE_PATH
  make clean
  make
  cd -
fi
KOFILE_PATH=$(find $MODULE_PATH -name '*.ko')
cp $KOFILE_PATH $ROOTFS_OVERLAY_PATH

# go to buildroot directory and re-make(to update overlay rootfs folder)
cd $BUILTROOT_PATH && make

# finally, boot the kernel...
cd -
qemu-system-x86_64 -kernel arch/x86/boot/bzImage -boot c -m 2049M \
 -hda $BUILTROOT_PATH/output/images/rootfs.ext4 \
 -append "root=/dev/sda rw console=ttyS0,115200 acpi=off nokaslr" -serial stdio -display none
