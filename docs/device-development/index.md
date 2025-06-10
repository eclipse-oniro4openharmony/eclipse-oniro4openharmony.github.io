---
layout: default
title: Device Development
nav_order: 3
has_children: true
---

# Device Development

This section provides an overview and entry points for developing devices with Eclipse Oniro, which is based on OpenHarmony and currently focuses on the standard system type.

## System Types

OpenHarmony defines three system types: Mini, Small, and Standard.  

- **Mini system**: For devices with MCUs (e.g., Arm Cortex-M, 32-bit RISC-V) and ≥128 KiB RAM. Offers lightweight networking, graphics, and IoT bus support. Typical products: sensors, wearables, connection modules.  

- **Small system**: For devices with application processors (e.g., Arm Cortex-A) and ≥1 MiB RAM. Provides higher security, standard graphics, and video capabilities. Typical products: smart cameras, routers, EDRs.  

- **Standard system**: Oniro currently targets this type, designed for devices with application processors (e.g., Arm Cortex-A) and at least 128 MiB of RAM. The standard system provides a complete application framework, 3D GPU support, hardware composer, and a wide range of components and animations. Typical products include high-end displays and smart appliances.  

## Development Flow

1. **Obtaining the Source Code**  
   Follow the instructions in [Building Oniro](building-oniro.md) to fetch the source code and set up your environment.

2. **Supported Hardware**  
   See [Developer Boards](developer-boards/index.md) for details on supported hardware.  
   Currently, Oniro supports the **HiHope HH-SCDAYU200** and **Raspberry Pi 4 Model B**, with step-by-step build and flashing instructions.

3. **Building and Flashing**  
      1. Use Docker for a clean build environment.  
      2. Set the target device (e.g., `rk3568` for HiHope, `rpi4` for Raspberry Pi).  
      3. Flashing instructions are hardware-specific and provided in each board’s documentation.

4. **Debugging and Tools**  
      1. Use the **HDC tool** for device communication and debugging.  
      2. Serial console access and other debugging tips are provided per board.

