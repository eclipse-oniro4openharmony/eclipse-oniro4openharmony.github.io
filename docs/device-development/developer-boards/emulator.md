---
title: Oniro Emulator
parent: Developer Boards
nav_order: 3
layout: default
---

# Oniro Emulator

The Oniro Emulator provides an easy and accessible way to develop and test applications or system components without the need for physical hardware.  
It is based on **QEMU**, a powerful open-source machine emulator and virtualizer.  
The emulator uses the **x86_64 architecture**. On an x86 host, it runs with hardware acceleration: **KVM** on Linux, the **Windows Hypervisor Platform** on Windows and **HVF** on Intel-based Macs.

The current emulator is built from **OpenHarmony 7.0** (`OpenHarmony-7.0-Release`). It boots to the SceneBoard lock screen and home screen, with English as the default language. It has the Oniro apps, touch input, sound, and virtual sensors that you can set from the host.

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

### Build the images

Start the build with ccache enabled:

```bash
sudo docker exec -u root -w /home/openharmony/workdir oniro-build \
     ./build.sh --product-name x86_general --ccache
```

To include the Oniro apps (app store, FlorisBoard keyboard and others), see [Optional: include the Oniro apps](volla-phones.md#step-4-build-the-oniro-system-images). The same steps apply to `x86_general`.

The images and the `run.sh` / `run.bat` launch scripts are written to:

```
out/x86_general/packages/phone/images
```

### (Optional) Revert patches

If needed, you can undo the applied patches:

```bash
bash device/board/oniro/system_patch/undo_patch.sh
```

## Alternative: Download Prebuilt Images

Instead of building the images yourself, you can download the [prebuilt Oniro Emulator images](https://github.com/eclipse-oniro4openharmony/device_board_oniro/releases/latest/download/oniro_emulator.zip). The latest release is [v7.0.1](https://github.com/eclipse-oniro4openharmony/device_board_oniro/releases/tag/v7.0.1), built from OpenHarmony 7.0.

After downloading, extract the archive and use the included run scripts as described in the next sections.

!!! tip
    The [`oniro-app`](../../application-development/environment-setup-guide/oniro/setup.md) CLI downloads the same release for you and manages it: `oniro-app emulator install`, then `oniro-app emulator start`, `status`, `connect` and `stop`.

## QEMU Installation

The emulator requires **QEMU**. See the [QEMU download page](https://www.qemu.org/download/) for all platforms.

### Linux

- **On Debian-based Linux distributions:**
  ```bash
  sudo apt install qemu-system-x86
  ```
- **On Fedora/RHEL:**
  ```bash
  sudo dnf install qemu-system-x86-core
  ```
- The emulator requires **KVM**. Add your user to the `kvm` group, then log out and back in:
  ```bash
  sudo usermod -aG kvm $USER
  ```

### Windows

- Install QEMU from the official website, and add its installation directory (e.g., `C:\Program Files\qemu`) to your `PATH`.
- Install [Git for Windows](https://gitforwindows.org/) (Git Bash) or [MSYS2](https://www.msys2.org/). `run.bat` uses Git Bash or MSYS2 to run `run.sh`.
- Enable **Windows Hypervisor Platform**: open **Turn Windows features on or off**, check **Windows Hypervisor Platform**, click **OK**, and restart.
- The host CPU must support AVX2 (x86-64-v3, any Intel CPU from Haswell or AMD CPU from Excavator on). `run.sh` passes these CPU features to the guest under WHPX.

### macOS

- Install QEMU with [Homebrew](https://brew.sh/): `brew install qemu`.
- Intel Macs use hardware acceleration (HVF). Apple Silicon Macs emulate x86, which is much slower.

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

The script picks the right acceleration for your host. If no display is available (for example, over SSH), it switches to headless mode.

The emulator opens in a window (GTK on Linux and Windows, Cocoa on macOS):

- **Mouse clicks and drags are touches.** A single tap wakes the screen.
- **Machine > Power Down** in the window menu is the phone's **power button**. It turns the screen off and on. It does not shut the emulator down.
- The default screen timeout is 10 minutes.

Useful options:

| Option | Description |
|--------|-------------|
| `--headless` | Run without a window. The screen is available over VNC on port 5900 and the serial console over telnet on port 4444 |
| `-s N` | Number of virtual CPUs (default 6) |
| `-m SIZE` | RAM size (default `4096M`) |
| `-r WxH` | Screen resolution (default `360x720`) |
| `--display UI` | Window backend: `gtk`, `cocoa` or `sdl` |
| `--audio DRIVER` | Host audio backend (see [Audio](#audio)) |
| `--help` | Show all options |

!!! note
    If you built the images in the container, they belong to `root`. If `run.sh` fails with `Could not reopen file: Permission denied`, take ownership of them first: `sudo chown "$USER" *.img bzImage`.

## Connecting to the Emulator with HDC

Once the emulator is running, you can connect to it using **HDC** (the OpenHarmony Device Connector). QEMU forwards the emulator's HDC port to `127.0.0.1:55555` on the host:

```bash
hdc tconn 127.0.0.1:55555
hdc shell "uname -a"
```

Wait about a minute for the emulator to boot. The Oniro lock screen then appears in the emulator window, or over VNC in headless mode. Swipe up to unlock.

!!! note
    `hdc` is included in the OpenHarmony SDK toolchain. Ensure it is in your `PATH`.

## Audio

The emulator's sound card plays through the host's speakers and records from its default input. `run.sh` picks the host backend: CoreAudio on macOS, DirectSound on Windows, and on Linux the desktop's PulseAudio or PipeWire server (ALSA if neither is running). To choose one yourself, pass a QEMU `-audiodev` driver:

```bash
./run.sh --audio pipewire               # or pa, alsa, sdl, coreaudio, dsound
./run.sh --audio wav,path=guest.wav     # record what the guest plays to a file
./run.sh --audio none                   # no host audio
```

## Simulating Sensors

The emulator has eight virtual sensors: accelerometer, gyroscope, magnetic field, ambient light, barometer, proximity, humidity and ambient temperature. Each one reads its value from a system parameter, so you can change it from the host at any time:

```bash
hdc shell param get | grep emulator.sensor                      # current values
hdc shell param set emulator.sensor.accelerometer "0 9.81 0"    # held upright
hdc shell param set emulator.sensor.proximity 0                 # something near the screen
hdc shell param set emulator.sensor.ambient_light "20 0 0"      # dim room, in lux
```

Values are floats, in the order and units of the `@ohos.sensor` API. At every boot they reset to the defaults: device lying flat, 300 lux, nothing near the screen.

!!! note
    Screen auto-rotation is not enabled on the emulator, so changing the accelerometer value does not rotate the screen.

## Reference

For additional information please refer to the `x86_general` target in the [Oniro Board Support Packages repository](https://github.com/eclipse-oniro4openharmony/device_board_oniro/tree/OpenHarmony-7.0-Release).
