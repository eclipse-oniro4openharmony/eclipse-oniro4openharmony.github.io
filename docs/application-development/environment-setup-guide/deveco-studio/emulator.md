# Emulator

Once your UI looks right in the Previewer (see [First App](first-app.md)), the next step is running the app for real — either on an emulator or on physical hardware.

## Device Manager

Open **Device Manager** from the toolbar (or **Tools → Device Manager**) to create and launch emulators.

To create a new emulator:

1. Click **New Emulator**.
2. Choose a device profile (phone, tablet, wearable, TV — availability depends on which system images you've installed via the SDK Manager).
3. Select the system image / API level to match the target you're developing against (see the version/API table in [Process of Installation](installation/process.md)).
4. Give it a name and confirm.

!!! tip "Match the API level to your project"
    If the emulator's API level is lower than your module's `compatibleSdkVersion`/`compileSdkVersion`, install/run can fail or behave inconsistently. Keep at least one emulator matching your project's target API.

### Boot Modes

* **Cold boot** — starts the emulator from a clean state every time. Slower, but useful when you need to rule out state left over from a previous run.
* **Quick boot** (if available for the image) — resumes from a saved snapshot, which is much faster for everyday iteration.

If an emulator becomes unresponsive or gets into a broken state (e.g. the "Unable to find BMS Service" issue described in [Common Issues and Solutions](first-app.md#common-issues-and-solutions)), a cold boot or wiping its data is usually the fastest fix.

## Connecting a Real Device

Physical devices generally give more representative performance and let you test hardware features the emulator can't (real sensors, cellular/Wi-Fi conditions, actual battery behavior).

1. On the device, enable **Developer Options** (usually by tapping the build number several times in system settings) and turn on **USB Debugging**.
2. Connect the device via USB.
3. Accept the debugging authorization prompt on the device the first time it connects.
4. The device should now appear in DevEco Studio's target device dropdown in the toolbar.

If the device isn't detected, check the USB connection troubleshooting steps in [Common Issues and Solutions](first-app.md#common-issues-and-solutions) — unstable USB power management is a common culprit on Windows.

## Using `hdc` from the Terminal

**`hdc`** (HarmonyOS Device Connector) is the command-line counterpart to Device Manager, bundled with the SDK. It's useful when you want to script something or diagnose a connection issue outside the IDE. Open the embedded **Terminal** tool window and try:

```bash
# List connected devices/emulators
hdc list targets

# Open a shell on the (single) connected target
hdc shell

# Push a file to the device
hdc file send ./local-file.txt /data/local/tmp/local-file.txt

# Pull a file from the device
hdc file recv /data/local/tmp/remote-file.txt ./remote-file.txt

# Install a HAP package directly
hdc install ./entry-default-signed.hap
```

!!! note "Multiple targets connected"
    If more than one device/emulator is connected, most `hdc` subcommands need a `-t <target-id>` (or `-s <serial>`, depending on version) flag — run `hdc list targets` first to get the identifier.

## Choosing a Run Target in the IDE

The dropdown next to the Run/Debug buttons in the navigation bar lists every currently available emulator and connected device. Select a target there before clicking **Run** — DevEco Studio remembers your last selection between sessions.

Next: run the app and diagnose problems in [First App](first-app.md#debugging-and-profiling).
