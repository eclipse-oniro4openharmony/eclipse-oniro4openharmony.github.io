---
title: Devtool
parent: Contribution
layout: default
---

**Devtool** is a tool available on OpenEmbedded that allows
you to start development with your OpenEmbedded distribution. This
command tool is available in addition to the bitbake command. The
**Devtool** command is an important component of the SDK\'s
extensibility. Run `devtool --help` to view the devtool help commands.

This tool commands allows to:

- devtool add: Assists in the development of a new recipe to build a
  specified source tree.
- devtool modify: Sets up the build environment to modify the source
  for an existing recipe.
- devtool upgrade: Upgrades an existing recipe to a new upstream
  version.

For more information on using devtool in your sdk workflow, see [Use the
Extensible
SDK](https://www.yoctoproject.org/docs/3.1.4/mega-manual/mega-manual.html#using-devtool-in-your-sdk-workflow).

# Adding a New Recipe

To add a new recipe [busybox]{.title-ref} to the workspace layer,
perform the following procedure:

1.  Add a new recipe [busybox]{.title-ref} to the workspace to build a
    specified source tree, execute:

> ```console
> $ devtool add busybox mysources/busybox
> ```
>
> The above example creates and adds a new recipe named
> [busybox]{.title-ref} to the workspace layer.
>
> **NOTE：**
>
> The `devtool add` command creates the workspace layer when you add a
> recipe and the workspace layer does not exist.

2.  Edit the source code file and commit the changes to your workspace,
    execute:

> ```console
> $ devtool edit-recipe busybox
> ```
>
> Executing the above command opens the default editor (as specified by
> the editor variable) on the specified recipe.

3.  Build your recipe [busybox]{.title-ref} from the workspace, execute:

    ```console
    $ devtool build busybox
    ```

4.  Deploy the recipe [busybox]{.title-ref} build output to test on the
    live target machine, execute:

    ```console
    $ devtool deploy-target busybox root@<ip of board>
    ```

5.  Populate the workspace layer with your new recipe in the
    [\<WORKSPACE_LAYER_PATH\> /workspace/sources]{.title-ref} directory.

# Modifying an Existing Recipe

To modify a new recipe [busybox]{.title-ref} to the workspace layer,
perform the following procedure:

1.  Extract the source files for an existing recipe, execute:

> ```console
> $ devtool modify busybox mysources/busybox
> ```

2.  Edit the code and commit your changes to your local git repository.

    You can use any editor to make the changes and save your source code
    modifications.

3.  Build your recipe [busybox]{.title-ref} from the workspace, execute:

    ```console
    $ devtool build busybox
    ```

4.  Deploy the recipe [busybox]{.title-ref} build output to test on the
    live target machine, execute:

    ```console
    $ devtool deploy-target busybox root@<ip of board>
    ```

> **NOTE：** Use command `devtool undeploy-target busybox root@IP` to undeploy and edit the recipe source file again.

5.  Apply the changes from the external source tree to a recipe and
    creates a patch for the committed changes, execute:

    ```console
    $ devtool update-recipe busybox
    ```

    The above devtool command allows the changes to be exported as
    patches and adds to the recipe. For more information on patching the
    source for a recipe, see [Patch the source for a
    recipe](https://wiki.yoctoproject.org/wiki/TipsAndTricks/Patching_the_source_for_a_recipe).

6.  Use the devtool reset command to remove a recipe and its
    configuration from the workspace layer.

> ```console
> $ devtool reset busybox
> ```

# Upgrading an Existing Recipe

The devtool upgrade command upgrades an existing recipe to that of a
more up-to -date version found upstream. You can use the devtool upgrade
workflow to make sure the recipes you are using for builds are
up-to-date with their upstream counterparts.

To upgrade a new recipe [busybox]{.title-ref} to the workspace layer,
perform the following procedure:

1.  Upgrade an existing recipe to a new upstream version, execute:

> ```console
> $ devtool upgrade busybox
> ```
>
> 💡 **NOTE：** Execute `devtool upgrade busybox --version <version to upgrade> --no-patch` command to upgrade the recipe to the upstream version without applying patches from the recipe to the new source code.

2.  Push the source code changes or write as patches on top of the
    recipe, execute:

    ```console
    $ devtool update-recipe busybox
    ```

3.  Build your recipe [busybox]{.title-ref} from the workspace, execute:

    ```console
    $ devtool build busybox
    ```

4.  Deploy the recipe [busybox]{.title-ref} build output to test on the
    live target machine, execute:

    ```console
    $ devtool deploy-target busybox root@<ip of board>
    ```

> 💡 **NOTE：** Use command `devtool undeploy-target busybox root@IP` to undeploy and edit the recipe source file again.

5.  Check the upgrade status of the recipe [busybox]{.title-ref},
    execute:

    ```console
    $ devtool check-upgrade-status -h
    ```
