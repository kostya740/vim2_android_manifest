title: Download the Android Source Code
---

The Android source tree of Khadas VIMs are hosted on [Github](https://www.github.com/khadas) with many different repositories.

Walk through the below steps to download the source code. 

### Steps
1) Create an empty directory to hold your working files:
```sh
$ mkdir -p WORKING_DIRECTORY
$ cd WORKING_DIRECTORY
```

2) Run `repo init` to bring down the manifest repository first:
For Android Marshmallow(6.0):
```sh
$ repo init -u https://github.com/khadas/android_manifest.git -b Mmallow
```
*Note: Only available for VIM1.*

For Android Nougat(7.1):
```sh
$ repo init -u https://github.com/khadas/android_manifest.git -b Nougat
```
*Note: Available for both VIM1 & VIM2.*

For Android Oreo(8.0):
```sh
$ repo init -u https://github.com/khadas/android_manifest.git -b Oreo
```
*Note: Only available for VIM1.*

3) Run `repo sync` to pull down the Android source tree:
```sh
$ repo sync -j4
```
The initial sync operation will take an hour or more to complete.

*Tips: you might need to run above command repeatly if it failed halfway. Or you can try with below script instead:*
```sh
#!/bin/bash
repo sync -j4
while [ $? = 1 ]; do
	echo "Sync failed, repeat again:"
	repo sync -j4
done
```
*If needed, press Ctrl-\ to quit.*

4) Begins a new branch for development:
```sh
$ repo start <BRANCH_NAME> --all
```

### Further Reading
* [Android official documents](https://source.android.com/source/downloading.html)
* [Build Android Source Code](/vim1/BuildAndroid.html)

title: Install Extra Toolchains for Amlogic Platform
---

Amlogic platform requires extra toolchains for cross-compiling, you might need to follow the below steps to do the setup.

### Install Cross Compiler for U-Boot BL301:
```sh
$ sudo apt-get install gcc-arm-none-eabi
```

*Note:*
* BL301 is an ARM Cortex-M3 based coprocessor, and is integrated into the Amlogic SoCs.
* We have verified that 'gcc-4.8.2 on Ubuntu 14.04' and 'gcc-4.9.3 on Ubuntu 16.04', both work fine on VIM1 platform.
* You might wanna have a try with other gcc versions, it may take a risk.

### Install Cross Compiler for Linux kernel:
```sh
$ wget http://releases.linaro.org/archive/14.09/components/toolchain/binaries/gcc-linaro-aarch64-linux-gnu-4.9-2014.09_linux.tar.bz2
$ sudo mkdir /opt/toolchains
$ sudo tar -xjf gcc-linaro-aarch64-linux-gnu-4.9-2014.09_linux.tar.bz2 -C /opt/toolchains
```

*Tips: you might need to replace a new address of gcc-linaro-aarch64-linux-gnu-4.9-2014.09_linux.tar.bz2 if the link address above was changed.*

### Further readings:
[Amlogic Openlinux Website](http://openlinux.amlogic.com/)

title: Building Android Source Code
---


### Building
*Note: Before you start to build, make sure you have done all the `Preperations` listed above.*

**Build U-Boot:**
```sh
$ cd PATH_YOUR_PROJECT
$ cd uboot
$ make CROSS_COMPILE=aarch64-linux-gnu- kvim2_defconfig
$ make CROSS_COMPILE=aarch64-linux-gnu-
```
*Gernerated images in this step:*

* fip/u-boot.bin: for onboard EMMC storage booting
* fip/u-boot.bin.sd.bin: for external TF card booting


**Build Android:**
```sh
$ cd PATH_YOUR_PROJECT
$ source build/envsetup.sh
$ lunch kvim2-userdebug-64
$ make -jN otapackage
```
*Note:*

* Replace 'N' as the number you want when you run 'make -jN'

*Gernerated images in this step:*

* out/target/product/kvim2/update.img


**Build Linux kernel:**

When you build Android aboved, will build Linux kernel at the same time.

In some case, you might want to build Linux kernel separately, you can run the script below to do that:
```sh
$ source device/khadas/kvim2/mkern.sh
```

### See Also
* [Upgrade Via an USB Cable](/vim2/UpgradeViaUSBCable.html)
* [Upgrade Via a Burnning TF Card](/vim2/UpgradeViaTFBurningCard.html)
