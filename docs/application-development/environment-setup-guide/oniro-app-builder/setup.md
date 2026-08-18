# Install Oniro Emulator

- [Install QEMU](https://www.qemu.org/download/#linux)
- [Download Oniro image](https://github.com/eclipse-oniro4openharmony/device_board_oniro/releases/latest/download/oniro_emulator.zip)
- Together with the Oniro image you'll find script run.sh. Run it to start the emulator.

> Your machine needs to have [KVM](https://wiki.archlinux.org/title/KVM) enabled

> If you have a tiling window manager installed, QEMU might be forced into a wrong resolution which might make the emulator not work. Configure your window manager in such a way that QEMU windows have the correct resolution before Oniro starts booting up. Deafult resolution is 360x720. You might need to make the window floating, and resize it to the correct size. (keep in mind that you might to need to resize it to for example 362x722 if you have 1 pixel borders.)

# Install Oniro App Builder

- Install [Oniro app builder](https://github.com/eclipse-oniro4openharmony/oniro-app-builder).
- Run `oniro-app cmdtools install` to install required tools.
- Run `oniro-app sdk install 6.1` to install SDK.
<!-- - TODO install jdk (`sudo apt install default-jre`) -->