Perfect! You already familar with basic knowledge about module development, let's jump into a step-by-step module development tutorial. In this example, you'll see why module development is so useful.

### Step 1: Initialize new project

Let's create a new project and select `Phone` as device type.

<div style="text-align:center">
    <img src='../images/image26.png'>
</div> 

Then, right click project root directory and select create new module.

We create 2 modules in this tutorial.
- Select `Empty Ability` and create **wearable** application.  
<div style="text-align:center">
    <img src='../images/image27.png'>
</div>   

- Select `Shared Library` and create **shared_library** module.
<div style="text-align:center">
    <img src='../images/image28.png'>
</div>   

!!!info
    In this tutorial, we are using one shared module in two different devices.
    While create application for another device, make sure the new module must select `entry` as module type and select one unique `Device type` in your project.  
    For shared dynamic library, we choose `Phone` and `Wearble` as device type since we want to use this library in both applications.

### Step 2: Define shared resources
We define a simple **Log** class in `shared_library` module, and export **Log** from its `Index.ets` page.
<div style="text-align:center">
    <img src='../images/image29.png'>
</div>   

And also, we can also use resources under `AppScope`.
<div style="text-align:center">
    <img src='../images/image30.png'>
</div>   

### Step 3: Add HSP dependency
Navigate to your application `entry` level folder in terminal and run the following command:  

*Here I rename `entry` module into `mobile`*

```bash
ohpm install ../shared_library
```  
<div style="text-align:center">
    <img src='../images/image31.png'>
</div>   

### Step 4: Use shared resouces  
We customize `index.ets` page in `mobile` and `wearable` applications which `Log` class is from `shared_library` module and string resource `public_resource` is from `AppScope` resouce folder.

<div style="text-align:center">
    <img src='../images/image32.png'>
</div>   

### Step 5: Compile and run
Select `mobile` in configuration and effect is as follows:  
<div style="text-align:center">
    <img src='../images/image33.png', width=50%>
</div>   

Select `wearable` in configuration and effect is as follows:  
<div style="text-align:center">
    <img src='../images/image34.png', width=50%>
</div>   

Also in we can find `Log` tool used in both applications:
<div style="text-align:center">
    <img src='../images/image35.png'>
    <img src='../images/image36.png'>
</div>   

!!!note
    Not all version of `DevEco Studio` have embeded device manager, in case of emulator is not working, using `Previewer` or `real device` instead.