To create a project in DevEco Studio:

**1.** Double-click the DevEco Studio icon to begin.
<div style="text-align:center">
    <img src='../images_common/image1.png' width='30%'>
</div>

**2.** On the welcome page, click **Create Project**.
<div style="text-align:center">
    <img src='../images_common/image2.png'>
</div>

The **Create Project** window displays the templates provided by DevEco Studio.
<div style="text-align:center">
    <img src='../images_common/image3.png'>
</div>

In DevEco Studio, a project template provides the structure, essential files, and starter code for a specific type of application.

**3.** Select the **Application** tab in the left sidebar, choose **Empty Ability** as the project template, and click **Next**.

**4.** Configure the project with the following information:

- The **Project name** field is used to enter the name of your project.

- The **Bundle name** field contains the package name, which also serves as the default application ID. Keep the default value to retain the generated file organization.

- The **Save location** field specifies where the project files are stored. You can keep the default value.

- The **Compatible SDK** field specifies the oldest API version on which the application can run. Select the version that matches your target: **6.1 (API 23)** for the Oniro emulator, for example. The screenshot below was captured with a 5.1 (API 18) SDK.

- Select **Stage** for **Model** and keep the default values for all other parameters.

<div style="text-align:center">
    <img src='../images_common/image5.png'>
</div>  

**5.** Click **Finish** and wait for DevEco Studio to create the project.

<div style="text-align:center">
    <img src='../images_common/image6.png'>
</div> 

**6.** Recent DevEco Studio versions create a HarmonyOS project from this template. To target Oniro/OpenHarmony, open the project-level `build-profile.json5` file (next to the `entry` directory) and ensure that the selected product contains the OpenHarmony SDK values. For OpenHarmony 6.1, use:

```json
"products": [
  {
    "name": "default",
    "signingConfig": "default",
    "compileSdkVersion": 23,
    "compatibleSdkVersion": 23,
    "runtimeOS": "OpenHarmony"
  }
]
```

Keep any other product options that DevEco Studio generated. Click **Sync Now** after editing. If Sync Check offers to replace HarmonyOS-specific device types with OpenHarmony's `default` type, accept that change.

!!! note
     If the target is a HarmonyOS device such as the HUAWEI WATCH 5 rather than an Oniro/OpenHarmony device, do not change `runtimeOS` to `OpenHarmony`; use the SDK and runtime that match that device.

**7.** Click **Previewer** on the right sidebar of DevEco Studio to view both the code and design simultaneously.

<div style="text-align:center">
    <img src='../images_mobile/image7.png'>
</div> 

- **Project view** (Part 1): Displays all files and folders in your project.
- **Code view** (Part 2): Provides the workspace for editing code.
- **Design view** (Part 3): Displays a preview of the application's design.
