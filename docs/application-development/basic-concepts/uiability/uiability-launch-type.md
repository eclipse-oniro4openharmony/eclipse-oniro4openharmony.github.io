# UIAbility Launch Type


The launch type of the UIAbility component refers to the state of the UIAbility instance at startup. Three launch types are available:

- [Singleton](#singleton)

- [Multiton](#multiton)

- [Specified](#specified)

In this tutorial, we are focusing on `Singleton` and `Multiton`.

## Singleton

**singleton** is the default launch type.

Each time **startAbility()** is called, if a UIAbility instance of this type already exists in the application process, the instance is reused. In other words, UIAbility of this type can have only one instance in the system, meaning that only one mission is displayed in the system application Recents.

**Figure 1** Demonstration effect in singleton mode

<div style="text-align:center">
    <img src='../images/v2.gif'>
</div>

To use the singleton mode, set **launchType** in the `module.json5` file to **singleton**.

```json
{
  "module": {
    ...
    "abilities": [
      {
        "launchType": "singleton",
        ...
      }
    ]
  }
}
```


## Multiton

In multiton mode, each time **startAbility()** is called, a new UIAbility instance is created in the application process. Multiple missions are displayed for UIAbility of this type in Recents.  

**Figure 2** Demonstration effect in multiton mode

<div style="text-align:center">
    <img src='../images/v3.gif'>
</div>

To use the multiton mode, set **launchType** in the `module.json5` file to **multiton**.

```json
{
  "module": {
    ...
    "abilities": [
      {
        "launchType": "multiton",
        ...
      }
    ]
  }
}
```

## Specified

The **specified** mode is used in some special scenarios. For example, in a document application, you may want a document instance to be created each time you create a document, and you may also want to use the same document instance when you open an existing document.

**Figure 3** Demonstration effect in specified mode

<div style="text-align:center">
    <img src='../images/v4.gif'>
</div> 

In the following example, there are two UIAbility components: EntryAbility and SpecifiedAbility (with the launch type **specified**). 

You can learn more about `Specified` from [this](https://github.com/eclipse-oniro-mirrors/docs/blob/OpenHarmony-5.0.2-Release/en/application-dev/application-models/uiability-launch-type.md#singleton) source.