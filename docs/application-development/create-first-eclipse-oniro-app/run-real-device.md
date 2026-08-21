### Configure HDC

HDC is included in the SDK installed by DevEco Studio; downloading a Full SDK or copying individual executables is not required. You can run it from DevEco Studio's **Terminal**. To use it from another terminal, add the `toolchains` directory of the installed SDK to `PATH`. The SDK location is shown in **Tools → SDK Manager**.

Depending on the SDK layout, HDC is under a path such as `<SDK location>/<API version>/toolchains` (OpenHarmony SDK) or `<DevEco Studio>/sdk/default/HarmonyOS/toolchains` (bundled HarmonyOS SDK). Keep the HDC executable and its accompanying libraries together.

Verify the installation and connection with:

```bash
hdc -h
hdc list targets
```

For the current command reference and USB troubleshooting, see the [official HDC documentation](https://developer.huawei.com/consumer/en/doc/harmonyos-guides/hdc).

---

### Run the Application on a Physical Device over USB

1. Connect a development board, such as the `HiHope HH-SCDAYU200`, running the OpenHarmony standard system to your computer.
   Enable USB debugging on the device and approve the authorization prompt. The device will then appear at the top of DevEco Studio.
   ![Device](images_mobile/image36.png){: .center}

2. Generate a signature:
   - Click **Project Structure... → Project → Signing Configs** and select **Automatically generate signature**.
   - Click **Apply** and wait for DevEco Studio to generate the signature.
     ![Signature Settings](images_common/image28.png){: .center}
   - The generated signing configuration appears under `app.signingConfigs` in the project-level `build-profile.json5`; its `material` fields contain the certificate, profile, and keystore paths.
     ![Signature File](images_common/image35.png){: .center}

3. Click the `Run 'entry'` triangle button in the IDE.  
   ![Run App](images_mobile/image37.png){: .center}

4. Your application will now run on the development board.  
   ![App Running](images_mobile/image38.png){: .center width="50%"}

---

### Run the Application on a Watch

!!! note
    This tutorial uses a Huawei Watch 5.


1. Connect the watch and your computer to the same network.

2. Find the watch's IP address, then click **Tools → IP Connection** in the navigation bar.
!!! note
    To find the watch's IP address, first enable **Developer options**. Go to **Settings → HUAWEI WATCH 5**, find **Software Version**, and tap it five times.

   Enter the watch's IP address in the following field. After clicking the green **Start** button, the device appears at the top of DevEco Studio:
   ![Device](images_wearable/image36.png){: .center}
   ![Device](images_wearable/image39.png){: .center}

3. Generate a signature:
   - Click **Project Structure... → Project → Signing Configs** and select **Automatically generate signature**.
   - Click **Apply** and wait for DevEco Studio to generate the signature.
     ![Signature Settings](images_common/image28.png){: .center}
   - The generated signing configuration appears under `app.signingConfigs` in the project-level `build-profile.json5`; its `material` fields contain the certificate, profile, and keystore paths.
     ![Signature File](images_common/image35.png){: .center}

4. Click the **Run 'entry'** triangle button in the IDE.
   ![Run App](images_wearable/image37.png){: .center}

5. The application now runs on the watch.
   ![App Running](images_wearable/image38.png){: .center width="50%"}

---

You have now verified HDC and deployed your first application with DevEco Studio.

The following video provides a more detailed introduction to wearable development and practical sensor use.
<iframe
  width="100%"
  height="420"
  src="https://www.youtube-nocookie.com/embed/WITjqfofG6k?list=PLy7t4z5SYNaT3VUbRGCoNH471N9sSs0uV&index=2"
  title="HarmonyOS Wearable Tutorial"
  frameborder="0"
  loading="lazy"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; picture-in-picture; web-share"
  allowfullscreen>
</iframe>

