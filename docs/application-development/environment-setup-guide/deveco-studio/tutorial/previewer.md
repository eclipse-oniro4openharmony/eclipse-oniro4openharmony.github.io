# Using the Previewer

The **Previewer** renders your ArkUI pages without needing an emulator or a physical device, which makes it the fastest feedback loop while building UI.

## Opening the Previewer

Open any page under `entry/src/main/ets/pages/` (a file with an `@Entry @Component struct` declaration). The Previewer panel should appear automatically, usually docked to the right of the editor. If it doesn't:

1. Click inside the `.ets` file so it has focus.
2. Look for the **Previewer** tab along the tool window bar, or use **View → Tool Windows → Previewer**.

!!! note "First render can be slow"
    The first preview of a session compiles the module, so it can take noticeably longer than subsequent updates. Later edits generally re-render in a second or two.

## Live Updates

With the Previewer open, most changes to the page's `build()` method are reflected as soon as you save (or immediately, if **Auto-save** is enabled). This works for:

* Layout and styling changes (padding, colors, alignment).
* Adding/removing components.
* Changes to `@State` initial values.

It does **not** reliably reflect:

* Behavior driven by native code or platform APIs unavailable in the preview sandbox.
* Runtime logic that depends on a real network call, file system, or sensor.
* Some animations and gesture-driven interactions — verify those on an emulator or device.

## Multi-Device Preview

Click the device selector above the Previewer canvas to render the same page across several device profiles at once — phone, tablet, foldable, and wearable, for instance. This is the fastest way to catch layout breakage on smaller or larger screens before you ever touch an emulator.

!!! tip
    Keep at least one small-screen and one large-screen profile enabled by default for any page with non-trivial layout — most responsive-layout bugs show up immediately in this comparison view.

## Interactive Preview

By default, the Previewer is a static snapshot of the page's initial render. Switching to **Interactive Preview** mode (button above the canvas) lets you click, tap, and scroll inside the rendered page as if it were running on a device — useful for checking simple state changes (toggles, tab switches, list scrolling) without a full deploy.

## Previewer Settings

Under **Settings → Languages & Frameworks → ArkUI Previewer** (path may vary slightly by version) you can adjust:

* Which device profiles are shown by default.
* Whether the Previewer refreshes automatically or only on manual trigger.
* Rendering scale, useful on high-DPI displays where the default preview looks too large or small.

## When to Stop Trusting the Previewer

The Previewer is a productivity tool, not a substitute for testing on a real target. Always validate on an emulator or device (see [Emulators and Devices](emulators-and-devices.md)) before considering a feature done, especially anything touching permissions, sensors, background tasks, or performance.

Next: set up something closer to a real target in [Emulators and Devices](emulators-and-devices.md).
