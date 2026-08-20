# Project Structure

When DevEco Studio creates a new project from a template, it generates a fair number of files. Understanding what each one does will save you a lot of confusion later, especially when something needs to be configured by hand instead of through a wizard.

## Stage Model vs. FA Model

OpenHarmony applications can be built using one of two application models:

* **Stage model** — the current, recommended model. Introduces `AbilityStage`, a shared context across components, and a clearer separation between the application and its abilities. All new projects created by recent DevEco Studio versions default to this model.
* **FA model** (Feature Ability model) — the legacy model, kept mainly for compatibility with older codebases.

!!! tip
    Unless you are maintaining an existing FA-model project, always choose the Stage model for new work — most current documentation, samples, and APIs assume it.

## Top-Level Layout

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

### AppScope

Settings that apply to the whole application, not just one module:

* `app.json5` — bundle name, vendor, version code/name, minimum/target/compatible API levels.
* `resources/` — app-wide resources such as the app icon and label, shared across all modules.

### entry Module

`entry` is the default **module** created for you — most simple apps only ever need this one module. Larger apps can add further modules (feature modules, shared libraries) alongside it.

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
    If your app refuses to show up as a run target for a certain emulator (e.g. "phone" not listed), check `deviceTypes` in `module.json5` — this is the same issue documented in [Common Issues and Solutions](../common-issue.md).

### Project-Level Files

* `build-profile.json5` (root) — declares the products/targets and which modules/signing configs they use.
* `oh-package.json5` (root) — workspace-level dependency declarations and the `oh_modules` resolution behavior.
* `hvigorfile.ts` — the build script for **Hvigor**, OpenHarmony's build system (conceptually similar to a Gradle build script).

## Resource Qualifiers

Resource folders under `resources/` use qualifiers to target specific device configurations, for example:

```text
resources/
├── base/            # fallback, always present
├── en_US/            # locale
├── dark/             # color mode
└── phone/            # device type
```

DevEco Studio resolves the best-matching folder at build/runtime based on the current device's locale, color mode, screen density, and device type — you rarely need to write this logic yourself.

## Where the IDE Keeps Its Own State

Two folders are IDE/tooling-generated and should **not** be committed to version control:

| Folder | Contents |
|---|---|
| `.idea/` | Project-specific IDE settings (mostly machine-local) |
| `build/`, `.hvigor/` | Build outputs and Hvigor's cache |
| `oh_modules/` | Resolved dependencies (equivalent to `node_modules`) |

The [Version Control](version-control.md) page later in this tutorial gives a ready-to-use `.gitignore` for these.

Next: see how the editor helps you write ArkTS/ArkUI code faster in [Editor Features](editor-features.md).
