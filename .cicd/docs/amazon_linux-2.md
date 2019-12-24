---
content_title: Amazon Linux 2
---

<!-- This document is aggregated by our internal documentation tool to generate EOSIO.CDT documentation. The code within the codeblocks below is used in our CI/CD. It will be converted line by line into statements inside of a temporary Dockerfile and used to build our docker tag for this OS. Therefore, COPY and other Dockerfile-isms are not permitted. Code changes will update hashes and regenerate new docker images, so use with caution and do not modify unless necessary. -->

This section contains shell commands to manually download, build, install, test, and uninstall EOSIO.CDT and dependencies on Amazon Linux 2.

[[info | Building EOSIO.CDT is for Advanced Developers]]
| If you are new to EOSIO.CDT, it is recommended that you install the [EOSIO.CDT Prebuilt Binaries](../../00_install-prebuilt-binaries.md) instead of building from source.

Select a manual task below, then copy/paste the shell commands to a Unix terminal to execute:

* [Download EOSIO.CDT Repository](#download-EOSIO.CDT-repository)
* [Install EOSIO.CDT Dependencies](#install-EOSIO.CDT-dependencies)
* [Build EOSIO.CDT](#build-EOSIO.CDT)
* [Install EOSIO.CDT](#install-EOSIO.CDT)
* [Test EOSIO.CDT](#test-EOSIO.CDT)
* [Uninstall EOSIO.CDT](#uninstall-EOSIO.CDT)

[[info | Building EOSIO.CDT on another OS?]]
| Visit the [Build EOSIO.CDT from Source](../index.md) section.

## Download EOSIO.CDT Repository
These commands set the EOSIO.CDT directories, install git, and clone the EOSIO.CDT repository.
<!-- DAC CLONE -->
```sh
# set EOSIO.CDT directories
export EOSIO_LOCATION=$HOME/eosio
export EOSIO_CDT_LOCATION=$EOSIO_LOCATION/cdt
export EOSIO_CDT_INSTALL_LOCATION=$EOSIO_LOCATION/install
mkdir -p $EOSIO_CDT_INSTALL_LOCATION
# install git
yum update -y && yum install -y git
# clone EOSIO.CDT repository
git clone https://github.com/eosio/eosio.cdt.git $EOSIO_CDT_LOCATION
cd $EOSIO_CDT_LOCATION && git submodule update --init --recursive
```
<!-- DAC CLONE END -->

## Install EOSIO.CDT Dependencies
These commands install the EOSIO.CDT software dependencies. Make sure to [Download the EOSIO.CDT Repository](#download-EOSIO.CDT-repository) first and set the EOSIO.CDT directories.
<!-- DAC DEPS -->
```sh
# install dependencies
yum install -y gcc.x86_64 gcc-c++.x86_64 autoconf automake libtool make bzip2 \
    bzip2-devel.x86_64 openssl-devel.x86_64 gmp-devel.x86_64 libstdc++.x86_64 \
    python.x86_64 python3-devel.x86_64 libedit-devel.x86_64 doxygen.x86_64 graphviz.x86_64 perl
PATH=$EOSIO_CDT_INSTALL_LOCATION/bin:$PATH
# build lcov
cd $EOSIO_CDT_INSTALL_LOCATION && git clone https://github.com/linux-test-project/lcov.git && \
    cd lcov && \
    make PREFIX=$EOSIO_CDT_INSTALL_LOCATION install && \
    rm -rf $EOSIO_CDT_INSTALL_LOCATION/lcov
# build cmake
cd $EOSIO_CDT_INSTALL_LOCATION && curl -LO https://cmake.org/files/v3.10/cmake-3.10.2.tar.gz && \
    tar -xzf cmake-3.10.2.tar.gz && \
    cd cmake-3.10.2 && \
    ./bootstrap --prefix=$EOSIO_CDT_INSTALL_LOCATION && \
    make -j$(nproc) && \
    make install && \
    rm -f $EOSIO_CDT_INSTALL_LOCATION/cmake-3.10.2.tar.gz
```
<!-- DAC DEPS END -->

## Build EOSIO.CDT
These commands build the EOSIO.CDT software on the specified OS. Make sure to [Install EOSIO.CDT Dependencies](#install-EOSIO.CDT-dependencies) first.
<!-- DAC BUILD -->
```sh
export EOSIO_CDT_BUILD_LOCATION=$EOSIO_CDT_LOCATION/build
mkdir -p $EOSIO_CDT_BUILD_LOCATION
cd $EOSIO_CDT_BUILD_LOCATION
$EOSIO_CDT_INSTALL_LOCATION/bin/cmake -DCMAKE_BUILD_TYPE='Release' -DCMAKE_INSTALL_PREFIX=$EOSIO_CDT_INSTALL_LOCATION ..
make -j$(nproc)
```
<!-- DAC BUILD END -->

## Install EOSIO.CDT
This command installs the EOSIO.CDT software on the specified OS. Make sure to [Build EOSIO.CDT](#build-EOSIO.CDT) first.
<!-- DAC INSTALL -->
```sh
make install
```
<!-- DAC INSTALL END -->

## Test EOSIO.CDT
These commands validate the EOSIO.CDT software installation on the specified OS. This task is optional but recommended. Make sure to [Install EOSIO.CDT](#install-EOSIO.CDT) first.
<!-- DAC TEST -->
```sh
make test
```
<!-- DAC TEST END -->

## Uninstall EOSIO.CDT
These commands uninstall the EOSIO.CDT software from the specified OS.
<!-- DAC UNINSTALL -->
```sh
awk '!seen[$0]++' $EOSIO_CDT_LOCATION/build/install_manifest.txt > $EOSIO_CDT_LOCATION/build/install_manifest.txt_deduped && rm -f $EOSIO_CDT_LOCATION/build/install_manifest.txt && mv $EOSIO_CDT_LOCATION/build/install_manifest.txt_deduped $EOSIO_CDT_LOCATION/build/install_manifest.txt
xargs rm < $EOSIO_CDT_LOCATION/build/install_manifest.txt
rm -rf $EOSIO_CDT_LOCATION/build
```
<!-- DAC UNINSTALL END -->