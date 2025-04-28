---
layout: default
title: Resource Categories
parent: Resource Categories and Access
grand_parent: Application Development
nav_order: 1
has_children: true
---

## Resource Categories

Resource files used during application development must be stored in specified directories for management. There are two types of resource directories, namely, **resource directories** and **resource group directories**. 
The resource directories are the **base**, **qualifiers**, **rawfile** directories. The resource group directories are the **element**, **media**, and **profile** directories.

> **NOTE**
>
> The common resource files used across projects in the stage model are stored in the **resources** directory under **AppScope**.

Example of the **resources** directory:
```
resources
|---base
|   |---element
|   |   |---color.json
|   |   |---string.json
|   |---media
|   |   |---icon.png
|   |   |---startIcon.png
|   |---profile
|   |   |---main_pages.json
|---en_US  // Default directory. When the device language is en-us, resources in this directory are preferentially matched.
|   |---element
|   |   |---string.json
|---rawfile // Other types of files are saved as raw files and will not be integrated into the resources.index file. You can customize the file name as needed.
|---zh_CN  // Default directory. When the device language is zh-cn, resources in this directory are preferentially matched.
|   |---element
|   |   |---string.json
|---module.json5
```
