# Environment Setup Guide Overview

## OpenHarmony Version and API Level Reference

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

## DevEco Studio

DevEco Studio is a Huawei IDE based on IntelliJ IDEA. It supports application development with an advanced HarmonyOS emulator and is available for Windows and macOS.

!!! warning "Warning"
    DevEco Studio's emulator isn't available on Intel-based Macs.

## Oniro App Builder & IDE 

These are two separate but related tools:

- Oniro App Builder is a command-line tool that provides the functionality required for Oniro development.
- Oniro IDE is a Visual Studio Code extension based on App Builder. It makes the same functionality available in an IDE.

Because the tools are closely related, this guide discusses them together. They run applications in the Oniro emulator, which provides fewer features than the DevEco Studio emulator. Both tools are available for Windows, Linux, and macOS.

## Full SDK & Public SDK

This section provides information for developers who want to create system applications.
