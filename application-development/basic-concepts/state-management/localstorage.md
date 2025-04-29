---
title: LocalStorage
parent: State Management
grand_parent: Basic Concepts
nav_order: 6
layout: default
---

# LocalStorage: UI State Storage


LocalStorage provides storage for the page-level UI state. The parameters of the LocalStorage type accepted through the @Entry decorator share the same LocalStorage instance on the page. LocalStorage also allows for state sharing between pages with UIAbility instances.


This topic describes only the LocalStorage application scenarios and related decorators: **@LocalStorageProp** and **@LocalStorageLink**.


> **NOTE**
>
> LocalStorage is supported since API version 9.


## Overview

LocalStorage is an in-memory "database" that ArkTS provides for storing state variables required to build pages of the application UI.

- An application can create multiple LocalStorage instances. These instances can be shared on a page or, by using the **GetShared** API, across pages in a UIAbility instance.

- The root node of a component tree, that is, the @Component decorated by @Entry, can be assigned to a LocalStorage instance. All child instances of this custom component automatically gain access to the same LocalStorage instance.

- An @Component decorated component has access to at most one LocalStorage instance and to `AppStorage`. A component not decorated with @Entry cannot be assigned a LocalStorage instance. It can only accept a LocalStorage instance passed from its parent component through @Entry. A LocalStorage instance can be assigned to multiple components in the component tree.

- All attributes in LocalStorage are mutable.

The application determines the lifecycle of a LocalStorage object. The JS Engine will garbage collect a LocalStorage object when the application releases the last reference to it, which includes deleting the last custom component.

LocalStorage provides two decorators based on the synchronization type of the component decorated with @Component:

- [@LocalStorageProp](#localstorageprop): creates a one-way data synchronization with the named attribute in LocalStorage.

- [@LocalStorageLink](#localstoragelink): creates a two-way data synchronization with the named attribute in LocalStorage.

## @LocalStorageProp

As mentioned above, if you want to establish a binding between LocalStorage and a custom component, you need to use the @LocalStorageProp and @LocalStorageLink decorators. Specially, use @LocalStorageProp(key) or @LocalStorageLink(key) to decorate variables in the component, where **key** identifies the attribute in LocalStorage.


When a custom component is initialized, the @LocalStorageProp(key)/@LocalStorageLink(key) decorated variable is initialized with the value of the attribute with the given key in LocalStorage. Local initialization is mandatory. If an attribute with the given key is missing from LocalStorage, it will be added with the stated initializing value. (Whether the attribute with the given key exists in LocalStorage depends on the application logic.)


> **NOTE**
>
> This decorator can be used in ArkTS widgets since API version 9.


By decorating a variable with @LocalStorageProp(key), a one-way data synchronization is established from the attribute with the given key in LocalStorage to the variable. This means that, local changes (if any) will not be synchronized to LocalStorage, and an update to the attribute with the given key in LocalStorage – for example, a change made with the **set ** API – will overwrite local changes.


## @LocalStorageLink

@LocalStorageLink is required if you need to synchronize the changes of the state variables in a custom component back to LocalStorage.

@LocalStorageLink(key) creates a two-way data synchronization with the attribute with the given key in LocalStorage.

1. If a local change occurs, it is synchronized to LocalStorage.

2. Changes in LocalStorage are synchronized to all attributes with the given key, including one-way bound variables (@LocalStorageProp decorated variables and one-way bound variables created through @Prop) and two-way bound variables (@LocalStorageLink decorated variables and two-way bound variables created through @Link).

## Use Scenarios

### Example of Using LocalStorage in Application Logic
```ts
let para: Record<string,number> = { 'PropA': 47 };
let storage: LocalStorage = new LocalStorage(para); // Create an instance and initialize it with the given object.
let propA: number | undefined = storage.get('PropA') // propA == 47
let link1: SubscribedAbstractProperty<number> = storage.link('PropA'); // link1.get() == 47
let link2: SubscribedAbstractProperty<number> = storage.link('PropA'); // link2.get() == 47
let prop: SubscribedAbstractProperty<number> = storage.prop('PropA'); // prop.get() == 47
link1.set(48); // two-way sync: link1.get() == link2.get() == prop.get() == 48
prop.set(1); // one-way sync: prop.get() == 1; but link1.get() == link2.get() == 48
link1.set(49); // two-way sync: link1.get() == link2.get() == prop.get() == 49
```


### Example for Using LocalStorage Inside the UI

The two decorators @LocalStorageProp and @LocalStorageLink can work together to obtain the state variable stored in a LocalStorage instance in the UI component.

This example uses @LocalStorageLink to show how to:

- Use the **build** function to create a LocalStorage instance named **storage**.

- Use the @Entry decorator to add **storage** to the top-level component **CompA**.

- Use @LocalStorageLink to create a two-way data synchronization with the given attribute in LocalStorage.

 ```ts
class PropB {
  code: number;

  constructor(code: number) {
    this.code = code;
  }
}
// Create a new instance and initialize it with the given object.
let para: Record<string, number> = { 'PropA': 47 };
let storage: LocalStorage = new LocalStorage(para);
storage.setOrCreate('PropB', new PropB(50));

@Component
struct Child {
  // @LocalStorageLink creates a two-way data synchronization with the PropA attribute in LocalStorage.
  @LocalStorageLink('PropA') childLinkNumber: number = 1;
  // @LocalStorageLink creates a two-way data synchronization with the PropB attribute in LocalStorage.
  @LocalStorageLink('PropB') childLinkObject: PropB = new PropB(0);

  build() {
    Column() {
      Button(`Child from LocalStorage ${this.childLinkNumber}`) // The changes will be synchronized to PropA in LocalStorage and with Parent.parentLinkNumber.
        .onClick(() => {
          this.childLinkNumber += 1;
        })
      Button(`Child from LocalStorage ${this.childLinkObject.code}`) // The changes will be synchronized to PropB in LocalStorage and with Parent.parentLinkObject.code.
        .onClick(() => {
          this.childLinkObject.code += 1;
        })
    }
  }
}
// Make LocalStorage accessible from the @Component decorated component.
@Entry(storage)
@Component
struct CompA {
  // @LocalStorageLink creates a two-way data synchronization with the PropA attribute in LocalStorage.
  @LocalStorageLink('PropA') parentLinkNumber: number = 1;
  // @LocalStorageLink creates a two-way data synchronization with the PropB attribute in LocalStorage.
  @LocalStorageLink('PropB') parentLinkObject: PropB = new PropB(0);

  build() {
    Column({ space: 15 }) {
      Button(`Parent from LocalStorage ${this.parentLinkNumber}`) // The initial value from LocalStorage will be 47, because PropA has been initialized.
        .onClick(() => {
          this.parentLinkNumber += 1;
        })

      Button(`Parent from LocalStorage ${this.parentLinkObject.code}`) // The initial value from LocalStorage will be 50, because PropB has been initialized.
        .onClick(() => {
          this.parentLinkObject.code += 1;
        })
      // @The @Component decorated child component automatically obtains access to the CompA LocalStorage instance.
      Child()
    }
  }
}
```

<div style="text-align:center">
    <img src='../images/image-basic/image25.png'>
</div>

### Simple Example of Using @LocalStorageProp with LocalStorage

In this example, the **CompA** and **Child** components create local data that is one-way synchronized with the PropA attribute in the LocalStorage instance **storage**.

- The change of **this.storProp1** in **CompA** takes effect only in **CompA** and is not synchronized to **storage**.

- In the **Child** component, the value of **storageProp2** bound to **Text** is still 47.

```ts
// Create a new instance and initialize it with the given object.
let para: Record<string, number> = { 'PropA': 47 };
let storage: LocalStorage = new LocalStorage(para);
// Make LocalStorage accessible from the @Component decorated component.
@Entry(storage)
@Component
struct CompA {
  // @LocalStorageProp creates a one-way data synchronization with the PropA attribute in LocalStorage.
  @LocalStorageProp('PropA') storageProp1: number = 1;

  build() {
    Column({ space: 15 }) {
      // The initial value is 47. After the button is clicked, the value is incremented by 1. The change takes effect only in storageProp1 in the current component and is not synchronized to LocalStorage.
      Button(`Parent from LocalStorage ${this.storageProp1}`)
        .onClick(() => {
          this.storageProp1 += 1
        })
      Child()
    }
  }
}

@Component
struct Child {
  // @LocalStorageProp creates a one-way data synchronization with the PropA attribute in LocalStorage.
  @LocalStorageProp('PropA') storageProp2: number = 2;

  build() {
    Column({ space: 15 }) {
      // When CompA changes, the current storageProp2 does not change, and 47 is displayed.
      Text(`Parent from LocalStorage ${this.storageProp2}`)
    }
  }
}
```
<div style="text-align:center">
    <img src='../images/image-basic/image26.png'>
</div>


### Additional Information
For additional information please refer to the [ArkTS LocalStorage](https://github.com/eclipse-oniro-mirrors/docs/blob/OpenHarmony-4.1-Release/en/application-dev/quick-start/arkts-localstorage.md#localstorageprop)

