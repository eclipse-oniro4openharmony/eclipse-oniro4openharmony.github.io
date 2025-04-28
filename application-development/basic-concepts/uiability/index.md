---
title: UIAbility
parent: Basic Concepts
grand_parent: Application Development
nav_order: 5
layout: default
has_children: true
---

# UIAbility

UIAbility is a type of application component that provides the UI for user interactions. The UIAbility module provides lifecycle callbacks such as **component creation**, **destruction**, and **foreground/background switching**. 

UIAbility is the basic unit of scheduling in OpenHarmony and provides a window for applications to draw the UI. An application can contain _one or more UIAbility components_. For example, for a payment application, you can use separate UIAbility components to carry the entry and payment functionalities.

Each UIAbility component instance is displayed as a mission in the system application **Recents**.

You can develop a single UIAbility or multiple UIAbilities for your application based on service requirements.

- If you want your application to be displayed as one mission in Recents, use one UIAbility and multiple pages.

- If you want your application to be displayed as multiple missions in Recents or multiple windows to be opened simultaneously, use multiple UIAbilities.


> Portions of this documentation are adapted from *[OpenHarmony Documentation](https://github.com/eclipse-oniro-mirrors/docs/blob/OpenHarmony-4.1-Release/en/application-dev/reference/apis-ability-kit/js-apis-app-ability-uiAbility.md)* by *OpenHarmony community*, licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).