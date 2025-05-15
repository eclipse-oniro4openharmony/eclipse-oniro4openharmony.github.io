# AppStorage

AppStorage provides central storage for application UI state attributes. It is bound to the application process and is created by the UI framework at application startup.


Unlike LocalStorage, which is usually used for page-level state sharing, AppStorage enables application-wide UI state sharing. AppStorage is equivalent to the hub of the entire application. 


This topic describes the AppStorage use scenarios and related decorators: **@StorageProp** and **@StorageLink**.


## Overview
**AppStorage** is a **built-in global storage** that is created automatically when your app starts. It's designed to **store shared data** that your UI components can access and update at any time.

- Think of it as a **central place** to keep important app-level values, like user settings, theme colors, or login status.
- The data in `AppStorage` stays there **as long as the app is running**.
- Each piece of data is stored with a **unique string key**, similar to a dictionary or map.

UI components can **automatically stay in sync** with values in `AppStorage`. So when the value changes, the UI updates too.

You can also use `AppStorage` to **share state between multiple pages (UIAbility instances)** in your app, as long as they run on the **main thread**.

If you want some values to be **saved even after the app is closed**, you can connect `AppStorage` to other storage sources—like databases or cloud services—using decorators like `@StorageProp` and `@StorageLink`.

## @StorageProp

As mentioned above, if you want to establish a binding between AppStorage and a custom component, you'll need the @StorageProp or @StorageLink decorator. Use @StorageProp(key) or @StorageLink(key) to decorate variables in the component, where **key** identifies an attribute in AppStorage.

When a custom component is initialized, the attribute value corresponding to the key in AppStorage is used to initialize the @StorageProp(key) or @StorageLink(key) decorated variable. Whether the attribute with the given key exists in AppStorage depends on the application logic. This means that it may be missing from AppStorage. In light of this, local initialization is mandatory for the @StorageProp(key) or @StorageLink(key) decorated variable.

By decorating a variable with @StorageProp(key), a one-way data synchronization is established from the attribute with the given key in AppStorage to the variable. A local change can be made, but it will not be synchronized to AppStorage. An update to the attribute with the given key in AppStorage will overwrite local changes.

## @StorageLink

@StorageLink(key) creates a two-way data synchronization between the variable it decorates and the attribute with the given key in AppStorage.

1. Local changes are synchronized to AppStorage.

2. Any change in AppStorage is synchronized to the attribute with the given key in all scenarios, including one-way bound variables (@StorageProp decorated variables and one-way bound variables created through @Prop), two-way bound variables (@StorageLink decorated variables and two-way bound variables created through @Link), and other instances (such as PersistentStorage).

## Use Scenarios
### Example of Using AppStorage and LocalStorage in Application Logic

Since AppStorage is a singleton, its APIs are all static. How these APIs work resembles the non-static APIs of LocalStorage.


```ts
AppStorage.setOrCreate('PropA', 47);

let storage: LocalStorage = new LocalStorage();
storage.setOrCreate('PropA',17);
let propA: number | undefined = AppStorage.get('PropA') // propA in AppStorage == 47, propA in LocalStorage == 17
let link1: SubscribedAbstractProperty<number> = AppStorage.link('PropA'); // link1.get() == 47
let link2: SubscribedAbstractProperty<number> = AppStorage.link('PropA'); // link2.get() == 47
let prop: SubscribedAbstractProperty<number> = AppStorage.prop('PropA'); // prop.get() == 47

link1.set(48); // two-way sync: link1.get() == link2.get() == prop.get() == 48
prop.set(1); // one-way sync: prop.get() == 1; but link1.get() == link2.get() == 48
link1.set(49); // two-way sync: link1.get() == link2.get() == prop.get() == 49

storage.get<number>('PropA') // == 17
storage.set('PropA', 101);
storage.get<number>('PropA') // == 101

AppStorage.get<number>('PropA') // == 49
link1.get() // == 49
link2.get() // == 49
prop.get() // == 49
```


### Example of Using AppStorage and LocalStorage Inside the UI

@StorageLink works together with AppStorage in the same way as @LocalStorageLink works together with LocalStorage. It creates two-way data synchronization with an attribute in AppStorage.


```ts
class PropB {
  code: number;

  constructor(code: number) {
    this.code = code;
  }
}

AppStorage.setOrCreate('PropA', 47);
AppStorage.setOrCreate('PropB', new PropB(50));
let storage = new LocalStorage();
storage.setOrCreate('PropA', 48);
storage.setOrCreate('PropB', new PropB(100));

@Entry(storage)
@Component
struct CompA {
  @StorageLink('PropA') storageLink: number = 1;
  @LocalStorageLink('PropA') localStorageLink: number = 1;
  @StorageLink('PropB') storageLinkObject: PropB = new PropB(1);
  @LocalStorageLink('PropB') localStorageLinkObject: PropB = new PropB(1);

  build() {
    Column({ space: 20 }) {
      Text(`From AppStorage ${this.storageLink}`)
        .onClick(() => {
          this.storageLink += 1;
        })

      Text(`From LocalStorage ${this.localStorageLink}`)
        .onClick(() => {
          this.localStorageLink += 1;
        })

      Text(`From AppStorage ${this.storageLinkObject.code}`)
        .onClick(() => {
          this.storageLinkObject.code += 1;
        })

      Text(`From LocalStorage ${this.localStorageLinkObject.code}`)
        .onClick(() => {
          this.localStorageLinkObject.code += 1;
        })
    }
  }
}
```

<div style="text-align:center">
    <img src='../../images/image-basic/image24.png'>
</div>

### Additional Information
For additional information please refer to the [ArkTS AppStorage](https://github.com/eclipse-oniro-mirrors/docs/blob/OpenHarmony-5.0.2-Release/en/application-dev/quick-start/arkts-appstorage.md)