---
title: Oniro Emulator
parent: Developer Boards
nav_order: 3
layout: default
---

# Oniro Emulator

The Oniro Emulator provides an easy and accessible way to develop and test applications or system components without the need for physical hardware.  
It is based on **QEMU**, a powerful open-source machine emulator and virtualizer, and runs **OpenHarmony 7.0**.

The emulator comes in two builds of the same device, one per CPU architecture. Use the one your host can run with hardware acceleration:

| Build | Product name | Runs with hardware acceleration on |
|-------|--------------|------------------------------------|
| **x86_64** | `emulator_x86_64` | Linux (**KVM**), Windows (**Windows Hypervisor Platform**), Intel Macs (**HVF**) |
| **arm64** | `emulator_arm64` | **Apple Silicon Macs** (**HVF**), arm64 Linux (**KVM**) |

Either build also runs on the other kind of host, using QEMU's CPU emulation (TCG). This works, but it is much slower: expect several minutes per boot.

This guide provides step-by-step instructions to **build and run the Oniro Emulator**.

<img src="../images/oniro_qemu.gif" alt="Oniro Emulator" width="200"/>

<div style="position:relative;padding-bottom:56.25%;height:0;overflow:hidden;">
  <iframe
    src="https://www.youtube-nocookie.com/embed/m190pu8L-Dk?list=PLy7t4z5SYNaT3VUbRGCoNH471N9sSs0uV&index=3"
    title="Running Applications on Oniro Emulator with DevEco Studio"
    loading="lazy"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; picture-in-picture; web-share"
    allowfullscreen
    style="position:absolute;top:0;left:0;width:100%;height:100%;border:0;">
  </iframe>
</div>

## Download Prebuilt Images

The quickest way to get the emulator is to download a prebuilt release:

- [x86_64 emulator](https://github.com/eclipse-oniro4openharmony/device_board_oniro/releases/latest/download/oniro_emulator.zip) (`oniro_emulator.zip`)
- [arm64 emulator](https://github.com/eclipse-oniro4openharmony/device_board_oniro/releases/latest/download/oniro_emulator_arm64.zip) (`oniro_emulator_arm64.zip`), for Apple Silicon Macs and arm64 Linux

Extract the archive, [install QEMU](#qemu-installation) and use the included run scripts as described in [Running the Emulator](#running-the-emulator).

!!! tip
    `oniro-app emulator install` downloads the build that suits your host, and `oniro-app emulator start` runs it. Pass `--arch x86_64` or `--arch arm64` to choose one yourself (oniro-app 0.21.0 or later).

To build the images yourself instead, follow the next section.

## Fetch and Build

You need a Linux host with [Docker](https://docs.docker.com/engine/install/) and about **150 GB** of free disk space. Also install [`git-lfs`](https://docs.github.com/en/repositories/working-with-files/managing-large-files/installing-git-large-file-storage) and [`repo`](https://gerrit.googlesource.com/git-repo). This guide uses the **`OpenHarmony-7.0-Release`** branch. Build commands run inside the build container (the `docker exec` commands). Everything else runs on the host, from the root of the source tree.

### Get the source code

```bash
repo init -u https://github.com/eclipse-oniro4openharmony/manifest.git \
     -b OpenHarmony-7.0-Release -m oniro.xml --no-repo-verify
repo sync -c
repo forall -c 'git lfs pull'
```

### Set up the build container

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

!!! tip
    To work interactively, open a shell in the container with `sudo docker exec -it oniro-build bash` and run the commands from `/home/openharmony/workdir`.

### Download the prebuilt toolchains

Run this once for each new source tree:

```bash
sudo docker exec -u root -w /home/openharmony/workdir oniro-build \
     ./build/prebuilts_download.sh
```

### Apply source patches

Run the patching script on the host:

```bash
bash device/board/oniro/system_patch/do_patch.sh
```

!!! note
    The script applies the patches with `git am`, so set your git identity first (`git config --global user.name` and `user.email`).

### Fetch the x86_64 web engine (x86_64 build only)

OpenHarmony ships the ArkWeb engine only for arm and arm64. Without an x86_64 engine, the Browser and every in-app WebView stay blank on the x86_64 emulator. Fetch the x86_64 build once for each source tree (about 150 MB):

```bash
bash device/board/oniro/emulator/utils/fetch-arkweb-core.sh
```

The arm64 build uses the stock engine and skips this step.

### Build the images

Start the build with ccache enabled, using the product name of the build you want:

```bash
sudo docker exec -u root -w /home/openharmony/workdir oniro-build \
     ./build.sh --product-name emulator_x86_64 --ccache    # or emulator_arm64
```

The images and the `run.sh` / `run.bat` launch scripts are written to:

```
out/emulator_x86_64/packages/phone/images     # out/emulator_arm64/... for arm64
```

The kernel is `bzImage` for x86_64 and `Image` for arm64.

### (Optional) Revert patches

If needed, you can undo the applied patches:

```bash
bash device/board/oniro/system_patch/undo_patch.sh
```

## QEMU Installation

The emulator requires **QEMU**. See the [QEMU download page](https://www.qemu.org/download/) for all platforms. The x86_64 build uses `qemu-system-x86_64`, and the arm64 build uses `qemu-system-aarch64`.

### Linux

- **On Debian-based Linux distributions:**
  ```bash
  sudo apt install qemu-system-x86      # x86_64 build
  sudo apt install qemu-system-arm      # arm64 build
  ```
- **On Fedora/RHEL:**
  ```bash
  sudo dnf install qemu-system-x86-core       # x86_64 build
  sudo dnf install qemu-system-aarch64-core   # arm64 build
  ```
- For hardware acceleration, the emulator uses **KVM**. Add your user to the `kvm` group, then log out and back in:
  ```bash
  sudo usermod -aG kvm $USER
  ```

### Windows

- Install QEMU from the official website, and add its installation directory (e.g., `C:\Program Files\qemu`) to your `PATH`.
- Install [Git for Windows](https://gitforwindows.org/) (Git Bash) or [MSYS2](https://www.msys2.org/). `run.bat` uses Git Bash or MSYS2 to run `run.sh`.
- Enable **Windows Hypervisor Platform**: open **Turn Windows features on or off**, check **Windows Hypervisor Platform**, click **OK**, and restart.
- Use the x86_64 build.

### macOS

- Install QEMU with [Homebrew](https://brew.sh/): `brew install qemu`.
- On an **Apple Silicon Mac**, use the **arm64** build: it runs with hardware acceleration (HVF). The x86_64 build also starts there, but it is emulated and much slower.
- On an **Intel Mac**, use the **x86_64** build, which runs with HVF.

## Running the Emulator

From the images directory, start the emulator with the script for your operating system:

- **Linux / macOS:**
  ```bash
  ./run.sh
  ```

- **Windows:**
  ```powershell
  .\run.bat
  ```

The script tells which build it is from the kernel next to it, and picks the right acceleration for your host. If the host has none for that build, for example the arm64 build on an x86_64 PC, it falls back to CPU emulation and gives the system longer timeouts. If no display is available (for example, over SSH), it switches to headless mode.

In the emulator window, mouse clicks are touches, and **Machine > Power Down** is the phone's power button (it turns the screen off and on).

Useful options:

| Option | Description |
|--------|-------------|
| `--headless` | Run without a window. The screen is available over VNC on port 5900 and the serial console over telnet on port 4444 |
| `-s N` | Number of virtual CPUs |
| `-m SIZE` | RAM size |
| `-r WxH` | Screen resolution |
| `-a MODE` | Acceleration: `kvm`, `hvf`, `whpx` or `tcg` (CPU emulation). The default picks the host's own |
| `--audio DRIVER` | Host audio backend (see [Audio](#audio)) |
| `--wifi` | WiFi instead of ethernet (see [WiFi](#wifi)) |
| `--shared-dir DIR` | Share a host folder with the emulator (see [Shared Folder, Storage and Several Emulators](#shared-folder-storage-and-several-emulators)) |
| `--userdata-size SIZE` | Grow the `/data` partition, e.g. `16G` |
| `--instance N` | Run another emulator alongside the first one |
| `--help` | Show all options |

!!! note
    If you built the images in the container, they belong to `root`. If `run.sh` fails with `Could not reopen file: Permission denied`, take ownership of them first: `sudo chown "$USER" *.img bzImage Image`.

## Connecting to the Emulator with HDC

Once the emulator is running, you can connect to it using **HDC** (the OpenHarmony Device Connector). QEMU forwards the emulator's HDC port to `127.0.0.1:55555` on the host:

```bash
hdc tconn 127.0.0.1:55555
hdc shell "uname -a"
```

With hardware acceleration, the emulator boots in about a minute. The Oniro lock screen then appears in the emulator window, or over VNC in headless mode. Under CPU emulation, the first boot takes several minutes.

!!! note
    `hdc` is included in the OpenHarmony SDK toolchain. Ensure it is in your `PATH`.

## Audio

The emulator's sound card plays through the host's speakers. `run.sh` picks the host audio backend automatically. To choose one yourself, pass a QEMU `-audiodev` driver:

```bash
./run.sh --audio pipewire               # or pa, alsa, sdl, coreaudio, dsound
./run.sh --audio none                   # no host audio
```

## Simulating Sensors

The emulator has virtual sensors (accelerometer, gyroscope, magnetic field, ambient light, barometer, proximity, humidity and ambient temperature). Each one reads its value from a system parameter, so you can change it at any time:

```bash
hdc shell param get | grep emulator.sensor                      # current values
hdc shell param set emulator.sensor.accelerometer "0 9.81 0"    # held upright
hdc shell param set emulator.sensor.ambient_light "20 0 0"      # dim room, in lux
```

Values are floats, in the order and units of the `@ohos.sensor` API. They reset to the defaults at every boot.

## Simulating Location

A simulated GNSS receiver reports the position set in `emulator.gnss.location` once location is switched on (**Control Center > Location**):

```bash
hdc shell param set emulator.gnss.location "48.8584 2.2945 35"   # latitude longitude [altitude]
hdc shell param set emulator.gnss.location ""                    # no fix
```

## Simulating Cellular

The emulator has a simulated LTE modem with a SIM: signal bars, outgoing calls (without audio) and SMS in both directions. From the host you can change the signal, deliver an SMS or ring the phone:

```bash
hdc shell param set emulator.modem.signal 2                       # 0-4 bars
hdc shell param set emulator.modem.sms "+447700900123 Hello"      # incoming SMS
hdc shell param set emulator.modem.call +447700900123             # incoming call
hdc shell param set emulator.modem.call hangup                    # the caller hangs up
```

Mobile data is not available. The emulator's network comes from ethernet or WiFi.

## Camera

The Camera app previews and takes photos of a test pattern with a live timestamp.

## WiFi

`./run.sh --wifi` gives the emulator WiFi instead of ethernet, as on a phone. **Settings > WLAN** lists one open network, `VirtWifi`. Join it to connect the emulator to the network.

## Web

The Browser and in-app WebViews (the ArkUI `Web` component) render pages. The Browser is in the image but not installed by default:

```bash
hdc shell bm install -p /system/app/com.ohos.browser/
```

## Shared Folder, Storage and Several Emulators

```bash
./run.sh --shared-dir ~/shared        # host folder at /mnt/host in the emulator (hdc shell)
./run.sh --userdata-size 16G          # grow /data (it never shrinks)
./run.sh --instance 1                 # a second emulator, with its own /data, on hdc 127.0.0.1:55556
```

An instance keeps its `/data` in `instances/N` next to the images and uses HDC port 55555+N, VNC display N and serial port 4444+N. Delete its directory to reset it. The shared folder needs a QEMU with 9p support (Linux and macOS).

## Reference

For additional information please refer to the `emulator_x86_64` and `emulator_arm64` targets in the [Oniro Board Support Packages repository](https://github.com/eclipse-oniro4openharmony/device_board_oniro/tree/OpenHarmony-7.0-Release) and the [release notes](https://github.com/eclipse-oniro4openharmony/device_board_oniro/releases).
