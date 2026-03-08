# Report for the final project sprint 2

## 1. Build the Linux image for Raspberry Pi 4B

In repository https://github.com/cu-ecen-aeld/assignment-5-leekoei, make sure the latest main branch commit ed0a9ba600aca3484553f529d1b66119545fe542 is checked out.

As a result, the base_external/configs/aesd_qemu_defconfig is configured to use Raspberry Pi 4B as target hardware. 

In order to generate an SD card image for Raspberry Pi 4B, needs to use "make menuconfig" to add the following setting:

```
BR2_ROOTFS_POST_SCRIPT_ARGS="-c board/raspberrypi4-64/genimage.cfg
```

Run the Buildroot build from the root of the local source tree:

```
$ ./build.sh
```
The SD card image for the Raspberry PI 4B hardware is generated and located at:

```
buildroot/output/images/sdcard.img
```

## 2. Test the Buildroot Linux image on Raspberry Pi 4B

Flash the sdcard.img on an micro SD card.

