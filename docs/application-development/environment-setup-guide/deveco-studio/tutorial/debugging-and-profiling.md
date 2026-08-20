# Debugging and Profiling

Running your app is only half the job — DevEco Studio's debugger and profiler are what let you find out *why* something is wrong.

## Starting a Debug Session

Select a run target (see [Emulators and Devices](emulators-and-devices.md)), then click the **Debug** icon (bug shape) instead of **Run** in the navigation bar. The app launches the same way, but now stops at breakpoints and lets you inspect state.

## Breakpoints

* Click in the gutter to the left of a line number to set a line breakpoint (a red dot appears).
* Right-click a breakpoint to configure it further:
    * **Condition** — only stop when an expression evaluates to true, e.g. `index == 3`. Useful inside loops where you only care about one iteration.
    * **Log message** — print a message (optionally including expression values) without actually pausing execution. This effectively gives you a temporary, zero-recompile `console.log` you can remove later without touching source.
    * **Suspend policy** — stop only the current thread or the whole process.

## While Paused

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

## HiLog

The **Log** (HiLog) tool window streams the device/emulator's system log. Two filters make it usable on a busy device:

* **Tag filter** — match the tag used in your `hilog` calls (e.g. a per-module tag you define).
* **Log level** — restrict to `WARN`/`ERROR` when hunting a crash, or `DEBUG`/`INFO` during normal iteration.

You can also save a filter configuration so you don't have to re-type it every session.

## Profiler

Open **View → Tool Windows → Profiler** (or the toolbar icon) while the app is running to attach the profiler. It has separate tabs:

| Tab | What it shows |
|---|---|
| CPU | Method-level call tree and flame chart during a recorded trace, to find hot functions |
| Memory | Live heap size, allocation tracking, and the ability to trigger/inspect a heap snapshot to hunt leaks |
| Network | Individual requests, timing, and payload size for HTTP(S) traffic made by the app |
| Energy (where available) | Coarse indicators of what's driving power usage (radio, CPU, GPS) |

!!! note "Profiling overhead"
    Recording detailed CPU traces or full allocation tracking adds overhead and will skew timings somewhat. Use a lighter sampling mode first to find the general area of a problem, then a detailed trace to zoom in.

## A Practical Workflow

1. Reproduce the issue once without any tooling attached, so you know what "broken" looks like.
2. If it's a logic bug, set a conditional breakpoint close to the suspected cause and step from there.
3. If it's a performance issue, record a CPU trace covering just the slow interaction, not the whole session — shorter traces are far easier to read.
4. If it's a crash under load or over time, check Memory for steadily growing retained size across repeated actions — a classic leak signature.

Next: once the app behaves correctly, prepare it for distribution in [Build Variants and Signing](build-and-signing.md).
