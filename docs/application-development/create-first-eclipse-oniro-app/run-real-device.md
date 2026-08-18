### Install HDC  

1. Find the [pipeline](https://ci.openharmony.cn/workbench/cicd/dailybuild/dailylist) whose name is ohos-sdk-full or ohos-sdk-public, click **Download Link**, and select **Full Package**.  
   Use conditional filtering: select project `openharmony`, branch `OpenHarmony-5.1.0-Release`, and a date from the previous month or a manual range.  
   Then find **ohos-sdk-full_5.1.0-Release**, and download the full package (Windows/Linux).  
   *(If daily build SDK is not compatible with your DevEco Studio, try the rolling build SDK.)*
   ![Download SDK](images_common/image39.png){: .center}

2. Under the `toolchain` folder, locate `hdc.exe` and `libusb_shared.ddl`.  
   ![Toolchain](images_common/image29.png){: .center}

3. Create a folder called `hdc_bin`, and copy `hdc.exe` and `libusb_shared.dll` into it.  
   ![hdc_bin](images_common/image30.png){: .center}

4. Add **Environment Variable** to your system:
   - Open `Settings` on Windows → search for `Edit the system environment variables`.
     ![Env Settings](images_common/image31.png){: .center}
   - In the `Advanced` tab, click `Environment Variables...`.
     ![Env Dialog](images_common/image32.png){: .center}
   - Edit `Path` under `System variables`, click `New`, and paste your `hdc_bin` path.
     ![Path Config](images_common/image33.png){: .center}
   - Open **Command Prompt** and run `hdc` to verify installation.
     ![HDC Check](images_common/image34.png){: .center}

---

### Use Real Machine to Run Application with USB  

1. Connect a development board (e.g., `HiHope HH-SCDAYU200`) running OpenHarmony standard system to your PC.  
   The device will appear at the top of DevEco Studio.  
   ![Device](images_mobile/image36.png){: .center}

2. Generate signature:
   - Click `Project Structure...` → `Project > Signing Configs` → check `Automatically generate signature`.
   - Click `Apply` and wait until signature is generated.
     ![Signature Settings](images_mobile/image28.png){: .center}
   - You will find it in the `configuration` folder under `build-profile.json5`.  
     ![Signature File](images_mobile/image35.png){: .center}

3. Click the `Run 'entry'` triangle button in the IDE.  
   ![Run App](images_mobile/image37.png){: .center}

4. Your application will now run on the development board.  
   ![App Running](images_mobile/image38.png){: .center width="50%"}

---

### Run the application on watch

!!! note
    We are using Huawei Watch 5 in this tutorial.


1. Make sure you watch connects the same network with your PC.

2. Check the IP address on your watch and click `Tools` → `IP Connection` on the navbar.
!!! note
    To find IP address on your watch, you need to enable `Developer Option` first. (Go to `Settings` → `HUAWEI WATCH 5`, find `Software Version` and continuously click 5 times)

   Put the watch IP address into the following field, you can find the device at the top of DevEco Studio after clicking the green `start` buttion:
   ![Device](images_wearable/image36.png){: .center}
   ![Device](images_wearable/image39.png){: .center}

2. Generate signature:
   - Click `Project Structure...` → `Project > Signing Configs` → check `Automatically generate signature`.
   - Click `Apply` and wait until signature is generated.
     ![Signature Settings](images_wearable/image28.png){: .center}
   - You will find it in the `configuration` folder under `build-profile.json5`.  
     ![Signature File](images_wearable/image35.png){: .center}

3. Click the `Run 'entry'` triangle button in the IDE.  
   ![Run App](images_wearable/image37.png){: .center}

4. Your application will now run on the development board.  
   ![App Running](images_wearable/image38.png){: .center width="50%"}

---

🎉 You’ve successfully installed HDC and deployed your first mobile application using DevEco Studio!

If you’re interested, this video takes you deeper into wearable development with practical sensor usage.
<iframe
  width="100%"
  height="420"
  src="https://www.youtube-nocookie.com/embed/watch?v=WITjqfofG6k&list=PLy7t4z5SYNaT3VUbRGCoNH471N9sSs0uV&index=2"
  title="HarmonyOS Wearable Tutorial"
  frameborder="0"
  loading="lazy"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; picture-in-picture; web-share"
  allowfullscreen>
</iframe>

