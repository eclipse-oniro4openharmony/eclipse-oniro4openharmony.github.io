### Install HDC  

1. Find the [pipeline](https://ci.openharmony.cn/workbench/cicd/dailybuild/dailylist) whose name is ohos-sdk-full or ohos-sdk-public, click **Download Link**, and select **Full Package**.  
   Use conditional filtering: select project `openharmony`, branch `OpenHarmony-5.1.0-Release`, and a date from the previous month or a manual range.  
   Then find **ohos-sdk-full_5.1.0-Release**, and download the full package (Windows/Linux).  
   *(If daily build SDK is not compatible with your DevEco Studio, try the rolling build SDK.)*
   ![Download SDK](images/image39.png){: .center}

2. Under the `toolchain` folder, locate `hdc.exe` and `libusb_shared.ddl`.  
   ![Toolchain](images/image29.png){: .center}

3. Create a folder called `hdc_bin`, and copy `hdc.exe` and `libusb_shared.dll` into it.  
   ![hdc_bin](images/image30.png){: .center}

4. Add **Environment Variable** to your system:
   - Open `Settings` on Windows → search for `Edit the system environment variables`.
     ![Env Settings](images/image31.png){: .center}
   - In the `Advanced` tab, click `Environment Variables...`.
     ![Env Dialog](images/image32.png){: .center}
   - Edit `Path` under `System variables`, click `New`, and paste your `hdc_bin` path.
     ![Path Config](images/image33.png){: .center}
   - Open **Command Prompt** and run `hdc` to verify installation.
     ![HDC Check](images/image34.png){: .center}

---

### Use Real Machine to Run Application with USB  

1. Connect a development board (e.g., `HiHope HH-SCDAYU200`) running OpenHarmony standard system to your PC.  
   The device will appear at the top of DevEco Studio.  
   ![Device](images/image36.png){: .center}

2. Generate signature:
   - Click `Project Structure...` → `Project > Signing Configs` → check `Automatically generate signature`.
   - Click `Apply` and wait until signature is generated.
     ![Signature Settings](images/image28.png){: .center}
   - You will find it in the `configuration` folder under `build-profile.json5`.  
     ![Signature File](images/image35.png){: .center}

3. Click the `Run 'entry'` triangle button in the IDE.  
   ![Run App](images/image37.png){: .center}

4. Your application will now run on the development board.  
   ![App Running](images/image38.png){: .center width="50%"}

---

🎉 You’ve successfully installed HDC and deployed your first application using DevEco Studio!
