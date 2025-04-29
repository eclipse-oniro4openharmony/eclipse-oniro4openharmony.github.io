---
layout: default
title: Resource Access
parent: Resource Categories and Access
grand_parent: Application Development
nav_order: 2
---

## Resource Access

- [HAP Resources](#hap-resources)  
- [Cross-HAP/HSP Resources](#cross-haphsp-resources)  
- [System Resources](#system-resources)  

### HAP Resources

 - Use **$r** or **$rawfile** to reference resources.<br>To reference resources of the color, float, string, plural, media, or profile type, use the "$r('app.type.name')" format, where **app** indicates the resource defined in the **resources** directory, **type** indicates the resource type or resource save path, and **name** indicates the name you assign to the resource.<br>To reference strings with multiple placeholders in the **string.json** file, use the "$r('app.string.label','aaa','bbb',444)" format.<br>To reference resources in the **rawfile** subdirectory, use the "$rawfile('filename')" format. Wherein **filename** indicates the relative path of a file in the **rawfile** subdirectory, which must contain the file name extension and cannot start with a slash (/).

The usage is as follows:

  ```typescript
    Text('Hello')
    .fontColor($r('sys.color.ohos_id_color_emphasize'))
    .fontSize($r('sys.float.ohos_id_text_size_headline1'))
    .fontFamily($r('sys.string.ohos_id_text_font_family_medium'))
    .backgroundColor($r('sys.color.ohos_id_color_palette_aux1'))

    Image($r('sys.media.ohos_app_icon'))
    .border({
      color: $r('sys.color.ohos_id_color_palette_aux1'),
      radius: $r('sys.float.ohos_id_corner_radius_button'), width: 2
    })
    .margin({
      top: $r('sys.float.ohos_id_elements_margin_horizontal_m'),
      bottom: $r('sys.float.ohos_id_elements_margin_horizontal_l')
    })
    .height(200)
    .width(300)
  ```

- Obtain a **ResourceManager** object through the application context, and then call resource management APIs to access different resources.<br>For example, call **getContext.resourceManager.getStringByNameSync('app.string.XXX')** to obtain string resources; call **getContext.resourceManager.getRawFd('rawfilepath')** to obtain the descriptor of the HAP where the raw file is located, and then use the descriptor ({fd, offset, length}) to access the raw file.

### Cross-HAP/HSP Resources
For cross-HAP/HSP resources, we have different scenarios as following:
- Cross-bundle access (for system applications only)
- Inter-bundle, cross-module access  

For detailed information please reference [Inter-Bundle, Cross-Module Access](https://docs.openharmony.cn/pages/v5.0/en/application-dev/quick-start/resource-categories-and-access.md#cross-bundle-access-for-system-applications-only)

### System Resources

Apart from custom resources, developers can obtain the ID and configuration-specific values of colors, fonts, or other resources in [Resources](https://gitee.com/openharmony/docs/blob/master/en/design/ux-design/design-resources.md) and system icons in [HarmonyOS Symbol](https://developer.huawei.com/consumer/cn/design/harmonyos-symbol/).

During development, the usage of layered parameters is basically the same as that of qualifiers. To reference a system resource, use the "$r('sys.type.resource_id')" format. Wherein: **sys** indicates a system resource; **type** indicates the resource type, which can be **color**, **float**, **string**, or **media**; **resource_id** indicates the resource ID.

> **NOTE**
>
> - The use of system resources is only supported in the declarative development paradigm.
>
> - For preset applications, you are advised to use system resources. For third-party applications, you can choose to use system resources or custom application resources as required.

  ```ts
    Text('Hello')
    .fontColor($r('sys.color.ohos_id_color_emphasize'))
    .fontSize($r('sys.float.ohos_id_text_size_headline1'))
    .fontFamily($r('sys.string.ohos_id_text_font_family_medium'))
    .backgroundColor($r('sys.color.ohos_id_color_palette_aux1'))

    Image($r('sys.media.ohos_app_icon'))
    .border({
      color: $r('sys.color.ohos_id_color_palette_aux1'),
      radius: $r('sys.float.ohos_id_corner_radius_button'), width: 2
    })
    .margin({
      top: $r('sys.float.ohos_id_elements_margin_horizontal_m'),
      bottom: $r('sys.float.ohos_id_elements_margin_horizontal_l')
    })
    .height(200)
    .width(300)
  ```