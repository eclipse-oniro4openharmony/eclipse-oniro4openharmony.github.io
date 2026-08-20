# First App

With DevEco Studio installed and its layout, project structure, and tooling covered in [Workflow](workflow.md), this page walks through creating an actual project, seeing it render, running it, and diagnosing anything that goes wrong along the way.

## Creating Your First Project

1. On the **Welcome** screen, click **New Project** (or, with a project already open, **File → New → New Project**).
2. Pick a template. **Empty Ability** is the simplest starting point for a Stage-model app and is the best choice for a first project — other templates add extra sample UI you don't need yet.
3. Choose the device types your app should target (Phone, Tablet, Wearable, ...). This sets the initial `deviceTypes` list in `module.json5`, which you can adjust later (see [Project Structure](workflow.md#project-structure)).
4. Fill in the project details:
    * **Project name** and **Bundle name** (reverse-domain style, e.g. `com.example.myapplication`).
    * **Save location** on disk.
    * **Compile/Compatible API** — match this to the OpenHarmony version you intend to run against (see the version/API table in [Process of Installation](installation/process.md)).
5. Click **Finish**. DevEco Studio generates the project and opens it; the first indexing pass can take a minute or two on a new machine.

!!! tip "Start from Empty Ability"
    Even if your real app will need more structure, starting from **Empty Ability** and adding pages/modules yourself builds a much clearer mental model of the project than trimming down a fuller template.

Once the project is open, `entry/src/main/ets/pages/Index.ets` is the default page you'll see rendered first — open it to continue.

## Using the Previewer

The **Previewer** renders your ArkUI pages without needing an emulator or a physical device, which makes it the fastest feedback loop while building UI.

### Opening the Previewer

Open any page under `entry/src/main/ets/pages/` (a file with an `@Entry @Component struct` declaration). The Previewer panel should appear automatically, usually docked to the right of the editor. If it doesn't:

1. Click inside the `.ets` file so it has focus.
2. Look for the **Previewer** tab along the tool window bar, or use **View → Tool Windows → Previewer**.

!!! note "First render can be slow"
    The first preview of a session compiles the module, so it can take noticeably longer than subsequent updates. Later edits generally re-render in a second or two.

### Live Updates

With the Previewer open, most changes to the page's `build()` method are reflected as soon as you save (or immediately, if **Auto-save** is enabled). This works for:

* Layout and styling changes (padding, colors, alignment).
* Adding/removing components.
* Changes to `@State` initial values.

It does **not** reliably reflect:

* Behavior driven by native code or platform APIs unavailable in the preview sandbox.
* Runtime logic that depends on a real network call, file system, or sensor.
* Some animations and gesture-driven interactions — verify those on an emulator or device.

### Multi-Device Preview

Click the device selector above the Previewer canvas to render the same page across several device profiles at once — phone, tablet, foldable, and wearable, for instance. This is the fastest way to catch layout breakage on smaller or larger screens before you ever touch an emulator.

!!! tip
    Keep at least one small-screen and one large-screen profile enabled by default for any page with non-trivial layout — most responsive-layout bugs show up immediately in this comparison view.

### Interactive Preview

By default, the Previewer is a static snapshot of the page's initial render. Switching to **Interactive Preview** mode (button above the canvas) lets you click, tap, and scroll inside the rendered page as if it were running on a device — useful for checking simple state changes (toggles, tab switches, list scrolling) without a full deploy.

### Previewer Settings

Under **Settings → Languages & Frameworks → ArkUI Previewer** (path may vary slightly by version) you can adjust:

* Which device profiles are shown by default.
* Whether the Previewer refreshes automatically or only on manual trigger.
* Rendering scale, useful on high-DPI displays where the default preview looks too large or small.

### When to Stop Trusting the Previewer

The Previewer is a productivity tool, not a substitute for testing on a real target. Always validate on an emulator or device (see [Emulator](emulator.md)) before considering a feature done, especially anything touching permissions, sensors, background tasks, or performance.

## Running Your App

Once the page looks right in the Previewer, pick a run target from the dropdown in the navigation bar and click **Run**. If no emulator or device is listed yet, set one up first in [Emulator](emulator.md) — that page covers creating an emulator with Device Manager and connecting real hardware in detail.

## Debugging and Profiling

Running your app is only half the job — DevEco Studio's debugger and profiler are what let you find out *why* something is wrong.

### Starting a Debug Session

Select a run target (see [Emulator](emulator.md)), then click the **Debug** icon (bug shape) instead of **Run** in the navigation bar. The app launches the same way, but now stops at breakpoints and lets you inspect state.

### Breakpoints

* Click in the gutter to the left of a line number to set a line breakpoint (a red dot appears).
* Right-click a breakpoint to configure it further:
    * **Condition** — only stop when an expression evaluates to true, e.g. `index == 3`. Useful inside loops where you only care about one iteration.
    * **Log message** — print a message (optionally including expression values) without actually pausing execution. This effectively gives you a temporary, zero-recompile `console.log` you can remove later without touching source.
    * **Suspend policy** — stop only the current thread or the whole process.

### While Paused

The **Debug** tool window shows:

| Pane | Purpose |
|---|---|
| Frames | Current call stack; click a frame to inspect its local variables |
| Variables | Local variables and `this` in the selected frame, expandable for object/array contents |
| Watches | Expressions you pin so they're always visible while stepping |
| Evaluate Expression (`Alt+F8`) | Run arbitrary expressions against the current paused state, including calling functions |

Step controls:

| Action | Shortcut |
|---|---|
| Step over | `F8` |
| Step into | `F7` |
| Step out | `Shift+F8` |
| Resume program | `F9` |

!!! tip "Evaluate Expression is underused"
    Instead of adding a temporary variable just to inspect a computed value, pause on a breakpoint and use **Evaluate Expression** (`Alt+F8`) — it can call methods and index into objects live, which is often faster than editing code and restarting.

### HiLog

The **Log** (HiLog) tool window streams the device/emulator's system log. Two filters make it usable on a busy device:

* **Tag filter** — match the tag used in your `hilog` calls (e.g. a per-module tag you define).
* **Log level** — restrict to `WARN`/`ERROR` when hunting a crash, or `DEBUG`/`INFO` during normal iteration.

You can also save a filter configuration so you don't have to re-type it every session.

### Profiler

Open **View → Tool Windows → Profiler** (or the toolbar icon) while the app is running to attach the profiler. It has separate tabs:

| Tab | What it shows |
|---|---|
| CPU | Method-level call tree and flame chart during a recorded trace, to find hot functions |
| Memory | Live heap size, allocation tracking, and the ability to trigger/inspect a heap snapshot to hunt leaks |
| Network | Individual requests, timing, and payload size for HTTP(S) traffic made by the app |
| Energy (where available) | Coarse indicators of what's driving power usage (radio, CPU, GPS) |

!!! note "Profiling overhead"
    Recording detailed CPU traces or full allocation tracking adds overhead and will skew timings somewhat. Use a lighter sampling mode first to find the general area of a problem, then a detailed trace to zoom in.

### A Practical Workflow

1. Reproduce the issue once without any tooling attached, so you know what "broken" looks like.
2. If it's a logic bug, set a conditional breakpoint close to the suspected cause and step from there.
3. If it's a performance issue, record a CPU trace covering just the slow interaction, not the whole session — shorter traces are far easier to read.
4. If it's a crash under load or over time, check Memory for steadily growing retained size across repeated actions — a classic leak signature.

## Build Variants and Signing

Sooner or later you need a package you can hand to someone else — a `.hap`/`.app` file installable outside of DevEco Studio's own Run/Debug flow. That requires understanding build products and signing.

### Debug vs. Release

By default, running or debugging from the IDE produces a **debug** build: automatically signed with a debug certificate so it can be installed on your own emulator/device, but not meant for distribution.

A **release** build is what you'd hand off or publish — optimized, and signed with a certificate meant to persist across builds (so updates are trusted as coming from the same source).

Which variant gets built is controlled by the **Build Variant** selector, plus the products/targets declared in the project's `build-profile.json5` files (see [Project Structure](workflow.md#project-structure)).

### Signing Configurations

DevEco Studio supports two signing approaches:

#### Automatic Signing (recommended for getting started)

1. Go to **File → Project Structure → Signing Configs** (path may vary slightly by version).
2. Enable **Automatically generate signing configuration**.
3. Sign in with your Huawei/HarmonyOS developer account when prompted.

DevEco Studio then generates a keystore, certificate, and provisioning profile for you and wires them into `build-profile.json5` automatically. This is the fastest path and is sufficient for local testing and most individual development.

#### Manual Signing

Needed for CI pipelines, team-shared release certificates, or when a specific provisioning profile (with particular permissions/ACLs) must be used.

1. Generate a private key and CSR: **Build → Generate Key and CSR**, or via `keytool` directly if you need full control over the parameters.
2. Apply for/download a certificate and provisioning profile from the developer console using that CSR.
3. In **Project Structure → Signing Configs**, point to the `.p12`/keystore file, the certificate, and the profile, and supply the store/key passwords.
4. Reference this signing config from the relevant product entry in `build-profile.json5`.

!!! warning "Keep release keys out of the repository"
    Never commit a keystore file or its passwords. Store them in a secrets manager or CI-only environment variables, and keep only a *reference* (path/alias) in version control — see [Version Control](workflow.md#version-control) for a `.gitignore` starting point.

### Generating a Package

Once a signing configuration is in place:

* **Build → Build Hap(s)/APP(s) → Build Hap(s)** — produces a `.hap` for the current module/product.
* **Build → Build Hap(s)/APP(s) → Build APP(s)** — produces an `.app` bundle if your product is configured to build one (used for multi-HAP distribution).

Build output appears under the module's `build/` directory, and the **Build** tool window (`Alt+0`) shows progress and any failures.

### Diagnosing Signing/Permission Errors

Two errors are common enough to call out specifically (both also covered in [Common Issues and Solutions](#common-issues-and-solutions) below):

* **`compileSdkVersion`/`releaseType` mismatch with the device** — the compiled SDK version is newer than what the target device supports. Lower the compiled version in the relevant `build-profile.json5`, or target a newer device/emulator.
* **Install failed due to "grant request permissions failed"** — the requested permission's level (`system_basic` or `system_core`) requires the ACLs be explicitly listed in the provisioning profile used for signing. Check [this permissions reference](https://gitcode.com/openharmony/resources/blob/master/systemres/main/config.json) for the level of each permission your `module.json5` requests, and make sure your signing profile grants it.

### A Practical Checklist Before Distributing a Build

1. Confirm you're building the **release** variant, not debug.
2. Confirm the signing config references a certificate meant for distribution, not the auto-generated debug one.
3. Bump `versionCode`/`versionName` in `AppScope/app.json5` if this is an update to a previously distributed build.
4. Do a clean install test on a device that was never used for debug builds of this app, to rule out state left over from development.

## Common Issues and Solutions

### Cannot find the emulator of a phone device

`entry\src\main\module.json5` is the configuration file for the module, check `deviceTypes`, add `phone` if it is missing.

![alt text](../images/SDK-11.png)

### Unable to find BMS Service when running on Emulator

Just wait for an extended period, or try clearing data of this device or creating a new device.

### Unstable USB connection, dev board not detected by IDE

solution worked for me:

Change USB Power Management Settings

1. Search for and open *Device Manager*.
2. Click to expand *Universal Serial Bus Controllers*.
3. Right-click on *USB Root Hub* and select *Properties*.
4. Uncheck *Power Management* and click *OK*.
<div>
    <figure >
        <img src="../images/SDK-12.png"  width="260"/><img src="../images/SDK-13.png"  width="260"/>
    </figure>
</div>

### compileSdkVersion and releaseType of the app do not match the apiVersion and releaseType on the device

Reason: The compiled SDK version is higher than the actual device.

Solution:
Step 1: Modify build\_profile.json5 under entry and set apiType to faMode.
Step 2: Modify build\_profile.json5 under the project, change the compiled version to a lower version.
Run again, and the problem will be resolved.
   <img title="" src="../images/SDK-14.png" alt="" width="294">

### Install Failed

Have the device connected and detected by IDE, click on "run", the IDE gives the error messages:
"Install Failed : failed to install bundle. code: 9568289, error: install failed due to grant request permissions failed."

<img title="" src="../images/SDK-15.png" alt="" width="467">

It should be a permission issue, and now we need to identify the permissions causing the problem.

[This documentation](https://gitcode.com/openharmony/resources/blob/master/systemres/main/config.json) lists all permissions and their levels in OpenHarmony.

There are three types of permissions used in OpenHarmony for requests, ordered from low to high: normal, system\_basic, system\_core.

If the permission level is set to "availableLevel": "system\_basic", then you need to configure the acls field in the UnsignedReleasedProfileTemplate.json file and include the required high-level permissions in acls. The specific steps are as follows:

set the "profile" with p7b file generated from java \-c commands in build-profile.json5

<img title="" src="../images/SDK-16.png" >

With your first app created, previewed, run, debugged, and packaged, head to [Emulator](emulator.md) if you haven't already set one up, or back to [Workflow](workflow.md) for a closer look at the IDE itself.
