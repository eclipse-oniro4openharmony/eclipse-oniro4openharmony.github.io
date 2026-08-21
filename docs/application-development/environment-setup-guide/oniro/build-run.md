# Core Workflow

The main development workflow consists of the following steps:

- Generate a signing configuration if you have not already done so.
- Build your application.
- Connect to the emulator if it is not already connected.
- Install your application.
- Optionally, launch it remotely.

# Oniro IDE

<div style="text-align:center">
    <img src='../images/build.png'>
</div> 

# Oniro App Builder

Use the following commands for the basic workflow:

- `oniro-app sign`
- `oniro-app build`
- `oniro-app emulator connect`
- `oniro-app install`
- `oniro-app launch`

# Additional App Builder Commands

Oniro App Builder provides other tools for debugging and working with AI agents:

- `oniro-app screenshot` takes a screenshot.
- `oniro-app dump` dumps the device state as JSON.
- `oniro-app lint` runs the OpenHarmony code linter.
- `oniro-app gesture` and `oniro-app input` simulate touch input.
