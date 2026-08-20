# Build Variants and Signing

Sooner or later you need a package you can hand to someone else — a `.hap`/`.app` file installable outside of DevEco Studio's own Run/Debug flow. That requires understanding build products and signing.

## Debug vs. Release

By default, running or debugging from the IDE produces a **debug** build: automatically signed with a debug certificate so it can be installed on your own emulator/device, but not meant for distribution.

A **release** build is what you'd hand off or publish — optimized, and signed with a certificate meant to persist across builds (so updates are trusted as coming from the same source).

Which variant gets built is controlled by the **Build Variant** selector, plus the products/targets declared in the project's `build-profile.json5` files (see [Project Structure](project-structure.md)).

## Signing Configurations

DevEco Studio supports two signing approaches:

### Automatic Signing (recommended for getting started)

1. Go to **File → Project Structure → Signing Configs** (path may vary slightly by version).
2. Enable **Automatically generate signing configuration**.
3. Sign in with your Huawei/HarmonyOS developer account when prompted.

DevEco Studio then generates a keystore, certificate, and provisioning profile for you and wires them into `build-profile.json5` automatically. This is the fastest path and is sufficient for local testing and most individual development.

### Manual Signing

Needed for CI pipelines, team-shared release certificates, or when a specific provisioning profile (with particular permissions/ACLs) must be used.

1. Generate a private key and CSR: **Build → Generate Key and CSR**, or via `keytool` directly if you need full control over the parameters.
2. Apply for/download a certificate and provisioning profile from the developer console using that CSR.
3. In **Project Structure → Signing Configs**, point to the `.p12`/keystore file, the certificate, and the profile, and supply the store/key passwords.
4. Reference this signing config from the relevant product entry in `build-profile.json5`.

!!! warning "Keep release keys out of the repository"
    Never commit a keystore file or its passwords. Store them in a secrets manager or CI-only environment variables, and keep only a *reference* (path/alias) in version control — see [Version Control](version-control.md) for a `.gitignore` starting point.

## Generating a Package

Once a signing configuration is in place:

* **Build → Build Hap(s)/APP(s) → Build Hap(s)** — produces a `.hap` for the current module/product.
* **Build → Build Hap(s)/APP(s) → Build APP(s)** — produces an `.app` bundle if your product is configured to build one (used for multi-HAP distribution).

Build output appears under the module's `build/` directory, and the **Build** tool window (`Alt+0`) shows progress and any failures.

## Diagnosing Signing/Permission Errors

Two errors are common enough to call out specifically (both also covered in [Common Issues and Solutions](../common-issue.md)):

* **`compileSdkVersion`/`releaseType` mismatch with the device** — the compiled SDK version is newer than what the target device supports. Lower the compiled version in the relevant `build-profile.json5`, or target a newer device/emulator.
* **Install failed due to "grant request permissions failed"** — the requested permission's level (`system_basic` or `system_core`) requires the ACLs be explicitly listed in the provisioning profile used for signing. Check [this permissions reference](https://gitcode.com/openharmony/resources/blob/master/systemres/main/config.json) for the level of each permission your `module.json5` requests, and make sure your signing profile grants it.

## A Practical Checklist Before Distributing a Build

1. Confirm you're building the **release** variant, not debug.
2. Confirm the signing config references a certificate meant for distribution, not the auto-generated debug one.
3. Bump `versionCode`/`versionName` in `AppScope/app.json5` if this is an update to a previously distributed build.
4. Do a clean install test on a device that was never used for debug builds of this app, to rule out state left over from development.

Next: keep all of the above under proper history in [Version Control](version-control.md).
