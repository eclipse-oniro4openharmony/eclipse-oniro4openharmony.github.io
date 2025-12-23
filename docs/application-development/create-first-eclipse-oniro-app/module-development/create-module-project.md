### Overview

Before you can implement modular architecture in your application, you need to set up a project that supports module development. This section guides you through creating a new project from scratch and understanding its structure.

**1.** Choose `Empty Ability` as project template and proceed.  
<div style="text-align:center">
    <img src='../images/image2.png'>
</div> 

**2.** In this tutorial we will seperate modules for `wearable` and `mobile` application. Make sure you have selected `Phone` and `Wearable` in `Device type` section.  
<div style="text-align:center">
    <img src='../images/image3.png'>
</div> 

In the current project, right-click the project name, select `New`, and then choose Module.  
<div style="text-align:center">
    <img src='../images/image4.png'>
</div> 

By default, there are two shared package templates available: one for dynamic shared packages and one for static shared packages, as shown in the figure below.  
<div style="text-align:center">
    <img src='../images/image5.png'>
</div> 


Set the shared package name and click **Finish**. Here I choose `Shared Library` in this tutorial.  
<div style="text-align:center">
    <img src='../images/image6.png'>
</div> 

After the dynamic shared package is created, its structure is shown below, with the **type** set to `shared`.
If you create a static shared package, the **type** here will be `har`.  
<div style="text-align:center">
    <img src='../images/image7.png'>
</div> 