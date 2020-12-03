# Shimmer3 Firmware Repository Fork

This repository is a fork of the original [Shimmer3 repository](https://github.com/ShimmerResearch/shimmer3). It
features bugfixes for multiple issues and pull requests in the original repository:

* [PR 6](https://github.com/ShimmerResearch/shimmer3/pull/6): Fixes capitalization of include statements on Linux
* [Issue 7](https://github.com/ShimmerResearch/shimmer3/issues/7): Race condition in Bluetooth Stack
* [Issue 8](https://github.com/ShimmerResearch/shimmer3/issues/8): InfoMem response uses uninitialized variable
* [Issue 10](https://github.com/ShimmerResearch/shimmer3/issues/10): Acknowledgment byte in unsolicited status updates

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
