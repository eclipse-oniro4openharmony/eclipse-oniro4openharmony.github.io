---
title: Create an Eclipse Oniro app using OpenHarmony Template
parent: Create Your First Eclipse Oniro Application
grand_parent: Application Development
nav_order: 1
layout: default
---

## Create an Eclipse Oniro app using OpenHarmony Template 
To create a project in DevEco Studio:

1. Double-click the DevEco Studio icon to begin.
<div style="text-align:center">
    <img src='./images/image1.png' width='30%'>
</div>

2. In welcome page click `Create Project`.
<div style="text-align:center">
    <img src='./images/image2.png'>
</div>

The **Create Project** window opens with template market provided by DevEco Studio.
<div style="text-align:center">
    <img src='./images/image3.png'>
</div>

In DevEco Studio, a project template serves as a blueprint for building specific types of apps. It defines the project structure and essential files, providing starter code to help you get started quickly.

3. Make sure the `Application` tab is selected on the left bar, then choose `[OpenHarmony]Empty Ability` as project template and click `Next`.
<div style="text-align:center">
    <img src='./images/image4.png'>
</div>

4. Configure your project with following information:  

- The **Project name** field is used to enter the name of your project.

- The **Bundle name** field represents the package name, which also serves as the default application ID. Keep it as default to maintain the file organization.

- The **Save location** field specifies where all project files are stored, you can leave it as default.  

- The **Compile SDK** specifies the API version used for compilation. By default, **API 11** is selected in our case, which also sets **ArkTS** as the development language.  

- Select **Stage** for **Model** and keep the default values for all other parameters.

>**Note:**
 If you plan to use **JavaScript**, select **API 8** instead. This will provide a language option where you can choose **JS**.

<div style="text-align:center">
    <img src='./images/image5.png'>
</div>  

5. Click `Finish` and wait for the project creation.

<div style="text-align:center">
    <img src='./images/image6.png'>
</div> 

6. Click **Previewer** on the right sidebar of DevEco Studio to view both the code and design simultaneously.

<div style="text-align:center">
    <img src='./images/image7.png'>
</div> 

- **Project View**(Part 1): Displays all files and folders in your project.  
- **Code View**(Part 2): The workspace for editing code.  
- **Design View**(Part 3): Allows you to preview your app’s design.