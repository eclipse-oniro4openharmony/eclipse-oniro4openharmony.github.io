# Fields

- Name - what your app is called, this can be anything
- Bundle name - uses reverse-domain-style identifier. General pattern: `com.organisation_name.application.name`.
- Location - the directory your project will be created in. Keep in mind that the project will be in a new directory, i.e. if location is set to `~`, all the files will be located in `~/project_name`.
- SDK version - choose depending on the version of OpenHarmony your device has. For emulator, choose 6.1 (API level 23)
- Template - optional, will default to `Empty Ability`.

# Creating the project

In the IDE, simply click on the tab "Create Project".

> ADD IMAGE HERE

With App Builder, execute `oniro-app create --name <app_name> --location <location> --bundle <bundle_name> --sdk <api_level>`.