# Installing the tools

For Oniro IDE, you can get it either from [Visual Studio Code Marketplace](https://marketplace.visualstudio.com/items?ItemName=oniro.oniro-app-ide)or [VSX Registry](https://open-vsx.org/extension/oniro/oniro-app-ide).

For Oniro App Builder follow the instructions on [github](https://github.com/eclipse-oniro4openharmony/oniro-app-builder).

> Make sure that you have JRE installed as it is needed for app signing.

# Downloading SDK and command-line tools

To get the full functionality you need to install command-line tools and SDK version compatible with your target device.

> the emulator runs OpenHarmony 6.1 (API level 23)

In IDE use the "SDK manager" tab. 

<div style="text-align:center">
    <img src='../images/download-sdk.png'>
</div> 

For App Builder run:

- `oniro-app cmdtools install`
- `oniro-app sdk install 6.1` (or another version)

# Downloading the emulator

> Your machine needs to have [KVM](https://linux-kvm.org/page/Main_Page) enabled

> It can cause some issues if your window manager forces the QEMU window into a certain size. If you're having trouble, make sure that QEMU window is floating and isn't being rescaled.

First, install [QEMU](https://www.qemu.org/download/#linux)

## Inside IDE or App Builder

You can download the emulator inside the "SDK manager" tab in IDE, and manage it using the "Start Emulator" and "Stop Emulator" buttons.

<div style="text-align:center">
    <img src='../images/download-emulator.png'>
</div> 

With App Builder, you'll need the following commands:

- `oniro-app emulator install`
- `oniro-app emulator start`
- `oniro-app emulator stop`

## Standalone

You can also download the emulator yourself [here](https://github.com/eclipse-oniro4openharmony/device_board_oniro/releases/latest/download/oniro_emulator.zip). After extracting the .zip file, inside you'll find a `run.sh` script that starts the emulator.