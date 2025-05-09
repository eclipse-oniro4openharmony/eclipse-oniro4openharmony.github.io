# UIAbility Lifecycle

## Overview

When a user opens or switches to and from an application, the UIAbility instances in the application transit in their different states. The UIAbility class provides a series of callbacks. Through these callbacks, you can know the state changes of the UIAbility instance.

The lifecycle of UIAbility has the following states: **Create**, **Foreground**, **Background**, **Destroy**, **onWindowStageCrate** and **onWindowStageDestory** as shown in the figure below.

**Figure 1** UIAbility lifecycle states

<div style="text-align:center">
    <img src='../images/image1.png'>
</div>

## Description of Lifecycle States

!!! note
    If you want to check the sample code below, because of limited functionailities of **Previewer**, you need to use real device instead. 
    Before proceeding, make sure you finished reading **[Running the Application on a real Device](../../create-first-eclipse-oniro-app/run-real-device.md)** tutorial.

### Create

The **Create** state is triggered when the UIAbility instance is created during application loading. It corresponds to the **onCreate()** callback. In this callback, you can perform page initialization operations.
For example, defining variables or loading resources.


```ts
import type AbilityConstant from '@ohos.app.ability.AbilityConstant';
import UIAbility from '@ohos.app.ability.UIAbility';
import type Want from '@ohos.app.ability.Want';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    // Initialize the page.
  }
  // ...
}
```

### WindowStageCreate

After the UIAbility instance is created but before it enters the **Foreground** state, the system creates a WindowStage instance and triggers the **onWindowStageCreate()** callback. You can set UI loading and WindowStage event subscription in the callback.

In the **onWindowStageCreate()** callback, use `loadContent()` to set the page to be loaded, and call `on('windowStageEvent')` to subscribe to `WindowStage events` like having or losing focus, or becoming visible or invisible.

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import window from '@ohos.window';

export default class EntryAbility extends UIAbility {
  // ...
  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Subscribe to the WindowStage events (having or losing focus, or becoming visible or invisible).
    try {
      windowStage.on('windowStageEvent', (data) => {
        let stageEventType: window.WindowStageEventType = data;
        switch (stageEventType) {
          case window.WindowStageEventType.SHOWN: // Switch to the foreground.
            console.info('windowStage foreground.');
            break;
          case window.WindowStageEventType.ACTIVE: // Gain focus.
            console.info('windowStage active.');
            break;
          case window.WindowStageEventType.INACTIVE: // Lose focus.
            console.info('windowStage inactive.');
            break;
          case window.WindowStageEventType.HIDDEN: // Switch to the background.
            console.info('windowStage background.');
            break;
          default:
            break;
        }
      });
    } catch (exception) {
      console.error('Failed to enable the listener for window stage event changes. Cause:' + JSON.stringify(exception));
    }
    // Set the page to be loaded.
    windowStage.loadContent('pages/Index', (err, data) => {
      // ...
    });
  }
}
```

The effect is as follows:

<div style="text-align:center">
    <img src='../images/v1.gif'>
</div>

### Foreground and Background

The **Foreground** and **Background** states are triggered when the UIAbility instance is switched to the foreground and background, respectively. They correspond to the **onForeground()** and **onBackground()** callbacks.

The **onForeground()** callback is triggered when the UI of the UIAbility instance is about to become visible, for example, when the UIAbility instance is about to enter the foreground. In this callback, you can apply for resources required by the system or re-apply for resources that have been released in the **onBackground()** callback.

The **onBackground()** callback is triggered when the UI of the UIAbility instance is about to become invisible, for example, when the UIAbility instance is about to enter the background. In this callback, you can release unused resources or perform time-consuming operations such as saving the status.

For example, there is an application that requires location access and has obtained the location permission from the user. Before the UI is displayed, you can enable location in the **onForeground()** callback to obtain the location information.

When the application is switched to the background, you can disable location in the **onBackground()** callback to reduce system resource consumption.


```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
  // ...

  onForeground(): void {
    // Apply for the resources required by the system or re-apply for the resources released in onBackground().
  }

  onBackground(): void {
    // Release unused resources when the UI is invisible, or perform time-consuming operations in this callback,
    // for example, saving the status.
  }
}
```

### WindowStageDestroy

Before the UIAbility instance is destroyed, the **onWindowStageDestroy()** callback is invoked to release UI resources.

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import window from '@ohos.window';

export default class EntryAbility extends UIAbility {
  windowStage: window.WindowStage | undefined = undefined;
  // ...
  onWindowStageCreate(windowStage: window.WindowStage): void {
    this.windowStage = windowStage;
    // ...
  }
  onWindowStageDestroy() {
    // Release UI resources.
  }
}
```

### Destroy

The **Destroy** state is triggered when the UIAbility instance is destroyed. You can perform operations such as releasing system resources and saving data in the **onDestroy()** callback.

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
  // ...

  onDestroy() {
    // Release system resources and save data.
  }
}
```

