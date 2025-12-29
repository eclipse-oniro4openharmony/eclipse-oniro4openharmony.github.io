## Module Development Guide

### Before You Begin

Make sure you have gone through the [Environment Setup Guide](../environment-setup-config/index.md) and studied [mobile development tutorial](../create-first-eclipse-oniro-app/mobile/index.md) or [wearable development tutorial](../create-first-eclipse-oniro-app/wearable/index.md).

### Why Module Development?

When your project becomes bigger and bigger, putting all code into one module quickly causes problems:

- Code becomes messy and difficult to maintain
- Build times become slow
- Changing one feature may affect others
- Code and resources cannot be efficiently shared across multiple modules

Therefore we have the **shared package** solution to resolve the problems.

### Shared Packages Solution(HAR and HSP)

A shared package is essentially the same as a **Library** in Android. Its purpose is to enable the sharing of code and resources.

In **OpenHarmony**, developers are provided with two types of shared packages:

- **HAR (Harmony Archive)** — a *static* shared package

- **HSP (Harmony Shared Package)** — a *dynamic* shared package

The difference between them is that **HAR**, as a static shared package, has its code and resources compiled together with the consuming module. If there are multiple consumers, multiple identical copies will exist in their respective build outputs.

In contrast, **HSP**, as a dynamic shared package, can be compiled independently. At runtime, only a single copy of its code exists within a process, as illustrated in the figure below.

<div style="text-align:center">
    <img src='./images/image1.png'>
</div> 

The introduction of **HSP (dynamic shared packages)** mainly aims to address the following issues:

1. When multiple HAPs reference the same HAR, the app package size increases due to duplicated copies.
2. When multiple HAPs reference the same HAR, certain state variables inside the HAR cannot be shared.

In general, if the shared code and resources are used **within a single application**, it is recommended to use a **dynamic shared package (HSP)**.
If the package is intended to be used as a **dependency by application modules**, a **static shared package (HAR)** can be chosen.

In practice, developers should select the appropriate option based on actual business requirements.

### HAR vs HSP Comparison

| Feature | HAR (Static) | HSP (Dynamic) |
|---------|-------------|--------------|
| Compilation | Compiled with consumer | Compiled independently |
| Code Copies | Multiple copies (per consumer) | Single copy at runtime |
| Package Size | Larger (due to duplication) | Smaller (shared code) |
| State Sharing | Cannot share state variables | Can share state across modules |
| Use Case | Module dependencies | Internal application sharing |
| Configuration Complexity | Lower | Higher |

### What You Will Learn

Through this module development guide, you will master:

- **Create Shared Modules**: Set up and configure HAR and HSP modules in your project
- **Manage Dependencies**: Add and configure shared package dependencies using multiple methods
- **Use Shared Resources**: Call methods, classes, components, and pages from shared packages
- **Navigate Between Modules**: Implement page navigation across different modules
- **Deploy Multi-Module Applications**: Configure and run applications with multiple packages
- **Best Practices**: Understand guidelines for effective shared package usage and troubleshooting

### Next Steps

Ready to get started? Begin with [Create Module Project](create-module-project.md) to learn how to set up your first modular HarmonyOS application.