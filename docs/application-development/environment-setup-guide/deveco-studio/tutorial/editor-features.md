# Editor Features

DevEco Studio's editor is one of its strongest points: because it is built on the IntelliJ Platform, ArkTS/ArkUI code gets the same class of tooling that TypeScript and Java developers rely on daily.

## Code Completion

As you type, DevEco Studio suggests:

* Component names and their parameters (e.g. typing `Text(` shows the expected argument).
* Available `@State`/`@Prop`/`@Link` decorators for ArkUI component properties.
* Imports it can add automatically when you accept a suggestion from an unimported symbol.

!!! tip "Smart completion"
    `Ctrl+Shift+Space` narrows suggestions to what's actually valid at the cursor (e.g. only types assignable to the expected parameter), which is more useful than basic completion (`Ctrl+Space`) once a project grows.

## Navigating Code

| Action | Shortcut | Notes |
|---|---|---|
| Go to declaration | `Ctrl+B` / Ctrl+Click | Jumps to where a symbol is defined |
| Go to implementation | `Ctrl+Alt+B` | Useful for interfaces with multiple implementers |
| Find usages | `Alt+F7` | Lists every call site in a dedicated tool window |
| Show call hierarchy | `Ctrl+Alt+H` | Visualizes callers/callees of a function |
| Structure view | `Alt+7` | Outline of the current file's declarations |

## Refactoring

Refactoring tools rewrite code across the whole project consistently, not just in the current file:

* **Rename** (`Shift+F6`) — renames a symbol and updates every reference, including in resource files where applicable.
* **Extract Variable / Extract Function** — pulls a selected expression or block into a named variable/function.
* **Extract Component** — turns a chunk of ArkUI declarative UI code into its own reusable `@Component`, which is one of the most useful refactors once a page's `build()` method starts growing.
* **Safe Delete** — checks for remaining usages before deleting a declaration, refusing (or warning) if something still depends on it.

!!! warning "Review before committing a rename"
    Renames across resource strings or files referenced by relative path aren't always fully tracked. After a large rename, run a project-wide search (`Ctrl+Shift+F`) for the old name before committing, just to be safe.

## Inspections and Quick Fixes

The editor continuously analyzes your code and underlines potential issues:

* Red underline — compile errors (e.g. missing import, type mismatch).
* Yellow underline — warnings and style suggestions (e.g. unused variable).

Press `Alt+Enter` on a highlighted piece of code to see quick fixes — importing a missing symbol, adding a missing `@State`, or suppressing a specific inspection.

The full, aggregated list of issues across the project is available in the **Problems** tool window, which is often faster than hunting file by file after a large change.

## Live Templates

Live Templates are expandable code snippets. Type an abbreviation and press `Tab` to expand it into a boilerplate block you then fill in. You can inspect and add your own under **Settings → Editor → Live Templates**. This is worth doing for repeated ArkUI patterns you write often (a specific `@Component` skeleton, a common `Column`/`Row` layout, etc.).

## Formatting and Imports

| Action | Shortcut |
|---|---|
| Reformat code | `Ctrl+Alt+L` |
| Optimize imports (remove unused, sort) | `Ctrl+Alt+O` |
| Reformat and optimize on save | Enable under **Settings → Tools → Actions on Save** |

!!! tip "Team consistency"
    If multiple people work on the same project, agree on formatter settings (**Settings → Editor → Code Style**) early — reformatting churn on every commit makes diffs much harder to review.

Next: see your ArkUI pages rendered live without a device in [Using the Previewer](previewer.md).
