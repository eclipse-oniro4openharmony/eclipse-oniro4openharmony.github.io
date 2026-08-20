# Interface Tour

DevEco Studio is built on the IntelliJ Platform, so if you have used Android Studio, WebStorm, or another JetBrains IDE, the layout will feel familiar.

## Welcome Screen

When no project is open, DevEco Studio shows the **Welcome** screen with:

* **New Project** – start a project from an OpenHarmony/HarmonyOS template (Empty Ability, Native C++, and more).
* **Open** – open an existing project directory.
* **Get from VCS** – clone a Git repository directly.
* A list of recently opened projects.
* A gear icon for **Settings/Preferences**, **Plugins**, and **SDK Manager** — useful because these are reachable even before a project is open.

## Main Window Layout

Once a project is open, the main window is split into the following regions:

| Region | Location | Purpose |
|---|---|---|
| Navigation bar | Top | Breadcrumb of the current file's path, run/debug configuration selector, run/debug/stop buttons |
| Project tool window | Left | File tree of the project (several views available, see below) |
| Editor | Center | Source files, resource files, previews |
| Tool window bar | Left/Right/Bottom edges | Icons to toggle tool windows such as Terminal, TODO, Problems |
| Status bar | Bottom | Encoding, line separator, current SDK/API level, background task progress |

!!! tip "Project view switcher"
    The dropdown at the top of the **Project** tool window (default label `Project`) lets you switch between several file tree presentations. The two used most often are:

    * **Project** – the raw directory structure on disk.
    * **Project Files** – filters out most build/IDE metadata so only source-relevant files remain.

## Key Tool Windows

| Tool window | Default shortcut | What it's for |
|---|---|---|
| Project | `Alt+1` | Browse and manage project files |
| Previewer | — (opens automatically for `.ets` pages) | Live rendering of the current ArkUI page |
| Terminal | `Alt+F12` | Embedded shell, useful for `ohpm` and `hdc` commands |
| Log / HiLog | — | Device/emulator log output, filterable by tag and level |
| Build | `Alt+0` | Output of Gradle-like Hvigor build tasks |
| Version Control | `Alt+9` | Git status, commit dialog, history, diff viewer |
| Device Manager | — | Create and launch emulators |
| Profiler | — | CPU, memory, network, and energy profiling of a running app |
| Problems | — | Aggregated inspection warnings/errors across the project |
| TODO | — | Collects `// TODO` comments across the codebase |

!!! note "Shortcuts differ per keymap"
    The shortcuts above use the default Windows/Linux keymap. macOS uses `Cmd` instead of `Ctrl` for most bindings. You can inspect or change any binding under **Settings → Keymap**.

## Essential Navigation Shortcuts

| Action | Shortcut |
|---|---|
| Search everywhere (files, classes, actions, settings) | `Shift` `Shift` (double Shift) |
| Go to file | `Ctrl+Shift+N` |
| Go to declaration/definition | `Ctrl+B` or Ctrl+Click |
| Find usages | `Alt+F7` |
| Show recent files | `Ctrl+E` |
| Reformat code | `Ctrl+Alt+L` |
| Optimize imports | `Ctrl+Alt+O` |

Getting comfortable with **Search Everywhere** (double `Shift`) is the single highest-leverage habit — it can open files, jump to a settings page, or run an IDE action without hunting through menus.

Next: learn what DevEco Studio actually generates for you in [Project Structure](project-structure.md).
