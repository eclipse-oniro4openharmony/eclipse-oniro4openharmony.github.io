# Core Workflow

After that, the main workflow loop looks like this:

- Generate a signing config if you haven't done so already.
- Build your app.
- Connect to the emulator if you aren't connected already.
- Install your app.
- Optionally you can launch it remotely.

# Oniro IDE

> ADD IMAGE HERE

# Oniro App Builder

The basic commands that you'll need are the following:

- `oniro-app sign`
- `oniro-app build`
- `oniro-app emulator connect`
- `oniro-app install`
- `oniro-app launch`

# Additional App Builder Commands

Oniro App Builder provides some additional tools that are useful for debugging and work with AI agents.

- `oniro-app screenshot` takes a screenshot.
- `oniro-app dump` dumps the device state as JSON.
- `oniro-app lint` an OpenHarmony code linter.
- `oniro-app gesture` and `oniro-app input` to simulate touch.