---
layout: default
title: Building Oniro
parent: Eclipse Oniro Project
---

# Building Oniro

Before beginning, ensure that [`git-lfs`](https://docs.github.com/en/repositories/working-with-files/managing-large-files/installing-git-large-file-storage) and [`repo`](https://gerrit.googlesource.com/git-repo) are installed. It is recommended to have at least 100GB of free disk space available for the full build.

## Obtaining the Source Code

To download the source code, execute the following commands in your terminal:

```bash
repo init -u https://github.com/eclipse-oniro4openharmony/manifest.git -b OpenHarmony-4.0-Release --no-repo-verify
repo sync -c
repo forall -c 'git lfs pull'
```

## Fetching Prebuilt Tools

Once you have the source code run the following script to fetch the prebuilt tools:

```bash
./build/prebuilts_download.sh
```

## Setting Up the Build Environment

For building the project, using an isolated Docker container is recommended for a clean and controlled build environment. Run the following command to start the Docker container:

```bash
docker run -it -v $(pwd):/home/openharmony swr.cn-south-1.myhuaweicloud.com/openharmony-docker/docker_oh_standard:3.2

```

## Configuring and Starting the Build

Inside the Docker instance, set the target device for the build (e.g. rk3568)
and use ccache to speed up subsequent builds:

```bash
./build.sh --product-name rk3568 --ccache
```

## Flashing the HiHope HH-SCDAYU200 Development Kit

To begin, connect the board to your computer as outlined in [the HiHope DAYU200 documentation](https://gitee.com/hihope_iot/docs/blob/master/HiHope_DAYU200/docs/%E7%83%A7%E5%BD%95%E6%8C%87%E5%AF%BC%E6%96%87%E6%A1%A3.md). Use the USB-C and mini-USB cables included in the kit to connect to the USB 3.0 OTG port and the mini-USB DEBUG port, respectively.

Power on the device by attaching the power cable. Upon successful connection, your serial console should display output similar to:

```bash
Bus 002 Device 009: ID 2207:5000 Fuzhou Rockchip Electronics Company "HDC Device"
...
Bus 001 Device 069: ID 0403:6001 Future Technology Devices International, Ltd FT232 Serial (UART) IC
```

Download the `flash.py` flashing tool from [Gitee](https://gitee.com/hihope_iot/docs/tree/master/HiHope_DAYU200/%E7%83%A7%E5%86%99%E5%B7%A5%E5%85%B7%E5%8F%8A%E6%8C%87%E5%8D%97/linux) using the following commands:

```bash
git clone https://gitee.com/hihope_iot/docs.git hihope_iot_docs
mkdir flash && cp -r hihope_iot_docs/HiHope_DAYU200/烧写工具及指南/linux/* flash/
chmod +x flash/flash.py flash/bin/flash.x86_64
```

To ensure proper device recognition, install the `udev` rule:

```bash
sudo cp flash/etc/udev/rules.d/85-rk3568.rules /etc/udev/rules.d/85-rk3568.rules
```
Then, either reload udev rules or reboot your system:

```bash
udevadm control --reload-rules
````

After this setup, running `flash/flash.py -q` should produce the following output, indicating readiness:

```bash
maskrom
```

To enable *programming mode* on the device, perform the following steps:

 1. Press nad hold `VOL/RECOVERY` then `RESET` buttons.
 2. Release `RESET` button.

Confirm the mode by running `lsusb`, which should show:

```bash
...
Bus 001 Device 070: ID 2207:350a Fuzhou Rockchip Electronics Company USB download gadget
...

$ flash/flash.py -q
loader
```

Once the above steps are completed successfully, you can proceed to flash the board:

```bash
flash/flash.py -a -i ./out/rk3568/packages/phone/images
```

## Additional Tips and Troubleshooting

### Connecting to serial console

To read the serial output, ensure the board is correctly connected and powered on. The default baud rate for the HH-SCDAYU200 board is 1500000. You can use minicom or a similar serial terminal:

```bash
minicom -D /dev/ttyUSB0 -b 1500000
```

### No HDC available in the system

If the `hdc` tool is not available on your host system, build it using the `ohos-sdk`:

```bash
./build.sh --product-name ohos-sdk --ccache
```

Find the `hdc` tool in `out/sdk/ohos-sdk/linux/toolchains`. To verify the connection with the device, run:

```bash
$ hdc list targets
150100424a544434520325874bb44900
```

For sending commands to the device:

```bash
hdc shell
```

To read hilog output:

```bash
hdc hilog
```
