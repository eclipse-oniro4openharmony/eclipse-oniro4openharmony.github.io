# Installation Process

**DevEco Studio** is an integrated development environment (IDE) based on IntelliJ IDEA, tailored for building OpenHarmony applications. It provides integrated tools for coding, debugging, and managing dependencies, making it easier to develop, test, and deploy apps for the OpenHarmony and Oniro platforms.

## Developer Account

!!! note
    For application development, developer verification is not required.

You need a Huawei account to download DevEco Studio. For instructions, see [HUAWEI ID Registration and Verification](https://developer.huawei.com/consumer/en/doc/start/registration-and-verification-0000001053628148).

Identity verification is not required for local Oniro/OpenHarmony application development. Publishing through Huawei services has separate account and verification requirements that are outside the scope of this guide.

## Download

The latest version of DevEco Studio can be downloaded from the [official download page](https://developer.huawei.com/consumer/en/download/).

!!! info
    You need a Huawei account to download DevEco Studio.

<img src='../../images/windows_download.png' alt="DevEco Studio download page listing the Windows, Mac (x86), and Mac (ARM) installers">

=== "Windows"
    Download the Windows installer (`.exe`, listed as "DevEco Studio for Windows").

=== "macOS"
    Download the build matching your Mac's chip: **Mac (X86)** for Intel Macs, or **Mac (ARM)** for Apple Silicon (M-series).

## Step-by-Step Installation

=== "Windows"
    After the download completes, run the installer.

    Click **Next** until you reach the **Choose Install Location** step.
    If necessary, click **Browse...** to select the installation path. The installer shows the required and available space for the selected drive. Click **Next**.
    <img src='../../images/windows_setup1.png' alt="Choose Install Location step, showing space required and space available">

    > **Note:** If this is not your first installation, delete all files from the previous installation directory before proceeding.

    In the **Installation Options** step, keep **Create Desktop Shortcut** selected. Select **Add "bin" folder to the PATH** if you want to invoke DevEco Studio's bundled command-line tools from any terminal; this option requires a restart to take effect. Adding **Open Folder as Project** to the Explorer context menu is optional.
    <img src='../../images/windows_setup2.png' alt="Installation Options step: desktop shortcut, PATH variable, and context menu checkboxes">

    In the **Choose Start Menu Folder** step, keep the default settings and click `Install`.
    <img src='../../images/windows_setup3.png' alt="Choose Start Menu Folder step with the Install button">

    DevEco Studio bundles OpenJDK, so a separate JDK installation is not required for basic use.

=== "macOS"
    Open the downloaded `.dmg` file, then drag **DevEco-Studio** into the **Applications** folder shown in the same window.
    <img src='../../images/macos_installation.png' alt="dmg window: dragging DevEco-Studio into the Applications folder">

    Wait for the copy to finish, then eject the mounted disk image.

    On first launch, macOS Gatekeeper may block the app since it was downloaded from outside the App Store — if so, go to **System Settings → Privacy & Security** and click **Open Anyway**.

## Environment Configuration

### OpenHarmony Version and API Level Reference

Before setting up your development environment, review the relationship between OpenHarmony system versions and their corresponding API levels. This mapping helps you choose the correct API version for your target OpenHarmony system.

<div style="width:100%;box-sizing:border-box; display:flex; justify-content:center;">
  <div style="width:100%; max-width:720px; box-sizing:border-box;">
    <table style="width:100%; table-layout:fixed; border-collapse:separate; border-spacing:0; border:1px solid #e6e6e6; border-radius:6px; overflow:hidden;">
      <thead style="background:#fafafa;">
        <tr>
          <th style="text-align:left;padding:12px 14px;white-space:normal;font-weight:600;border-right:1px solid #f0f0f0;">Version</th>
          <td style="text-align:center;padding:12px 10px;">4.0</td>
          <td style="text-align:center;padding:12px 10px;">4.1</td>
          <td style="text-align:center;padding:12px 10px;">5.0.0</td>
          <td style="text-align:center;padding:12px 10px;">5.0.1</td>
          <td style="text-align:center;padding:12px 10px;">5.0.2</td>
          <td style="text-align:center;padding:12px 10px;">5.0.3</td>
          <td style="text-align:center;padding:12px 10px;">5.1.0</td>
          <td style="text-align:center;padding:12px 10px;">6.0</td>
          <td style="text-align:center;padding:12px 10px;">6.1</td>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th style="text-align:left;padding:12px 14px;white-space:normal;background:#fff;font-weight:600;border-right:1px solid #f0f0f0;">API Level</th>
          <td style="text-align:center;padding:12px 10px;">10</td>
          <td style="text-align:center;padding:12px 10px;">11</td>
          <td style="text-align:center;padding:12px 10px;">12</td>
          <td style="text-align:center;padding:12px 10px;">13</td>
          <td style="text-align:center;padding:12px 10px;">14</td>
          <td style="text-align:center;padding:12px 10px;">15</td>
          <td style="text-align:center;padding:12px 10px;">18</td>
          <td style="text-align:center;padding:12px 10px;">20</td>
          <td style="text-align:center;padding:12px 10px;">23</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

!!! tip "API Level Selection"
    Select the API level that corresponds to the target OpenHarmony system version. For example, use API level 23 for OpenHarmony 6.1.

### DevEco Studio Initial Setup

When you run DevEco Studio for the first time, the setup wizard appears. The wizard is the same on Windows and macOS. The screenshots below are from Windows, but macOS displays the same dialogs with macOS-style window controls.
<img src='../../images/image7.png'>

!!! note "macOS Gatekeeper / User Agreement"
    On macOS, a **User Agreement** dialog may appear before the wizard and ask you to accept the "HarmonyOS Software License and Service Agreement." Accept it to continue. If macOS prevents the application from opening, see the Gatekeeper note in [Step-by-Step Installation](#step-by-step-installation).

Select `Do not import settings` (unless migrating from a previous install).
<img src='../../images/image8.png'>

!!! tip "Behind a proxy?"
    If your network requires a proxy, configure it now under **Settings → Appearance & Behavior → System Settings → HTTP Proxy** — otherwise the Basic Setup step below may fail to download Node.js, Ohpm, or SDK components.

    === "Windows"
        Reach Settings via **File → Settings**.

    === "macOS"
        Reach Settings via **DevEco Studio → Settings** in the menu bar (or the `⌘,` shortcut).

On the **Basic Setup** page, select **Install** for **Node.js** and **Ohpm**.
<img src='../../images/image9.png'>
Click **Next** to proceed to the SDK configuration page, and then set the SDK path to an appropriate location.
<img src='../../images/image10.png'>
Accept the license agreement.
<img src='../../images/image11.png'>
Review the summary and click **Next**.
<img src='../../images/image12.png'>
<img src='../../images/image13.png'>
The environment is now configured.
<img src='../../images/image14.png'>

!!! note "Install Relevant APIs"

    <div style="display:flex;gap:20px;align-items:flex-start;flex-wrap:wrap;" markdown>
      <div style="flex:1 1 360px;font-size:0.9rem;line-height:1.9;" markdown>

      To run applications for OpenHarmony, you also need to install the relevant APIs.

      Open **DevEco Studio** and go to:
      **Tools → SDK Manager → OpenHarmony**, select an API version, and click **Apply** to download it.

      > 💡 With **DevEco Studio 6.1**, select **API Version 23** for development.
      >
      > Refer to the version mapping table above to choose the appropriate API level for your target OpenHarmony system.

      </div>

    </div>

    ![SDK Manager](../images/image18.png){: .center}

After setting up a developer account, installing DevEco Studio, and configuring the environment, continue to [Workflow](../workflow.md) to explore the IDE.
