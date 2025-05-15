# Resource Directories

### base Directory

The **base** directory is provided by default. Under this directory, the **element** subdirectory stores basic elements such as strings, colors, and boolean values, and the **media** and **profile** subdirectories store resource files such as media, animations, and layouts.<br>
Resource files in the subdirectories are compiled into binary files, and each resource file is assigned an ID. Resource files in the subdirectory are referenced based on the resource type and resource name.

### Qualifiers Directory

**en_US** and **zh_CN** are two default qualifiers directories. You need to create other qualifiers directories on your own. Under this directory, the subdirectories store basic elements such as strings, colors, and boolean values, as well as resource files such as media, animations, and layouts.<br>Resource files in the subdirectories are compiled into binary files, and each resource file is assigned an ID. Resource files in the subdirectories are referenced based on the resource type and resource name.

### rawfile Directory

You can create multiple levels of subdirectories with custom names to store various resource files.<br>Resource files in the subdirectories are directly packed into the application without being compiled, and no IDs will be assigned to the resource files. The subdirectories are referenced based on the specified file path and file name.
