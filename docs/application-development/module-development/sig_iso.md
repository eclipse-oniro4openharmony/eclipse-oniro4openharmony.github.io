In multi-developer projects, inconsistent local signing configurations can lead to uncommitted files, frequent conflicts, and repeated automatic-signing logins. While these issues are usually minor, they are hard to notice and can cause confusing runtime errors.

We’ll introduce development environment isolation in HarmonyOS, a feature that is often overlooked.

Here, we’ll walk through how to configure isolated signing certificate settings to solve this problem.

### Step 1: Create a new signature
First, add your own automatic signing configuration.
<div style="text-align:center">
    <img src='../images/image37.png'>
</div>   

Then input your `signature name`.
<div style="text-align:center">
    <img src='../images/image38.png'>
</div>   

### Step 2: Configure project `build-profile.json5`
Once the signing configuration is created, your signing profile will appear under `build-profile.json5 → app → signingConfigs`.  
<div style="text-align:center">
    <img src='../images/image39.png'>
</div>  
Also executing the following points:  

- Add a signing configuration under `build-profile.json5 → app → products`  
<div style="text-align:center">
    <img src='../images/image40.png', width=50%>
</div>  

- Add a target configuration under `build-profile.json5 → modules`  
<div style="display:flex; justify-content:center; gap:16px;">
  <img src="../images/image41.png" style="width:50%;">
  <img src="../images/image42.png" style="width:50%;">
</div>

!!!note
    Here I modified both module `mobile` and `shared_library` since module `shared_library` also have been used.

### Step 3: Configure the entry target
Adding a target under both module's `build-profile.json5 → targets`  
<div style="text-align:center">
    <img src='../images/image43.png'>
</div>  

### Step 4: Configure the editor product

<div style="text-align:center">
    <img src='../images/image44.png'>
</div>  

### Step 5: Compile and run
Click run button and check effects.
<div style="text-align:center">
    <img src='../images/image45.png', width=50%>
</div>  
