# Fields

- Name - what your app is called, this can be anything
- Bundle name - uses reverse-domain-style identifier. General pattern: `com.organisation_name.application.name`.
- Location - the directory your project will be created in. Keep in mind that the project will be in a new directory, i.e. if location is set to `~`, all the files will be located in `~/project_name`.
- SDK version - choose depending on the version of OpenHarmony your device has. For emulator, choose 6.1 (API level 23)
- Template - optional, will default to `Empty Ability`.

!!! note
	- The bundle name contains at least three segments, separated by periods (.). Each segment can contain only letters, digits, and underscores (_). Example: com.example.myapplication.
	- The first segment starts with a letter, and other segments start with a digit or letter. Each segment ends with a digit or letter. Example: com.01example.myapplication.
	- Consecutive periods (.) are not allowed, for example, com.example..myapplication.
	- The bundle name contains 7 to 128 characters. 


# Creating the project

In the IDE, simply click on the tab "Create Project".

<div style="text-align:center">
    <img src='../images/create-project.png'>
</div> 

With App Builder, execute `oniro-app create --name <app_name> --location <location> --bundle <bundle_name> --sdk <api_level>`.

This creates a simple hello world app, ready to be built and installed.