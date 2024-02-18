## Quick Start

1. source poky/oe-init-build-env rpi-build
2. Add this layer to bblayers.conf and the dependencies above
3. Set MACHINE in local.conf to one of the supported boards
4. bitbake core-image-base
5. Use bmaptool to copy the generated .wic.bz2 file to the SD card
6. Boot your RPI


Build Command: bitbake rpi-test-image 

Prepare SDCard: sudo dd if=./rpi-test-image-raspberrypi4-20240217194600.rootfs.wic of=/dev/sdf bs=1M iflag=fullblock oflag=direct conv=fsync status=progress

GPIO App: raspi-gpio: https://github.com/RPi-Distro/raspi-gpio/blob/master/raspi-gpio.c#L803
GPIO Driver: https://github.dev/eq-3/RaspberryMatic/blob/master/linux-4.1/drivers/char/broadcom/bcm2835-gpiomem.c
