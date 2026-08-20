# Version Control

DevEco Studio includes the same built-in Git integration found across JetBrains IDEs, so you rarely need to leave the editor for everyday version control work.

## Enabling VCS for a Project

If a project wasn't cloned from Git, enable it via **VCS → Enable Version Control Integration** and choose Git. DevEco Studio then treats the project root as a Git repository and starts tracking file status.

## The Commit Tool Window

Open it with `Alt+9` or **View → Tool Windows → Commit**. It shows:

* Changed files, grouped by changelist (the default changelist is fine for most workflows).
* A diff preview for the selected file.
* A commit message box, plus **Commit** and **Commit and Push** buttons.

!!! tip "Review before committing"
    Click through each changed file's diff in the Commit window before committing — it's the same discipline as `git diff` before `git add`, just inline with the editor.

## Useful VCS Shortcuts

| Action | Shortcut |
|---|---|
| Open Commit tool window | `Alt+9` |
| Show local changes / diff | `Ctrl+D` (with a file selected) |
| Update project (pull) | `Ctrl+T` |
| Push | `Ctrl+Shift+K` |
| Show history for a file | Right-click file → Git → Show History |
| Annotate (blame) | Right-click gutter → Annotate with Git Blame |

## Branch Management

The branch indicator in the bottom-right status bar opens a menu to checkout, create, rename, or merge branches without a terminal. It also shows incoming/outgoing commit counts once you've fetched.

!!! note "Fetch vs. Update"
    **Update Project** (`Ctrl+T`) fetches and merges/rebases according to your configured settings in one step. If you only want to see what's changed remotely without touching your working tree yet, use **Git → Fetch** instead.

## Resolving Conflicts

When a merge/rebase produces a conflict, DevEco Studio opens a three-pane merge tool: your version, the result, and the incoming version, with per-block **Accept Yours/Theirs** actions plus manual editing of the result pane. Resolve each conflicting file this way, then mark the merge/rebase as continued from the VCS menu.

## Recommended `.gitignore`

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
    If a keystore or credentials file was ever committed before adding it to `.gitignore`, adding the ignore rule alone does not remove it from history. You'd need to purge it from history (e.g. with `git filter-repo`) and rotate the exposed credentials — treat anything committed as compromised.

## A Reasonable Day-to-Day Flow

1. Pull/update before starting work (`Ctrl+T`).
2. Make changes, using the Previewer and emulator/device as covered earlier in this tutorial to verify them.
3. Review the diff in the Commit tool window, write a clear message, and commit.
4. Push (`Ctrl+Shift+K`), or open a pull request from your Git hosting provider as your team's workflow dictates.

This wraps up the in-depth tutorial. Return to the [tutorial overview](index.md) for the full list of topics, or to the [Environment Setup Guide](../../index.md) for the rest of the setup documentation.
