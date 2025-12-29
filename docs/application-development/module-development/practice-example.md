Perfect! You are already familar with basic knowledge about module development, let's jump into a step-by-step module development tutorial. In this example, you'll see why module development is so useful.

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
    In this tutorial, we use a shared module across two different device types.
    When creating an application for another device, ensure the new module is set to `entry` as the module type and select the appropriate unique `Device type` in your project.  
    For the shared dynamic library, we select both `Phone` and `Wearable` as device types to ensure compatibility with both applications.

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

### Step 4: Use shared resources  
We customize the `index.ets` page in both `mobile` and `wearable` applications to import the `Log` class from the `shared_library` module and access the string resource `public_resource` from the `AppScope` resource folder.

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

The `Log` utility is now accessible in both applications:
<div style="text-align:center">
    <img src='../images/image35.png'>
    <img src='../images/image36.png'>
</div>   

!!!note
    Not all version of `DevEco Studio` have embedded device manager, in case of emulator is not working, using `Previewer` or `real device` instead.