# Setup

## Installing the Tools

Install Oniro IDE from either the [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?ItemName=oniro.oniro-app-ide) or the [Open VSX Registry](https://open-vsx.org/extension/oniro/oniro-app-ide).

To install Oniro App Builder, follow the instructions on [GitHub](https://github.com/eclipse-oniro4openharmony/oniro-app-builder).

!!! note
    Make sure that you have JDK installed and that it's available on `PATH`, because it's needed for app signing.

## Downloading the SDK and Command-Line Tools

For full functionality, install the command-line tools and an SDK version compatible with your target device.

!!! note
    The emulator runs OpenHarmony 7.0 (API level 26). Apps built with the 6.1 SDK (API level 23) run on it.

!!! warning
    On Windows or macOS, download only the SDK at this stage. The next section explains how to download the command-line tools for these systems.

=== "Oniro IDE"

    Open the **SDK Manager** tab.

    <div style="text-align:center">
        <img src='../images/download-sdk.png'>
    </div> 

    Click `Download` next to the SDK version you want, and `Install` in the command-line tools box.

=== "Oniro App Builder"

    Install the SDK with:

    `oniro-app sdk install 6.1` (or another version)

    For command-line tools, run:

    `oniro-app cmdtools install`

### Command-Line Tools on Windows and macOS

The command-line tools for Windows and macOS cannot be downloaded automatically. Download them from the [Huawei Developer website](https://developer.huawei.com/consumer/en/download/).

=== "Oniro IDE"

    Click **Install** in the command-line tools box on the **SDK Manager** tab. When prompted, select the ZIP archive that you downloaded from the Huawei Developer website.

    <div style="text-align:center">
        <img src='../images/cmd-tools-install.png'>
    </div> 

=== "Oniro App Builder"

    Run `oniro-app cmdtools install --from-zip <path to your zip>`.

## Downloading the Emulator

The emulator comes in two builds, **x86_64** and **arm64**. It requires QEMU and uses a host-specific accelerator:

- Linux: KVM must be enabled and accessible to the current user.
- Windows: enable Windows Hypervisor Platform. The launcher also requires Git Bash, MSYS2, or Cygwin.
- macOS: Intel Macs run the x86_64 build and Apple Silicon Macs the arm64 build, both with Hypervisor.framework.

Without hardware acceleration, for example the arm64 build on an x86_64 PC, the emulator falls back to TCG emulation, which is much slower.

!!! tip
    A window manager that forces the QEMU window to a fixed size can cause display problems. If you encounter problems, ensure that the QEMU window is floating and is not being resized automatically.

First, install [QEMU](https://www.qemu.org/download/) and ensure that `qemu-system-x86_64` (x86_64 build) or `qemu-system-aarch64` (arm64 build) is on `PATH`.

### Inside IDE or App Builder

=== "Oniro IDE"

    Download the emulator from the **SDK Manager** tab. Use the **Start Emulator** and **Stop Emulator** buttons to manage it.

    <div style="text-align:center">
        <img src='../images/download-emulator.png'>
    </div> 

=== "Oniro App Builder"

    Install the emulator with `oniro-app emulator install`. It picks the build for your host: arm64 on Apple Silicon Macs and arm64 Linux, x86_64 elsewhere. Pass `--arch x86_64` or `--arch arm64` to choose one yourself (oniro-app 0.21.0 or later).

    You can manage the emulator with:

    - `oniro-app emulator start` 
    - `oniro-app emulator stop`

### Standalone

You can also download the emulator directly: the [x86_64 build](https://github.com/eclipse-oniro4openharmony/device_board_oniro/releases/latest/download/oniro_emulator.zip) or the [arm64 build](https://github.com/eclipse-oniro4openharmony/device_board_oniro/releases/latest/download/oniro_emulator_arm64.zip). See [Oniro Emulator](../../../device-development/developer-boards/emulator.md) for all its options. After extracting the ZIP archive, run `run.sh` on Linux or macOS, or `run.bat` on Windows.
