# Emulator

Once your UI looks right in the Previewer (see [First App](first-app.md)), the next step is running the app for real — either on an emulator or on physical hardware. The emulator works the same way on Windows and macOS.

## Prerequisites

* **Hardware virtualization** must be enabled — Intel VT-x / AMD-V on Windows (usually a BIOS/UEFI setting). Apple Silicon Macs run the emulator natively; Intel-based Macs do not support emulation in DevEco Studio.
* You must be **signed in with a Huawei ID** — downloading emulator system images requires an authenticated account.
* The first time you download a virtual device image, DevEco Studio shows an additional **User Agreement** (HarmonyOS Software License and Service Agreement, plus the User Experience Improvement Program notice) that you need to accept.

<img src='../images/emulator_user_agreement.png' alt="User Agreement dialog with HarmonyOS Software License and User Experience Improvement Program checkboxes">

## Device Manager

Open **Device Manager** either from **Tools → Device Manager**, or from the device dropdown next to the Run/Debug buttons in the toolbar (it's listed at the bottom of that dropdown).

<img src='../images/emulator_device_manager.png' alt="Run target dropdown with Device Manager listed under Huawei|Emulator">

Device Manager's **Your Devices** page lists any emulators you've already created under the **Local Emulator** tab, along with the on-disk location they're stored in.

<img src='../images/emulator_device_manager2.png' alt="Device Manager Your Devices page, Local Emulator tab, with the New Emulator button">

### Creating a Virtual Device

1. Click **+ New Emulator**.
2. On the **Select Virtual Device** screen, pick a device type from the tree on the left (Phone, Foldable, WideFold, TripleFold, Tablet, 2in1, 2in1 Foldable, TV) and a row matching the HarmonyOS/API version you're targeting (see the version/API table in [Process of Installation](installation/process.md)).
3. If the system image isn't downloaded yet, click the download icon in the **Actions** column and wait for it to finish — budget roughly **2 GB** of free disk space to download an image and around **12 GB** free to actually run it.
4. Click **Next**.
<img src='../images/emulator_new_emulator.png' alt="Select Virtual Device screen listing device types, API versions, and a download action per row">
5. On the **Virtual Device Configure** screen, confirm the device name and hardware profile — screen size, resolution, and DPI (or click **Customize** to change the screen profile) — choose a boot option (see below), and set the RAM/ROM allocated to the virtual device.
6. Click **Finish** to create it.
<img src='../images/emulator_device_configuration.png' alt="Virtual Device Configure screen with name, screen profile, boot options, RAM and ROM fields">

!!! tip "Match the API level to your project"
    If the emulator's API level is lower than your module's `compatibleSdkVersion`/`compileSdkVersion`, install/run can fail or behave inconsistently. Keep at least one emulator matching your project's target API.

!!! note "Limited device types available?"
    If Device Manager only offers a narrow set of device types, your DevEco Studio region setting may be restricting them — see [Enabling Additional Device Types](installation/process.md#enabling-additional-device-types).

### Boot Modes

* **Cold boot** — starts the emulator from a clean state every time. Slower, but useful when you need to rule out state left over from a previous run.
* **Quick boot** — resumes from a saved snapshot, which is much faster for everyday iteration.

If an emulator becomes unresponsive or gets into a broken state (e.g. the "Unable to find BMS Service" issue described in [Common Issues and Solutions](first-app.md#common-issues-and-solutions)), a cold boot or wiping its data is usually the fastest fix.

## Running Your App on the Emulator

1. In Device Manager, start the emulator (or let the IDE boot it automatically the first time you target it).
2. Back in your project, open the run target dropdown in the toolbar and select the emulator under **Huawei|Emulator** — alongside it you'll also find the **Huawei Simulator** entries (Wearable Simulator, Smart Vision Simulator) and the **Huawei Previewer**.
<img src='../images/emulator_device_selection.png' alt="Run target dropdown with a created emulator selected under Huawei|Emulator">
3. Click **Run**. The app builds, installs, and launches automatically on the emulator.

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

## Troubleshooting

* **Emulator won't start / black screen** — confirm virtualization is enabled in BIOS/UEFI (Windows), and check that no other virtualization software (Hyper-V, VirtualBox, etc.) is conflicting with it.
* **Slow performance** — close unused emulators, allocate more RAM/CPU cores to the virtual device in its configuration, and prefer a lower-resolution device profile for quick UI checks.
* **System image download stuck or failing** — check the region setting from [Enabling Additional Device Types](installation/process.md#enabling-additional-device-types) (some system images are region-restricted) and verify your network/proxy configuration.
* **App fails to install on the emulator** — see the `compileSdkVersion`/permission-related entries in [Common Issues and Solutions](first-app.md#common-issues-and-solutions).

Next: run the app and diagnose problems in [First App](first-app.md#debugging-and-profiling).
