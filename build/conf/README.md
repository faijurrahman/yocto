## Quick Start

1. source poky/oe-init-build-env rpi-build
2. Add this layer to bblayers.conf and the dependencies above
3. Set MACHINE in local.conf to one of the supported boards
4. bitbake core-image-base
5. Use bmaptool to copy the generated .wic.bz2 file to the SD card
6. Boot your RPI


Build Command: bitbake rpi-test-image

Prepare SDCard: 
	cd ~/yocto/poky/build/tmp/deploy/images/raspberrypi4/
	find -name "*wic.bz2"
	untar core-image-sato-raspberrypi4-20240217215934.rootfs.wic.bz2 or rpi-test-image-raspberrypi4.tar.bz2
		Extact the xxxx.wic.bz2 file and you will get xxxx.wic file
	sudo dd if=./rpi-test-image-raspberrypi4-20240217194600.rootfs.wic of=/dev/sdf bs=1M iflag=fullblock oflag=direct conv=fsync status=progress

GPIO App: raspi-gpio: https://github.com/RPi-Distro/raspi-gpio/blob/master/raspi-gpio.c#L803
GPIO Driver: https://github.dev/eq-3/RaspberryMatic/blob/master/linux-4.1/drivers/char/broadcom/bcm2835-gpiomem.c


To Enable/Disable Different feature please check the file ~/yocto/poky/meta-raspberrypi/docs/extra-build-config.md
	1. Overwrite IMAGE_FSTYPES in local.conf
		* `IMAGE_FSTYPES = "tar.bz2 ext3.xz"`
	2. Overwrite SDIMG_ROOTFS_TYPE in local.conf
		* `SDIMG_ROOTFS_TYPE = "ext3.xz"`
	3. Enable kgdb over console support
		To add the kdbg over console (kgdboc) parameter to the kernel command line, set
		this variable in local.conf:
			ENABLE_KGDB = "1"
	4. Enable SPI bus
		When using device tree kernels, set this variable to enable the SPI bus:
			ENABLE_SPI_BUS = "1"
	5. Enable I2C
		When using device tree kernels, set this variable to enable I2C:
			ENABLE_I2C = "1"
		Furthermore, to auto-load I2C kernel modules set:
			KERNEL_MODULE_AUTOLOAD:rpi += "i2c-dev i2c-bcm2708"
	6. Enable UART
		ENABLE_UART = "1
	7. Enable CAN
		In order to use CAN with an MCP2515-based module, set the following variables:
			ENABLE_SPI_BUS = "1"
			ENABLE_CAN = "1"
		In case of dual CAN module (e.g. PiCAN2 Duo), set following variables instead:
			ENABLE_SPI_BUS = "1"
			ENABLE_DUAL_CAN = "1"
		Some modules may require setting the frequency of the crystal oscillator used on the particular board. The frequency is usually marked on the package of the crystal. By default, it is set to 16 MHz. To change that to 8 MHz, the following variable also has to be set:
			CAN_OSCILLATOR="8000000"
