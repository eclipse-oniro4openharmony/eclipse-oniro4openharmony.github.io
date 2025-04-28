---
layout: default
title: Resource Group Directories
parent: Resource Categories
grand_parent: Resource Categories and Access
nav_order: 2
---

## Resource Group Directories

Resource group directories include **element**, **media**, and **profile**, which are used to store resources of specific types.

  **Table 1** Resource group directories

| Directory   | Description                                    | Resource File                                    |
| --------- | ---------------------------------------- | ---------------------------------------- |
| element | Element resources. Each type of data is represented by a JSON file. (Only files are supported in this directory.) The options are as follows:<br>- **boolean**: boolean data<br>- **color**: color data<br>- **float**: floating point number ranging from -2^128 to 2^128<br>- **intarray**: array of integers<br>- **integer**: integer ranging from -2^31 to 2^31-1<br>- **pattern**: style (for system applications only) <br>- **plural**: plural form data<br>- **strarray**: array of strings<br>- **string**: string in the specified format.<br>- theme: theme (for system applications only) | It is recommended that files in the **element** subdirectory be named the same as the following files, each of which can contain only data of the same type:<br>- boolean.json<br>- color.json<br>- float.json<br>- intarray.json<br>- integer.json<br>- pattern.json<br>- plural.json<br>- strarray.json<br>- string.json |
| media   | Indicates media resources, including non-text files such as images, audios, and videos. (Only files are supported in this directory.)<br>Table 4 and Table 5 describe the types of images, audios, and videos.             | The file name can be customized, for example, **icon.png**.                    |
| profile  | Indicates a custom configuration file. (Only JSON files are supported in this directory.)      | The file name can be customized, for example, **test_profile.json**.          |

**Media Resource Types**

**Table 2** Image resource types

| Format  | File Name Extension|
| ---- | ----- |
| JPEG | .jpg  |
| PNG  | .png  |
| GIF  | .gif  |
| SVG  | .svg  |
| WEBP | .webp |
| BMP  | .bmp  |

**Table 3** Audio and video resource types

| Format                                  | File Name Extension        |
| ------------------------------------ | --------------- |
| H.264 AVC |.3gp |
| Baseline Profile (BP) | .mp4   |

**Resource File Examples**

The content of the **color.json** file is as follows:

```json
{
    "color": [
        {
            "name": "color_hello",
            "value": "#ffff0000"
        },
        {
            "name": "color_world",
            "value": "#ff0000ff"
        }
    ]
}
```

The content of the **float.json** file is as follows:

```json
{
    "float":[
        {
            "name":"font_hello",
            "value":"28.0fp"
        },
        {
            "name":"font_world",
            "value":"20.0fp"
        }
    ]
}
```

The content of the **string.json** file is as follows:

```json
{
    "string":[
        {
            "name":"string_hello",
            "value":"Hello"
        },
        {
            "name":"string_world",
            "value":"World"
        },
        {
            "name":"message_arrive",
            "value":"We will arrive at %1$s."
        },
        {
            "name":"message_notification",
            "value":"Hello, %1$s!,You have %2$d new messages."
        }
    ]
}
```

The content of the **plural.json** file is as follows:

```json
{
    "plural":[
        {
            "name":"eat_apple",
            "value":[
                {
                    "quantity":"one",
                    "value":"%d apple"
                },
                {
                    "quantity":"other",
                    "value":"%d apples"
                }
            ]
        }
    ]
}
```
