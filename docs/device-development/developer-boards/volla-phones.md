---
title: Volla Phone X23 & Plinius
parent: Developer Boards
nav_order: 4
layout: default
---

# Volla Phone X23 & Plinius

## Introduction

Oniro runs **natively** on two MediaTek-based Volla phones, the **Volla Phone X23** and the **Volla Phone Plinius**. The phone boots straight into Oniro.

Both phones use the same build target, `hybris_generic`, and share one set of system images. Oniro detects the phone at boot, so there is no build-time switch between devices. Only the kernel and boot images are built per device.

Oniro reuses the phone's existing Android hardware drivers (Halium) through **libhybris**. That is how the phones get graphics, audio and modem support.

## Specification

|                     | Volla Phone X23                 | Volla Phone Plinius                   |
|---------------------|---------------------------------|---------------------------------------|
| **Codename**        | `vidofnir`                      | `ansuz`                               |
| **SoC**             | MediaTek MT6789 (Helio G99)     | MediaTek MT6878 (Dimensity 7300)      |
| **Halium / kernel** | Halium 12, 5.10 vendor kernel   | Halium 14, android14-6.1 GKI kernel   |
| **Device name used by the scripts** | `x23`           | `ansuz`                               |

## Feature Status

| Feature                                          | X23     | Plinius |
|--------------------------------------------------|---------|---------|
| Boot, USB `hdc`, display, touch, Wi-Fi, audio    | Yes     | Yes     |
| Hardware keys, sensors, vibrator, camera         | Not yet | Yes     |
| Cellular (mobile data, SMS, voice calls)         | Not yet | Yes     |
| Bluetooth, NFC, fingerprint                      | Not yet | Not yet |

Most active development happens on the Plinius.

## Prerequisites

- A Volla Phone X23 or Plinius with an **unlocked bootloader**.
- A Linux host with [Docker](https://docs.docker.com/engine/install/) and about **180 GB** of free disk space.
- [`git-lfs`](https://docs.github.com/en/repositories/working-with-files/managing-large-files/installing-git-large-file-storage) and [`repo`](https://gerrit.googlesource.com/git-repo) installed on the host.
- `fastboot`, from [Android SDK Platform-Tools](https://developer.android.com/tools/releases/platform-tools), on the host that the phone is plugged into.

!!! warning
    Flashing Oniro replaces the operating system that is currently on the phone, such as Ubuntu Touch. Back up any data you want to keep before you start.

## Building

The Volla phones need the **`OpenHarmony-6.1-LTS`** branch. Some steps run inside the build container (the `docker exec` commands) and the rest run on the host, from the root of the source tree.

### Step 1: Get the Source Code

```bash
repo init -u https://github.com/eclipse-oniro4openharmony/manifest.git \
     -b OpenHarmony-6.1-LTS -m oniro.xml --no-repo-verify
repo sync -c
repo forall -c 'git lfs pull'
```

### Step 2: Set Up the Build Container

Oniro provides its own build image. It is the upstream OpenHarmony image plus a few tools that a clean build needs. Build it once, then start a container with the source tree mounted:

```bash
sudo docker build -t oniro-oh-standard:3.2 device/board/oniro/docker

sudo docker run -d -it --name oniro-build \
     -w /home/openharmony \
     -v "$PWD":/home/openharmony/workdir \
     -v "$HOME/.ccache":/root/.ccache \
     -v "$(dirname "$PWD")/openharmony_prebuilts":/home/openharmony/openharmony_prebuilts \
     oniro-oh-standard:3.2 /bin/bash
```

The ccache and prebuilts mounts are optional. They make rebuilds faster and let a new container reuse the toolchains you have already downloaded.

### Step 3: Download Prebuilts and Apply Patches

Download the prebuilt toolchains inside the container. Then apply the Oniro patch series on the host:

```bash
sudo docker exec -u root -w /home/openharmony/workdir oniro-build \
     ./build/prebuilts_download.sh

bash device/board/oniro/system_patch/do_patch.sh
```

!!! note
    `do_patch.sh` applies the patches with `git am`, so set your git identity first (`git config --global user.name` and `user.email`). The patches also register the `hybris_generic` product. If they are not applied, the build fails with an unknown product error.

### Step 4: Build the Oniro System Images

```bash
sudo docker exec -u root -w /home/openharmony/workdir oniro-build \
     ./build.sh --product-name hybris_generic --ccache

# The container builds as root. Take back the output folder for the host-side steps.
sudo chown "$USER" out out/hybris_generic
```

??? tip "Optional: include the Oniro apps (app store, keyboard)"
    By default, the image contains the standard OpenHarmony apps. To add the Oniro distribution apps, build them once on the host before Step 4:

    ```bash
    bash vendor/oniro/oniro-haps/build-oniro-haps.sh
    ```

    This step needs network access and the OpenHarmony SDK and command-line tools. See [Oniro IDE & App Builder setup](../../application-development/environment-setup-guide/oniro/setup.md) to install them with `oniro-app sdk install 6.1` and `oniro-app cmdtools install`.

    Then add `--gn-args "oniro_install_custom_haps=true"` to the `build.sh` command in Step 4. 

### Step 5: Build the Device Images

These commands run on the host. They build the kernel and pack the `super` and boot images for your phone. `pull-halium-blobs.sh` downloads about 1 GB of Halium files from Volla and UBports. You only need to run it once per source tree, because it keeps the downloads.

=== "Volla Phone X23"

    ```bash
    D=device/board/oniro/hybris_generic

    bash $D/kernel/x23/build_kernel.sh
    bash $D/utils/host/pull-halium-blobs.sh -d x23
    bash $D/kernel/x23/build_super_img.sh
    bash $D/kernel/x23/build_boot_img_chainload.sh
    ```

    Output images:

    - `out/hybris_generic/super.img`
    - `out/hybris_generic/boot-chainload.img`
    - `kernel/linux/volla-vidofnir/out/vendor_boot.img`

=== "Volla Phone Plinius"

    ```bash
    D=device/board/oniro/hybris_generic

    bash $D/kernel/ansuz/build_kernel.sh
    bash $D/utils/host/pull-halium-blobs.sh -d ansuz
    bash $D/kernel/ansuz/build_super_img.sh
    bash $D/kernel/ansuz/build_init_boot_chainload.sh
    ```

    Output images:

    - `out/hybris_generic/super.img`
    - `out/hybris_generic/init_boot-chainload.img`
    - `out/hybris_generic/vendor_boot-ohos.img`
    - `kernel/linux/volla-ansuz/out/boot.img`

    !!! note
        `build_kernel.sh` builds the latest commit of the `android14-6.1-halium` kernel branch, so it may print `WARN: … pinned <sha>`. This is expected. After the build, a check compares the kernel with the stock MediaTek modules and stops if they are no longer compatible.

## Flashing

1. Put the phone into **fastboot** mode. Power it off, then hold **Volume Down + Power** and select `fastboot`. If the phone is already running Oniro, you can use this command instead:

    ```bash
    hdc shell "param set ohos.startup.powerctrl reboot,bootloader"
    ```

2. Connect the phone over USB and flash all the images in one pass:

    === "Volla Phone X23"

        ```bash
        bash device/board/oniro/hybris_generic/utils/host/flash-native.sh -d x23
        ```

    === "Volla Phone Plinius"

        ```bash
        bash device/board/oniro/hybris_generic/utils/host/flash-native.sh -d ansuz
        ```

The script writes every partition to slot `_a` and reboots the phone. Run it with `--help` to see more options.

!!! note
    Always flash the kernel together with the `vendor_boot` image from the same build, which `flash-native.sh` does for you. If they don't match, the vendor kernel modules fail to load.

## Connecting with HDC

About 60–70 seconds after the reboot, the Oniro lock screen appears and the phone shows up over USB:

```bash
hdc list targets
hdc shell "uname -a"
```

!!! note
    `hdc` is included in the OpenHarmony SDK toolchain. Ensure it is in your `PATH`.

## How It Works

1. The phone's bootloader loads a small **chainload** image (`boot_a` on the X23, `init_boot_a` on the Plinius).
2. Its ramdisk loads the vendor kernel modules, mounts the Oniro and Halium partitions, and then starts Oniro's `init` as PID 1.
3. Oniro starts `androidd`, which runs the Android (Halium) hardware services in an isolated namespace.
4. Oniro's graphics and hardware services reach those Android HALs through **libhybris**, over a binder connection that both sides share.

A single `super` partition holds both the Oniro partitions (`system`, `vendor`, `sys_prod`, `chip_prod`) and the Halium ones.

## Reference

For more details, see the `hybris_generic` target in the [Oniro Board Support Packages repository](https://github.com/eclipse-oniro4openharmony/device_board_oniro/tree/OpenHarmony-6.1-LTS).
