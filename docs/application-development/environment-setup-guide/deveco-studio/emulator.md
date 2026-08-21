# Emulator

After verifying the UI in the Previewer (see [First App](first-app.md)), run the application on an emulator or physical hardware. The emulator works the same way on Windows and macOS.

The virtual devices downloaded in DevEco Studio's Device Manager are HarmonyOS images. Use them for a HarmonyOS project with a matching API and release type. To run a project configured with `runtimeOS: "OpenHarmony"`, use the [Oniro emulator](../oniro/setup.md#downloading-the-emulator) or an OpenHarmony device instead.

## Prerequisites

* **Hardware virtualization** must be enabled — Intel VT-x / AMD-V on Windows (usually a BIOS/UEFI setting). Apple Silicon Macs run the emulator natively; Intel-based Macs do not support emulation in DevEco Studio.
* You must be **signed in with a Huawei ID** — downloading emulator system images requires an authenticated account.
* The first time you download a virtual device image, DevEco Studio shows an additional **User Agreement** (HarmonyOS Software License and Service Agreement, plus the User Experience Improvement Program notice) that you need to accept.

<img src='../images/emulator_user_agreement.png' alt="User Agreement dialog with HarmonyOS Software License and User Experience Improvement Program checkboxes">

## Device Manager

Open **Device Manager** from **Tools → Device Manager** or from the device menu next to the Run and Debug buttons on the toolbar. It appears at the bottom of that menu.

<img src='../images/emulator_device_manager.png' alt="Run target dropdown with Device Manager listed under Huawei|Emulator">

Device Manager's **Your Devices** page lists existing emulators under the **Local Emulator** tab, along with their locations on disk.

<img src='../images/emulator_device_manager2.png' alt="Device Manager Your Devices page, Local Emulator tab, with the New Emulator button">

### Creating a Virtual Device

1. Click **+ New Emulator**.
2. On the **Select Virtual Device** screen, select a device type from the tree on the left (Phone, Foldable, WideFold, TripleFold, Tablet, 2in1, 2in1 Foldable, or TV) and a row that matches the target HarmonyOS and API version (see the version/API table in [Installation Process](installation/process.md)).
3. If the system image has not yet been downloaded, click the download icon in the **Actions** column and wait for the download to finish. Allow approximately **2 GB** of free disk space for the image and approximately **12 GB** to run it.
4. Click **Next**.
<img src='../images/emulator_new_emulator.png' alt="Select Virtual Device screen listing device types, API versions, and a download action per row">
5. On the **Virtual Device Configure** screen, confirm the device name and hardware profile, including the screen size, resolution, and DPI. Click **Customize** to change the screen profile. Choose a boot option as described below, then set the RAM and ROM allocated to the virtual device.
6. Click **Finish** to create it.
<img src='../images/emulator_device_configuration.png' alt="Virtual Device Configure screen with name, screen profile, boot options, RAM and ROM fields">

!!! tip "Match the API level to your project"
    The emulator's API level must be at least the project's `compatibleSdkVersion`, and its release type must match the project. `compileSdkVersion` can be newer than the device when the application does not use unavailable APIs.

### Boot Modes

* **Cold boot** — starts the emulator from a clean state every time. Slower, but useful when you need to rule out state left over from a previous run.
* **Quick boot** — resumes from a saved snapshot, which is much faster for everyday iteration.

If an emulator becomes unresponsive or enters an invalid state, such as the "Unable to Find the BMS Service" problem described in [Common Issues and Solutions](first-app.md#common-issues-and-solutions), perform a cold boot or wipe its data.

## Running Your App on the Emulator

1. In Device Manager, start the emulator (or let the IDE boot it automatically the first time you target it).
2. Return to your project, open the run-target menu on the toolbar, and select the emulator under **Huawei|Emulator**. The menu also contains the **Huawei Simulator** entries (Wearable Simulator and Smart Vision Simulator) and the **Huawei Previewer**.
<img src='../images/emulator_device_selection.png' alt="Run target dropdown with a created emulator selected under Huawei|Emulator">
3. Click **Run**. The app builds, installs, and launches automatically on the emulator.

## Connecting a Real Device

Physical devices generally provide more representative performance and let you test hardware features that the emulator cannot reproduce, such as real sensors, cellular and Wi-Fi conditions, and battery behavior.

1. On the device, enable **Developer Options** (usually by tapping the build number several times in system settings) and turn on **USB Debugging**.
2. Connect the device via USB.
3. Accept the debugging authorization prompt on the device the first time it connects.
4. The device should now appear in DevEco Studio's target device dropdown in the toolbar.

If the device is not detected, follow the USB-connection troubleshooting steps in [Common Issues and Solutions](first-app.md#common-issues-and-solutions). USB power management often causes unstable connections on Windows.

## Using `hdc` from the Terminal

**`hdc`** (HarmonyOS Device Connector) is the command-line counterpart to Device Manager and is bundled with the SDK. Use it to automate tasks or diagnose connection problems outside the IDE. Open the embedded **Terminal** tool window and try the following commands:

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
    If more than one device or emulator is connected, use `hdc -t <target-id> <command>`. Run `hdc list targets` first to get the identifier. The `-s` option selects an HDC server endpoint; it does not select a device serial number.

## Troubleshooting

* **Emulator does not start or displays a black screen** — confirm that virtualization is enabled in the BIOS or UEFI settings on Windows. Check whether other virtualization software, such as Hyper-V or VirtualBox, conflicts with it.
* **Slow performance** — close unused emulators, allocate more RAM/CPU cores to the virtual device in its configuration, and prefer a lower-resolution device profile for quick UI checks.
* **System image download stuck or failing** — verify your network/proxy configuration and that the selected image is available for your host platform and account region.
* **App fails to install on the emulator** — see the `compileSdkVersion`/permission-related entries in [Common Issues and Solutions](first-app.md#common-issues-and-solutions).

Next: run the app and diagnose problems in [First App](first-app.md#debugging-and-profiling).
