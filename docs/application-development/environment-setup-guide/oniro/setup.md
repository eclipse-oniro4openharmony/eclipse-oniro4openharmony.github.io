# Installing the Tools

Install Oniro IDE from either the [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?ItemName=oniro.oniro-app-ide) or the [Open VSX Registry](https://open-vsx.org/extension/oniro/oniro-app-ide).

To install Oniro App Builder, follow the instructions on [GitHub](https://github.com/eclipse-oniro4openharmony/oniro-app-builder).

> Ensure that you have a Java Runtime Environment (JRE) installed; application signing requires it.

# Downloading the SDK and Command-Line Tools

For full functionality, install the command-line tools and an SDK version compatible with your target device.

!!! note
    On Windows or macOS, download only the SDK at this stage. The next section explains how to download the command-line tools for these systems.

> The emulator runs OpenHarmony 6.1 (API level 23).

In Oniro IDE, open the **SDK Manager** tab.

<div style="text-align:center">
    <img src='../images/download-sdk.png'>
</div> 

For Oniro App Builder, run:

- `oniro-app cmdtools install`
- `oniro-app sdk install 6.1` (or another version)

## Command-Line Tools on Windows and macOS

The command-line tools for Windows and macOS cannot be downloaded automatically. Download them from the [Huawei Developer website](https://developer.huawei.com/consumer/en/download/).

In Oniro IDE, click **Install** in the command-line tools box on the **SDK Manager** tab. When prompted, select the ZIP archive that you downloaded from the Huawei Developer website.

<div style="text-align:center">
    <img src='../images/cmd-tools-install.png'>
</div> 

<div style="text-align:center">
    <img src='../images/cmd-tools-prompt.png'>
</div> 

With Oniro App Builder, run `oniro-app cmdtools install --from-zip <path to your zip>`.

# Downloading the Emulator

> Your machine must have [KVM](https://linux-kvm.org/page/Main_Page) enabled.

> A window manager that forces the QEMU window to a fixed size can cause display problems. If you encounter problems, ensure that the QEMU window is floating and is not being resized automatically.

First, install [QEMU](https://www.qemu.org/download/#linux).

## Inside IDE or App Builder

Download the emulator from the **SDK Manager** tab in Oniro IDE. Use the **Start Emulator** and **Stop Emulator** buttons to manage it.

<div style="text-align:center">
    <img src='../images/download-emulator.png'>
</div> 

With Oniro App Builder, use the following commands:

- `oniro-app emulator install`
- `oniro-app emulator start`
- `oniro-app emulator stop`

## Standalone

You can also [download the emulator directly](https://github.com/eclipse-oniro4openharmony/device_board_oniro/releases/latest/download/oniro_emulator.zip). After extracting the ZIP archive, run the included `run.sh` script to start the emulator.
