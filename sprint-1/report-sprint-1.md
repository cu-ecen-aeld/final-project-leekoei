# Report for the final project sprint 1

## 1. Update the Buildroot target

In the main branch of my Buildroot project:

https://github.com/cu-ecen-aeld/assignment-5-leekoei

On top of the Assignment 9 final sumbission, added a commit ed0a9ba600aca3484553f529d1b66119545fe542.

The major changes:

* Updated the aesd_qemu_defconfig to change the target hardware to Rasbperry Pi 4B:
```
BR2_cortex_a72=y
BR2_ARM_FPU_VFPV4=y
BR2_PACKAGE_HOST_LINUX_HEADERS_CUSTOM_6_6=y
BR2_TOOLCHAIN_BUILDROOT_CXX=y
BR2_GLOBAL_PATCH_DIR="board/raspberrypi/patches"
BR2_ROOTFS_POST_BUILD_SCRIPT="board/raspberrypi4-64/post-build.sh"
BR2_ROOTFS_POST_IMAGE_SCRIPT="board/raspberrypi4-64/post-image.sh"
BR2_LINUX_KERNEL_DEFCONFIG="bcm2711"
BR2_LINUX_KERNEL_DTS_SUPPORT=y
BR2_LINUX_KERNEL_INTREE_DTS_NAME="broadcom/bcm2711-rpi-4-b broadcom/bcm2711-rpi-400 broadcom/bcm2711-rpi-cm4 broadcom/bcm2711-rpi-cm4s"
BR2_PACKAGE_RPI_FIRMWARE=y
BR2_PACKAGE_RPI_FIRMWARE_VARIANT_PI4=y
BR2_PACKAGE_RPI_FIRMWARE_CONFIG_FILE="board/raspberrypi4-64/config_4_64bit.txt"
```
NOTE: there are other changes to the defconfig beyond the hardware target.

* In the aesd-assignments.mk and ldd.mk files, changed the file copy LINUX_VERSION to LINUX_VERSION_PROBED:

It was found during Buildroot build that the Linux version cannot be properly fetched with LINUX_VERSION

After the update in this commit, Buildroot build successfully.

## 2. Fix the aesd driver

During the QEMU test, a regression issue was found in the aesd driver.
A commit cc7cf3c0aeb308a02510247bae6318dd001369b2 was updated for the main branch of:

https://github.com/leekoei/aesd-assignment-leekoei

This commit improves robustness of read/write behavior, especially around buffer rollover, partial user-copy results, and newline-delimited command commit semantic.

* Updated the aesd_circular_buffer_find_entry_offset_for_fpos() function to rework the traversal and skip empty entries.
* In aesd_circular_buffer_add_entry(), simplifed the overwrite hadnling when the buffer is full.
* In the main.c, improved aesd_write/aesd_read function, and made all the module functions static.

The updated aesd driver shows no issue in the next QEMU test.

## 3. QEMU test

With the clean build from step 1, the generated image ran into issue in the QEMU test.
After debugging, I decided to start another branch "final-project-qemu-test" on top of the latest main branch, just for the purpose of QEMU testing.

The update includes:
* Fine tuning of the defconfig file
* Update the S98lddmodules file to make the module probing more robust
* Updated the runqemu.sh to handle different rootfs path
* Updated the commit hash of aesd-assignment-leekoei project

With all the updates, the QEMU is working as expected. the test log of full-test.sh can be found at:

https://github.com/cu-ecen-aeld/final-project-leekoei/blob/main/sprint-1/test-log/qemu-full-test.log

## 4. Conclusion
After udpating both the aesd-assignment-leekoei and assignment-5-leekoei projects, the folloiwng goals were achieved:

* The Buildroot hardware target was updated to the Raspberry Pi 4B and a clean build was completed.
* The full-test.sh ran successfully for the QEMU.

The DoDs of Sprint 1 are completed.
