# Example Usage

## Creating new project
- `oniro-app create --name hello_world --location . --sdk 23 --bundle com.example.hello_world`

## Building the app
- `oniro-app sign` (You only need to sign the app one time.)
- `oniro-app build`

## Installing app on the emulator

- Make sure that the emulator is running.
- `oniro-app emulator connect`
- `oniro-app app install` 
- `oniro-app app launch` if you want to launch the app remotely.