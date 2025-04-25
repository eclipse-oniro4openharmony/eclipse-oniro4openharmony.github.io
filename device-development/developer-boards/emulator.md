---
title: Oniro Emulator
parent: Developer Boards
nav_order: 3
layout: default
---

# Oniro Emulator

The Oniro Emulator provides an easy and accessible way to develop and test applications or system components without the need for physical hardware.  
It is based on **QEMU**, a powerful open-source machine emulator and virtualizer.  
The emulator uses the **x86_64 architecture**, which, when run on an x86 host PC, enables faster emulation by leveraging **KVM** on Linux and **Hyper-V** on Windows for hardware-assisted virtualization.

This guide provides step-by-step instructions to **build and run the Oniro Emulator**.

<img src="images/oniro_qemu.gif" alt="Oniro Emulator" width="200"/>

### 📦 Prerequisites

Before proceeding, make sure you have followed the [Quick Build Setup](/device-development/building-oniro.html) guide to prepare your build environment.

### ⬇️ Download Oniro Source Code

Use the following commands to fetch the Oniro source:

```bash
repo init -u https://github.com/eclipse-oniro4openharmony/manifest.git -b OpenHarmony-5.0.2-Release -m oniro.xml --no-repo-verify
repo sync -c
repo forall -c 'git lfs pull'
```

### 🧰 Switch to Required Kernel Version

Replace the default kernel with the required version:

```bash
rm -rf kernel/linux/linux-6.6
git clone -b weekly_20241216 https://gitee.com/openharmony/kernel_linux_6.6.git kernel/linux/linux-6.6 --depth=1
```

### 🩹 Apply source patches

Run the patching script:

```bash
bash vendor/oniro/std_emulator/hook/hook_start.sh
```

### 🛠️ Build the images

Start the build with ccache enabled:

```bash
./build.sh --product-name std_emulator --ccache
```

### 🔄 (Optional) Revert patches

If needed, you can undo the applied patches:

```bash
bash vendor/oobemulator/std_emulator/hook/hook_end.sh
```

### 📥 Download Prebuilt Images

Alternatively, you can download the [prebuilt Oniro Emulator images](https://github.com/eclipse-oniro4openharmony/device_board_oniro/releases/latest/download/oniro_emulator.zip). 

Extract the archive and use the included run scripts as described below.

## QEMU Installation

The emulator requires **QEMU** to be installed on your system.

- Download and install QEMU from the official website:  
  [https://www.qemu.org/download/](https://www.qemu.org/download/)

Refer to the platform-specific instructions on the QEMU website for installation details.

## Running the Emulator

After building, you will find the emulator run scripts (`run.sh` for Linux, `run.bat` for Windows) in the output images directory:

```
out/std_emulator/packages/phone/images
```

These scripts launch QEMU with the correct parameters for the Oniro Emulator.

To start the emulator, use the appropriate script for your operating system:

- **Linux:**  
  ```bash
  sudo ./run.sh
  ```
  > If you encounter permission errors, ensure the script is executable:  
  > `chmod +x run.sh`

- **Windows:**  
  ```powershell
  .\run.bat
  ```
  > Run the script from a Command Prompt or PowerShell window with administrator rights if required.

> **Note for Windows Users:**  
> The emulator requires **Hyper-V** to be enabled on your system.

### Enabling Hyper-V on Windows

1. Open **Start Menu** and search for “Turn Windows features on or off”.
2. In the Windows Features dialog, check the boxes for:
   - **Hyper-V**
   - **Hyper-V Management Tools**
   - **Hyper-V Platform**
3. Click **OK** and wait for the changes to apply.

Restart your system after enabling Hyper-V.

## Connecting to the Emulator with HDC

Once the emulator is running, you can connect to it using **HDC** (the OpenHarmony Device Connector):

```bash
hdc tconn localhost:55555
```

This command connects your host to the emulator instance for debugging and file transfer.

> **Note:**  
> `hdc` is included in the OpenHarmony SDK toolchain. Ensure it is in your `PATH`.

## Reference

For additional information please refer to the [Oniro Board Support Packages repository](https://github.com/eclipse-oniro4openharmony/device_board_oniro).
