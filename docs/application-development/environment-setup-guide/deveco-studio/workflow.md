# Workflow

This page describes the IDE window layout, the files that DevEco Studio generates for a new project, editor tools that streamline daily development, and the built-in Git integration.

## Interface Tour

DevEco Studio is built on the IntelliJ Platform, so if you have used Android Studio, WebStorm, or another JetBrains IDE, the layout will feel familiar.

### Welcome Screen

When no project is open, DevEco Studio shows the **Welcome** screen with:

* **New Project** – start a project from an OpenHarmony/HarmonyOS template (Empty Ability, Native C++, and more).
* **Open** – open an existing project directory.
* **Get from VCS** – clone a Git repository directly.
* A list of recently opened projects.
* A gear icon for **Settings/Preferences**, **Plugins**, and **SDK Manager** — useful because these are reachable even before a project is open.

### Main Window Layout

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

### Key Tool Windows

| Tool window | Default shortcut | Purpose |
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

### Essential Navigation Shortcuts

| Action | Shortcut |
|---|---|
| Search everywhere (files, classes, actions, settings) | `Shift` `Shift` (double Shift) |
| Go to file | `Ctrl+Shift+N` |
| Go to declaration/definition | `Ctrl+B` or `Ctrl`+Click |
| Find usages | `Alt+F7` |
| Show recent files | `Ctrl+E` |
| Reformat code | `Ctrl+Alt+L` |
| Optimize imports | `Ctrl+Alt+O` |

**Search Everywhere** (double `Shift`) can open files, jump to a settings page, or run an IDE action without navigating through menus.

## Project Structure

When DevEco Studio creates a project from a template, it generates many files. Understanding their purposes makes manual configuration easier when a wizard is not available.

### Stage Model vs. FA Model

OpenHarmony applications can be built using one of two application models:

* **Stage model** — the current, recommended model. Introduces `AbilityStage`, a shared context across components, and a clearer separation between the application and its abilities. All new projects created by recent DevEco Studio versions default to this model.
* **FA model** (Feature Ability model) — the legacy model, kept mainly for compatibility with older codebases.

!!! tip
    Unless you are maintaining an existing FA-model project, always choose the Stage model for new work — most current documentation, samples, and APIs assume it.

### Top-Level Layout

A typical Stage-model project looks like this:

```text
MyApplication/
├── AppScope/
│   ├── app.json5
│   └── resources/
├── entry/
│   ├── src/
│   │   └── main/
│   │       ├── ets/
│   │       │   ├── entryability/
│   │       │   │   └── EntryAbility.ets
│   │       │   └── pages/
│   │       │       └── Index.ets
│   │       ├── resources/
│   │       │   ├── base/
│   │       │   ├── en_US/
│   │       │   └── rawfile/
│   │       └── module.json5
│   ├── build-profile.json5
│   └── oh-package.json5
├── build-profile.json5
├── oh-package.json5
└── hvigorfile.ts
```

#### AppScope

Settings that apply to the whole application, not just one module:

* `app.json5` — bundle name, vendor, version code/name, minimum/target/compatible API levels.
* `resources/` — app-wide resources such as the app icon and label, shared across all modules.

#### entry Module

`entry` is the default **module**. Most simple applications need only this module, while larger applications can add feature modules or shared libraries alongside it.

| File/Folder | Purpose |
|---|---|
| `src/main/ets/entryability/EntryAbility.ets` | The module's entry point (`UIAbility`); handles lifecycle callbacks like `onCreate`, `onWindowStageCreate` |
| `src/main/ets/pages/` | Your ArkUI page components (`.ets` files using `@Entry`/`@Component`) |
| `src/main/resources/base/` | Default resources (strings, colors, media) used when no more specific qualifier matches |
| `src/main/resources/en_US/`, `zh_CN/`, ... | Locale-specific resource overrides |
| `src/main/resources/rawfile/` | Raw assets bundled as-is, accessed by path rather than resource ID |
| `module.json5` | Module-level manifest: `deviceTypes`, abilities, requested permissions, module name/type |
| `build-profile.json5` | Module-level build configuration: target SDK/compile API, product flavors, signing config reference |
| `oh-package.json5` | Module's dependencies, similar in spirit to `package.json` |

!!! note "Where `deviceTypes` matters"
    If your app refuses to show up as a run target for a certain emulator (e.g. "phone" not listed), check `deviceTypes` in `module.json5` — this is the same issue documented in [Common Issues and Solutions](first-app.md#common-issues-and-solutions).

#### Project-Level Files

* `build-profile.json5` (root) — declares the products/targets and which modules/signing configs they use.
* `oh-package.json5` (root) — workspace-level dependency declarations and the `oh_modules` resolution behavior.
* `hvigorfile.ts` — the build script for **Hvigor**, OpenHarmony's build system (conceptually similar to a Gradle build script).

### Resource Qualifiers

Resource folders under `resources/` use qualifiers to target specific device configurations, for example:

```text
resources/
├── base/            # fallback, always present
├── en_US/            # locale
├── dark/             # color mode
└── phone/            # device type
```

DevEco Studio resolves the best-matching folder at build/runtime based on the current device's locale, color mode, screen density, and device type — you rarely need to write this logic yourself.

### Where the IDE Keeps Its Own State

Two folders are IDE/tooling-generated and should **not** be committed to version control:

| Folder | Contents |
|---|---|
| `.idea/` | Project-specific IDE settings (mostly machine-local) |
| `build/`, `.hvigor/` | Build outputs and Hvigor's cache |
| `oh_modules/` | Resolved dependencies (equivalent to `node_modules`) |

The [Version Control](#version-control) section below gives a ready-to-use `.gitignore` for these.

## Editor Features

DevEco Studio's editor is one of its strongest points: because it is built on the IntelliJ Platform, ArkTS/ArkUI code gets the same class of tooling that TypeScript and Java developers rely on daily.

### Code Completion

As you type, DevEco Studio suggests:

* Component names and their parameters (e.g. typing `Text(` shows the expected argument).
* Available `@State`/`@Prop`/`@Link` decorators for ArkUI component properties.
* Imports it can add automatically when you accept a suggestion from an unimported symbol.

!!! tip "Smart completion"
    `Ctrl+Shift+Space` narrows suggestions to those valid at the cursor, such as types assignable to the expected parameter. This is more useful than basic completion (`Ctrl+Space`) as a project grows.

### Navigating Code

| Action | Shortcut | Notes |
|---|---|---|
| Go to declaration | `Ctrl+B` / Ctrl+Click | Jumps to where a symbol is defined |
| Go to implementation | `Ctrl+Alt+B` | Useful for interfaces with multiple implementers |
| Find usages | `Alt+F7` | Lists every call site in a dedicated tool window |
| Show call hierarchy | `Ctrl+Alt+H` | Visualizes callers/callees of a function |
| Structure view | `Alt+7` | Outline of the current file's declarations |

### Refactoring

Refactoring tools rewrite code across the whole project consistently, not just in the current file:

* **Rename** (`Shift+F6`) — renames a symbol and updates every reference, including in resource files where applicable.
* **Extract Variable / Extract Function** — pulls a selected expression or block into a named variable/function.
* **Extract Component** — turns a chunk of ArkUI declarative UI code into its own reusable `@Component`, which is one of the most useful refactors once a page's `build()` method starts growing.
* **Safe Delete** — checks for remaining usages before deleting a declaration, refusing (or warning) if something still depends on it.

!!! warning "Review before committing a rename"
    Renames across resource strings or files referenced by relative paths are not always fully tracked. After a large rename, run a project-wide search (`Ctrl+Shift+F`) for the old name before committing.

### Inspections and Quick Fixes

The editor continuously analyzes your code and underlines potential issues:

* Red underline — compile errors (e.g. missing import, type mismatch).
* Yellow underline — warnings and style suggestions (e.g. unused variable).

Press `Alt+Enter` on a highlighted piece of code to see quick fixes — importing a missing symbol, adding a missing `@State`, or suppressing a specific inspection.

The **Problems** tool window provides an aggregated list of issues across the project, which is faster than inspecting every file after a large change.

### Live Templates

Live Templates are expandable code snippets. Type an abbreviation and press `Tab` to expand it into a boilerplate block you then fill in. You can inspect and add your own under **Settings → Editor → Live Templates**. This is worth doing for repeated ArkUI patterns you write often (a specific `@Component` skeleton, a common `Column`/`Row` layout, etc.).

### Formatting and Imports

| Action | Shortcut |
|---|---|
| Reformat code | `Ctrl+Alt+L` |
| Optimize imports (remove unused, sort) | `Ctrl+Alt+O` |
| Reformat and optimize on save | Enable under **Settings → Tools → Actions on Save** |

!!! tip "Team consistency"
    If multiple people work on the same project, agree on formatter settings (**Settings → Editor → Code Style**) early — reformatting churn on every commit makes diffs much harder to review.

## Version Control

DevEco Studio includes the same built-in Git integration found across JetBrains IDEs, so you rarely need to leave the editor for everyday version control work.

### Enabling VCS for a Project

If a project wasn't cloned from Git, enable it via **VCS → Enable Version Control Integration** and choose Git. DevEco Studio then treats the project root as a Git repository and starts tracking file status.

### The Commit Tool Window

Open it with `Alt+9` or **View → Tool Windows → Commit**. It shows:

* Changed files, grouped by changelist (the default changelist is fine for most workflows).
* A diff preview for the selected file.
* A commit message box, plus **Commit** and **Commit and Push** buttons.

!!! tip "Review before committing"
    Review each changed file in the Commit window before committing. This is equivalent to reviewing `git diff` before `git add`, but the diff is displayed in the editor.

### Useful VCS Shortcuts

| Action | Shortcut |
|---|---|
| Open Commit tool window | `Alt+9` |
| Show local changes / diff | `Ctrl+D` (with a file selected) |
| Update project (pull) | `Ctrl+T` |
| Push | `Ctrl+Shift+K` |
| Show history for a file | Right-click file → Git → Show History |
| Annotate (blame) | Right-click gutter → Annotate with Git Blame |

### Branch Management

The branch indicator in the lower-right status bar opens a menu for checking out, creating, renaming, or merging branches without a terminal. After you fetch, it also shows incoming and outgoing commit counts.

!!! note "Fetch vs. Update"
    **Update Project** (`Ctrl+T`) fetches and merges or rebases according to your configured settings. To inspect remote changes without modifying the working tree, use **Git → Fetch** instead.

### Resolving Conflicts

When a merge/rebase produces a conflict, DevEco Studio opens a three-pane merge tool: your version, the result, and the incoming version, with per-block **Accept Yours/Theirs** actions plus manual editing of the result pane. Resolve each conflicting file this way, then mark the merge/rebase as continued from the VCS menu.

### Recommended `.gitignore`

Several folders under a DevEco Studio project are either machine-local IDE state or fully regenerable build output, and should not be committed:

```gitignore
# Build output
build/
.hvigor/

# Dependency cache (regenerated from oh-package.json5)
oh_modules/

# IDE metadata
.idea/
*.iml

# Local, machine-specific config
local.properties

# Signing material — never commit real keystores or passwords
*.p12
*.jks
*.cer
*.p7b
```

!!! warning "Check history for secrets before pushing publicly"
    If a keystore or credentials file was committed before it was added to `.gitignore`, the ignore rule does not remove it from history. Purge it from history, for example with `git filter-repo`, and rotate the exposed credentials. Treat committed credentials as compromised.

### A Reasonable Day-to-Day Flow

1. Pull/update before starting work (`Ctrl+T`).
2. Make changes, using the Previewer and emulator/device to verify them (see [First App](first-app.md) and [Emulator](emulator.md)).
3. Review the diff in the Commit tool window, write a clear message, and commit.
4. Push (`Ctrl+Shift+K`), or open a pull request from your Git hosting provider as your team's workflow dictates.

With the layout, project files, editor, and VCS integration covered, move on to [First App](first-app.md) to actually build and run something.
