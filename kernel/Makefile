# Makefile for kernel modules and device tree script
# This file is a part of the BeagleLogic project

# Path to kernel sources relative to current directory
KSRC ?= /lib/modules/$(shell uname -r)/build

# When running under sudo, the PWD env variable is not set.
PWD ?= $(shell pwd)

# Module targets (run from host)
obj-m := beaglelogic.o

all: modules overlay

install: modules_install deploy_overlay

modules:
	@make -C $(KSRC) M=$(PWD) ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- modules

modules_install:
	@make -C $(KSRC) M=$(PWD) ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- modules_install
	@depmod -a

clean:
	@make -C $(KSRC) M=$(PWD) ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- clean
	@rm -f beaglelogic-00A0.dtbo

# Compile and install device tree
overlay: beaglelogic-00A0.dtbo

beaglelogic-00A0.dtbo: beaglelogic-00A0.dts
	dtc -O dtb -o beaglelogic-00A0.dtbo -b 0 -@ beaglelogic-00A0.dts

deploy_overlay:
	cp -v beaglelogic-00A0.dtbo /lib/firmware
