# Shimmer3 Firmware Repository Fork

This repository is a fork of the original [Shimmer3 repository](https://github.com/ShimmerResearch/shimmer3). It
features bugfixes for multiple issues and pull requests in the original repository:

* [PR 6](https://github.com/ShimmerResearch/shimmer3/pull/6): Fixes capitalization of include statements on Linux
* [Issue 7](https://github.com/ShimmerResearch/shimmer3/issues/7): Race condition in Bluetooth Stack
* [Issue 8](https://github.com/ShimmerResearch/shimmer3/issues/8): InfoMem response uses uninitialized variable
* [Issue 10](https://github.com/ShimmerResearch/shimmer3/issues/10): Acknowledgment byte in unsolicited status updates

# Building and Flashing the Firmware

The [Shimmer3 User Manual](http://www.shimmersensing.com/images/uploads/docs/Shimmer_User_Manual_rev3p.pdf) provides
some basic information on how to build and flash the firmware under Linux. In the following, we aim to provide a more
detailed description of how to perform these steps. However, at the moment, this manual only allows to compile the
*LogAndStream* firmware since the SDLog firmware requires a different compiler version.

**DISCLAIMER**: You use this repository and the following guide at your own risk. The information provided here is
based on our own experience by filling in the gaps of the original documentation.

## Building the LogAndStream Firmware

In order to build the firmware, the correct version of the
[TI Code Composer Studio (CCS)](https://software-dl.ti.com/ccs/esd/documents/ccs_downloads.html) is required. We use
version 7 of the CCS. This version ships with the correct compiler version of the LogAndStream firmware. The easiest
way to ensure a smooth installation is to use a virtual machine. In it, the CCS is installed, and the firmware is built.

### Build the virtual machine

* Install VirtualBox or another virtualization tool
* Create a virtual machine with Ubuntu 18.04 or download a prebuilt image
* Install the following packages as requirement for the CCS:
```shell
sudo apt install libgconf-2-4 libc6:i386 libusb-0.1-4 build-essential
```

### Download the CCS

* Download version 7 of the CCS
  from <https://software-dl.ti.com/ccs/esd/CCSv7/CCS_7_4_0/exports/CCS7.4.0.00015_linux-x64.tar.gz>
* Extract the archive and run the installer to start the installation GUI:
```shell
tar -xf CCS7.4.0.00015_linux-x64.tar.gz
cd CCS7.4.0.00015_linux-x64
./ccs_setup_linux64_7.4.0.00015.bin
```

### Complete the CCS installation
In the installation GUI:

* Select the `MSP430 ultra-low power MCUs` when asked what Processor Support to enable,
* Acknowledge the unsupported boards in the next step.
* Leave the debug probes settings the way they are
* Wait until the installation has completed

### Open the CCS and build the firmware

* Clone the firmware repository

```shell
git clone https://github.com/seemoo-lab/shimmer3.git
```
* Start the Code Composer Studio
* Click `File -> Import`
* Select `Code Composer Studio -> CCS Projects` from the list and hit `Next`
* Select the firmware root folder as `search directory`
* In the projects selection field, select the `LogAndStream` firmware and select `Finish`

The IDE should then index the project. Once indexing is complete, you can start the building process by clicking the
hammer at the top. The compiled binary should be located in `<Build Configuration>/<Project Name>.txt`. For the
*LogAndStream* firmware with *Debug* configuration, this would be: `Debug/LogAndStream.txt`. The firmware is in
TIText format and has the `.txt` extension.

## Flashing the firmware to the Shimmer

The firmware is flashed using the *python-msp430-tools* toolset.
Since the original repository on Launchpad is no longer maintained, we provide a
[forked version](https://github.com/seemoo-lab/python-msp430-tools) that has the necessary patches applied
(see the corresponding bug report on Launchpad [here](https://bugs.launchpad.net/python-msp430-tools/+bug/1258574)).

* Create a virtualenv with Python2.7
```shell
virtualenv -p /usr/bin/python2.7 PyEnv
source PyEnv/bin/activate
```

* Clone the repository
```shell
git clone https://github.com/seemoo-lab/python-msp430-tools
cd python-msp430-tools
```

* Build and install the tools
```shell
python setup.py install
```

* Retrieve the `LogAndStream.txt` firmware file from the virtual machine and flash it to the device
```shell
python2 -m msp430.bsl5.uart --invert-test --invert-reset -p <bl_device_path> -r -e -i titext -P LogAndStream.txt
```
where `bl_device_path` is the path to the UART device file of the Shimmer bootloader. If you have no custom udev
ruleset installed and only a single Shimmer device connected to the host, this should be `/dev/ttyUSB0`. The
programming will take several minutes. Afterwards, remove the Shimmer from the dock and power cycle the device.

***

# Note to customers
For all customers with queries related to this repository, please contact Shimmer through our [technical support page](http://www.shimmersensing.com/support/wearable-sensing-support/) available on our website. All Issues raised in this public repository will be redirected to our support page.

# FW_Shimmer3
compiler information:
 - workbench: TI CCS v7.2.0.00013
 - compiler version: TI v4.4.8
 - output format:
   - eabi (ELF) for most fw images
   - legacy COFF for SDLog
