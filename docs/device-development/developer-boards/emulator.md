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

<img src="../images/oniro_qemu.gif" alt="Oniro Emulator" width="200"/>

## Fetch and Build

Before proceeding, ensure your build environment is ready and the Oniro source code is available by following the [Quick Build Setup](../building-oniro.md) guide (make sure you are using the `OpenHarmony-5.1.0-Release` branch). Proceed with the following steps for additional environment setup and build the system image for the emulator.

### Apply source patches

Run the patching script:

```bash
bash vendor/oniro/std_emulator/hook/do_patch.sh
```

### Build the images

Start the build with ccache enabled:

```bash
./build.sh --product-name std_emulator --ccache --gn-args allow_sanitize_debug=true
```

### (Optional) Revert patches

If needed, you can undo the applied patches:

```bash
bash vendor/oniro/std_emulator/hook/undo_patch.sh
```

## Alternative: Download Prebuilt Images

Instead of building the images yourself, you can download the [prebuilt Oniro Emulator images](https://github.com/eclipse-oniro4openharmony/device_board_oniro/releases/latest/download/oniro_emulator.zip).

After downloading, extract the archive and use the included run scripts as described in the next section.

## QEMU Installation

The emulator requires **QEMU** to be installed on your system.

- Download and install QEMU from the official website:  
  [https://www.qemu.org/download/](https://www.qemu.org/download/)

### Linux

- **On Debian-based Linux distributions:**  
  Install QEMU using the following command:
  ```bash
  sudo apt install qemu-system-x86
  ```

### Windows

- Download and install QEMU from the official website.
- After installing QEMU, add the QEMU installation directory (e.g., `C:\Program Files\qemu`) to your `PATH` environment variable to ensure the QEMU executables are accessible from the command line.
- The emulator requires **Hyper-V** to be enabled on your system:

  1. Open **Start Menu** and search for “Turn Windows features on or off”.
  2. In the Windows Features dialog, check the boxes for:
     - **Hyper-V**
     - **Hyper-V Management Tools**
     - **Hyper-V Platform**
  3. Click **OK** and wait for the changes to apply.
  4. Restart your system after enabling Hyper-V.

Refer to the platform-specific instructions on the QEMU website for further installation details.

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

## Connecting to the Emulator with HDC

Once the emulator is running, you can connect to it using **HDC** (the OpenHarmony Device Connector):

```bash
hdc tconn 127.0.0.1:55555
```

This command connects your host to the emulator instance for debugging and file transfer.

!!! note
    `hdc` is included in the OpenHarmony SDK toolchain. Ensure it is in your `PATH`.

## Reference

For additional information please refer to the [Oniro Board Support Packages repository](https://github.com/eclipse-oniro4openharmony/device_board_oniro).
