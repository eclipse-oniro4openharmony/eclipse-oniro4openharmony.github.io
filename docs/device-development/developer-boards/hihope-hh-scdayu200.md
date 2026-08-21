---
title: HiHope HH-SCDAYU200
parent: Developer Boards
nav_order: 1
layout: default
---

# HiHope HH-SCDAYU200 Development Kit

## Introduction
Based on the Rockchip RK3568, this development kit includes a dual-core GPU and a
high-performance NPU. Its onboard quad-core, 64-bit Cortex-A55 processor runs at
up to 2.0 GHz.

The rich set of peripherals ranges from Bluetooth and Wi-Fi to audio, video,
camera, and several Bosch Sensortec sensors.

The expansion board provides several interfaces, including video input and output
interfaces suitable for applications with sophisticated user interfaces.

It also has two adaptive Gigabit RJ45 Ethernet ports suitable for NVRs, industrial
gateways, and other products that require multiple network ports.

![HiHope HH-SCDAYU200 Development Kit](images/hihope-hh-scdayu200/hh-scdayu200.png)

## Specification

### Development Board Specification

| Category          | Specification |
|-------------------|---------------|
| **SOC Model**     | Rockchip RK3568 |
| **CPU Architecture** | Quad-core Cortex-A55 up to 2.0GHz |
| **GPU**           | Mali-G52 GPU |
| **Supported APIs** | OpenGL ES 1.1/2.0/3.2, OpenCL 2.0, Vulkan 1.1 |
| **Video Decoding** | Supports 4K at 60fps H.265/H.264/VP9 video decoding |
| **Video Encoding** | Supports 1080P at 100fps H.265/H.264 video encoding |
| **NPU Performance** | 0.8TOPs |
| **Supported Operations** | INT8, INT16, FP16 operations |
| **RAM**           | 2/4GB LPDDR4/LPDDR4x, running at 1600MHz |
| **Storage**       | 16/32 GB |
| **Power Input**   | DC 12V/2A |
| **Operating Systems** | OpenHarmony |
| **Connector Type** | SODIMM 314P (MXM 3.0) |

### Expansion Board Specification

| Category          | Specification |
|-------------------|---------------|
| **HDMI**          | 1x HDMI2.0(Type-A), supports 4K at 60fps output |
| **MIPI**          | 2x MIPI interface, supports 1920x1080 at 60fps output |
| **eDP Interface** | 1x eDP interface, supports 2K at 60fps output |
| **I2S/TDM/PDM**   | 1x 8 channel I2S/TDM/PDM |
| **Ethernet**      | 2x GMAC(10/100/1000M) |
| **SDIO**          | Supports Wi-Fi 5G/2.5G, BT4.2 |
| **Camera Interface** | MIPI-CSI2, 1x4-lane/2x2-lane at 2.5Gbps/lane |
| **USB**           | 1x USB2.0 Host, Type-A; 1x USB3.0 Host, Type-A; 1x USB3.0 OTG |
| **M.2 Interface** | 4G LTE Module |
| **PCIe**          | 1x 2 Lanes PCIe3.0 Connector (RC Mode) |
| **SATA**          | 1x SATA3.0 Connector |
| **SDMMC**         | 1x Micro SD Card3.0 |
| **Buttons**       | 1x Vol+/Recovery; 1x Reset; 1x Power; 1x Vol-; 1x Mute |
| **RTC**           | 1x RTC |
| **Infrared**      | 1x IR |
| **LEDs**          | 3x LED |
| **Sensors**       | Bosch Sensortec BMA456, BMI270 and BMP581 |
| **Fan**           | 1x Fan |

## Building

To build Eclipse Oniro for this board, follow the standard [build procedure](../building-oniro.md)
to obtain the required source code and prepare the environment.

During the build step, set the target device to **rk3568** inside the Docker instance.

```bash
./build.sh --product-name rk3568 --ccache
```

## Flashing

To begin, connect the board to your computer as outlined in [the HiHope DAYU200 documentation](https://gitee.com/hihope_iot/docs/blob/master/HiHope_DAYU200/docs/%E7%83%A7%E5%BD%95%E6%8C%87%E5%AF%BC%E6%96%87%E6%A1%A3.md). Use the USB-C and mini-USB cables included in the kit to connect to the USB 3.0 OTG port and the mini-USB DEBUG port, respectively.

Follow the appropriate procedure for either a standalone Linux system or a Windows system with built-in WSL.

- [Standalone Linux System](#in-standalone-linux-system)  
- [Windows System with built-in WSL](#windows-system-with-built-in-wsl)  

### In Standalone Linux System

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
```

After this setup, running `flash/flash.py -q` should produce the following output, indicating readiness:

```bash
maskrom
```

To enable *programming mode* on the device, perform the following steps:

 1. Press and hold `VOL/RECOVERY` then `RESET` buttons.
 2. Release `RESET` button.

Confirm the mode by running `lsusb`, which should show:

```bash
...
Bus 001 Device 070: ID 2207:350a Fuzhou Rockchip Electronics Company USB download gadget
...

$ flash/flash.py -q
loader
```

After completing these steps, flash the board:

```bash
flash/flash.py -a -i ./out/rk3568/packages/phone/images
```

#### Connecting to serial console

To read the serial output, ensure the board is correctly connected and powered on. The default baud rate for the HH-SCDAYU200 board is 1500000. You can use minicom or a similar serial terminal:

```bash
minicom -D /dev/ttyUSB0 -b 1500000
```

### Windows System with Built-in WSL
Connect the power cable to turn on the device. Run `lsusb` in **WSL**. The console should display output similar to the following:

```bash
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
```
The device is not yet attached to WSL.

Download the `flash.py` flashing tool from [Gitee](https://gitee.com/hihope_iot/docs/tree/master/HiHope_DAYU200/%E7%83%A7%E5%86%99%E5%B7%A5%E5%85%B7%E5%8F%8A%E6%8C%87%E5%8D%97/linux) by running the following commands in **WSL**:

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
sudo udevadm control --reload-rules
```

Next, use **Windows PowerShell** to attach the USB device to the WSL environment.
Run `usbipd list` and find the **BUSID** of the device named **HDC Device**.

```bash
Connected:
BUSID  VID:PID    DEVICE                                                        STATE
1-2    24ae:185a  USB Input Device                                              Not shared
1-8    174f:2435  Integrated Camera                                             Not shared
1-9    06cb:00bd  Synaptics UWP WBDI                                            Not shared
1-14   8087:0029  Intel(R) Wireless Bluetooth(R)                                Not shared
1-17   2207:5000  "HDC Device"                                                  Shared
```

In this example, the **BUSID** is `1-17`.
!!! note
    If the status of **HDC Device** is **Not shared**, run `usbipd bind --busid 1-17`. Replace `1-17` with the BUSID shown on your system.

    Run `usbipd list` again and confirm that the device status is now **Shared**.

Attach the device to WSL by running `usbipd attach --wsl --busid 1-17`. Wait for the process to finish. The output should resemble the following:

```bash
usbipd: info: Using WSL distribution 'Ubuntu' to attach; the device will be available in all WSL 2 distributions.
usbipd: info: Detected networking mode 'nat'.
usbipd: info: Using IP address 172.24.144.1 to reach the host.
```

!!! note
    If you are unsure whether the device is already attached, first run `usbipd detach --busid 1-17`.


After this setup, running `flash/flash.py -q` should produce the following output, indicating readiness:

```bash
maskrom
```

!!! note
    If the output is `none`, repeat the **detach** and **attach** procedures above. You can also inspect the device connection by running `dmesg | tail -n 50` in **WSL**.

    If the problem persists, try a different cable or USB port and keep the board's screen unlocked.




To enable *programming mode* on the device, perform the following steps:

 1. Press and hold `VOL/RECOVERY` then `RESET` buttons.
 2. Release `RESET` button.

When you run `flash/flash.py -q` again, the output will be `none` because the device's USB identity has changed.

Open **Windows PowerShell** again and run `usbipd list`. The output should resemble the following:
```bash
Connected:
BUSID  VID:PID    DEVICE                                                        STATE
1-1    24ae:185a  USB Input Device                                              Not shared
1-2    2207:350a  USB download gadget                                           Not shared
1-8    174f:2435  Integrated Camera                                             Not shared
1-9    06cb:00bd  Synaptics UWP WBDI                                            Not shared
1-14   8087:0029  Intel(R) Wireless Bluetooth(R)                                Not shared
```

Find the **BUSID** of the device named **USB download gadget**. In this example, it is `1-2`.
In a Windows PowerShell session running as administrator, bind the device with `usbipd bind --busid 1-2`.

Then attach it to **WSL** by running `usbipd attach --wsl --busid 1-2`.

Switch to **WSL** and run `flash/flash.py -q` again. An output of `loader` means that the device is now in programming mode.

After completing these steps, flash the board:

```bash
flash/flash.py -a -i ./out/rk3568/packages/phone/images
```

#### Connecting to serial console

To read the serial output, ensure the board is correctly connected and powered on. The default baud rate for the HH-SCDAYU200 board is 1500000. You can use minicom or a similar serial terminal:

```bash
minicom -D /dev/ttyUSB0 -b 1500000
```

## Reference
The original specifications and some hardware descriptions come from the Chinese
HiHope documentation published on [Gitee](https://gitee.com/hihope_iot/docs/tree/master/HiHope_DAYU200).

For more details and purchasing options, see the manufacturer's [product page](http://www.hihope.org/pro/pro1.aspx?mtt=54).
