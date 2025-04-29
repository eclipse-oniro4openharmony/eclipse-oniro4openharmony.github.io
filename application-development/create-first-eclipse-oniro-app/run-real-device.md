---
title: Running the Application on a Real Device
parent: Create Your First Eclipse Oniro Application
grand_parent: Application Development
nav_order: 7
layout: default
---

## Running the Application on a Real Device
### Install HDC  
1. Find the [pipeline](https://ci.openharmony.cn/workbench/cicd/dailybuild/dailylist) whose name is ohos-sdk-full or ohos-sdk-public, click Download Link, and select Full Package.

>**Note:**
You can skip this part if you followed the procedure from [Environment Setup and Configuration](/application-development/environment-setup-config) tutorial. 

Use conditional filtering, such as selecting the project as openharmony, selecting the target branch OpenHarmony-4.1-Release, selecting a date from the previous month, or manually choosing a range.  
   
In the daily build or rolling build, find **ohos-sdk-full_4.1-Release**, and click on the download link to choose and download the full package, which includes Full-SDK for Windows and Linux.  (If daily build SDK is not compatible with your version of DevEco Studio, try to use rolling build SDK instead)  
<img src='./images/image39.png'>  

2. Under `toolchain` folder, find `hdc.exe` and `libusb_shared.ddl`.
<div style="text-align:center">
    <img src='./images/image29.png'>
</div> 

3. Create a folder called `hdc_bin`, you can create it wherever you like and put `hdc.exe` and `libusb_shared` into that folder.
<div style="text-align:center">
    <img src='./images/image30.png'>
</div> 

4. Add **Environment Variable**
- Open `Settings` on Windows system, type `environment` to search `Edit the system environment variables` and click it.
<div style="text-align:center">
    <img src='./images/image31.png'>
</div> 

- Make sure the `System Properties` window is under `Advanced` tab, click `Environment Variables...`
<div style="text-align:center">
    <img src='./images/image32.png'>
</div> 

- Double click `Path` in `System variables` area. Click `New` on new pop-up window and paste your `hdc_bin` folder path.  
After that, click `OK` for all windows. 
<div style="text-align:center">
    <img src='./images/image33.png'>
</div> 

- Check whether the HDC is running properly
You can open your `Command prompt` and type `hdc` to check.
<div style="text-align:center">
    <img src='./images/image34.png'>
</div> 

### Use real machine to run application with USB  
1. Connect the development board(Here I used `HiHope HH-SCDAYU200 Development Kit`) running the OpenHarmony standard system to the computer and you can find the running device on the top part of the IDE.
<div style="text-align:center">
    <img src='./images/image36.png'>
</div> 

2. Generate signature. 
- Click `Project Structure...` icon on the top-right corner of the IDE, Choose `Project > Signing Configs` and select `Automatically generate signature`. 
- Click `Apply` and wait until the automatic signing is complete.
<div style="text-align:center">
    <img src='./images/image28.png'>
</div> 

- You can find signed signature in `configuration` folder and open `build-profile.json5` file.

<div style="text-align:center">
    <img src='./images/image35.png'>
</div> 

3. Click `Run 'entry'` triangle button.
<div style="text-align:center">
    <img src='./images/image37.png'>
</div> 

4. You can observe the application running on the board.
<div style="text-align:center">
    <img src='./images/image38.png' width="50%">
</div> 

You learned about DevEco Studio and built your first Eclipse Oniro Application, congratulations!