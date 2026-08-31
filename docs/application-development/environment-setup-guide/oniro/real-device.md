# Connecting a Real Device

!!! warning "Working with multiple devices"
	If multiple devices are connected, such as a real device over USB and an emulator, pass `--device <serial>` to Oniro App Builder commands that target a device.

    Run `oniro-app devices` to list the connected devices. Each output line contains three fields:

    - device serial
    - connection type
    - status

    Oniro IDE does not currently support multiple connected devices. To work with a real device, stop the emulator first.

## Connecting via USB

Connect an Oniro device through USB. When prompted, choose "Transfer files".

<div style="text-align:center">
    <img src='../images/real-device-prompt.png'
    style="width: 300px; max-width: 100%;">
</div> 

Now you'll be able to work with the connected device in the same way you worked with the emulator in the previous section.

## Connecting over Wi-Fi

!!! warning
    You can connect a device over Wi-Fi only with Oniro App Builder. This feature is not available in Oniro IDE.

Some devices — wearables in particular, which often have no USB data connection — must be connected over Wi-Fi instead. Your computer and the device must be on the same Wi-Fi network.

1. On the device, enable **Developer Options**, then turn on **HDC Debugging** and **Debugging via WLAN** (naming can vary slightly by device).
2. Note the IP address and port shown once WLAN debugging is enabled, for example `192.168.1.42:5555`.
3. Run `oniro-app emulator connect --address <IP_ADDRESS>:<PORT>`.

You're now connected and can work with the device in the same way you would work with an emulator.
