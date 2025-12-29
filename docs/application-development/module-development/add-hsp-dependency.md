### Overview

After completing the first step, a **dynamic shared package** has been created in our project. As shown below, the project now contains a main module (**entry**) and a dynamic shared package (**shared_library**).  

<div style="text-align:center">
    <img src='../images/image8.png'>
</div> 

If **entry** needs to access code or resources in **shared_library**, it must be linked to **shared_library**.  

There are 3 solutions to add dependencies:  

### Mapping Solution 1

Introduce the shared package in **entry** module `oh-package.json5` file under `dependencies` with the following format:

```bash
"mapped_directory_name": "file:../folder"
```  

- The **`folder`** refers to the shared package you created.  
- The **`mapped directory name`** is the name for the generated dependency kit, it must be the same as the name defined in `oh_package.json5` from the shared package.

<div style="text-align:center">
    <img src='../images/image9.png'>
</div>

You can place the mouse over the warning area to see a prompt, and then click Run `ohpm install`.

<div style="text-align:center">
    <img src='../images/image10.png'>
</div>

After installation, the shared package will be mapped into the `entry` module, under its `oh_modules` directory.  
<div style="text-align:center">
    <img src='../images/image11.png'>
</div>

### Mapping Solution 2  

Besides Mapping Solution 1, you can also use the following approach:

Introduce the shared package in **entry** module `oh-package.json5` file under `dependencies` with following format:

```bash
"@xx/mapped directory name": "file:../folder"
```  

The underlying mechanism is the same as in Method 1. The difference is that an `@xx` directory will be created under the `oh_modules` directory.

<div style="text-align:center">
    <img src='../images/image12.png'>
</div>

### Mapping Solution 3  

You can also install the dependencies through the command line.

**Navigate** to your `entry` module directory, and run the following command:  

```bash
ohpm install ../{mapped directory name}
```

<div style="text-align:center">
    <img src='../images/image13.png'>
</div>

After the command is executed, the dependency will be automatically added to `oh-package.json5`, and the mapping files will be generated.