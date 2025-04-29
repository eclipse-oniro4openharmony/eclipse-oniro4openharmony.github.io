---
title: Observed and ObjectLink Decorators
parent: State Management
grand_parent: Basic Concepts
nav_order: 5
layout: default
---

# @Observed and @ObjectLink Decorators

@ObjectLink and @Observed class decorators are used for two-way data synchronization in scenarios involving **nested objects or arrays**, making up for the limitation that other decorators can only observe changes in one layer.

> **NOTE**
>
> These two decorators can be used in ArkTS widgets since API version 9.

## Features

- `@Observed` is used to mark a class so that its created instances can observe property changes, mainly used in nested class scenarios.
- `@ObjectLink` decorates the state variables in the child component, receives the instance of the class decorated with `@Observed`, and establishes a two-way data binding with the corresponding state variable in the parent component.

## Constraints

- Using @Observed to decorate a class changes the original prototype chain of the class. Using @Observed and other class decorators to decorate the same class may cause problems.

- The @ObjectLink decorator cannot be used in custom components decorated by @Entry.

- Value assignment is not allowed for the @ObjectLink decorated variable. To assign a value, use @Prop instead.

## Usage Scenarios
### Nested Object

> **NOTE**
>
> **NextID** is used to generate a unique, persistent key for each array item during **ForEach rendering**, so as to identify the corresponding component.

```ts
// objectLinkNestedObjects.ets
let NextID: number = 1;

@Observed
class Bag {
  public id: number;
  public size: number;

  constructor(size: number) {
    this.id = NextID++;
    this.size = size;
  }
}

@Observed
class User {
  public bag: Bag;

  constructor(bag: Bag) {
    this.bag = bag;
  }
}

@Observed
class Book {
  public bookName: BookName;

  constructor(bookName: BookName) {
    this.bookName = bookName;
  }
}

@Observed
class BookName extends Bag {
  public nameSize: number;

  constructor(nameSize: number) {
    // Invoke the parent class method to process nameSize.
    super(nameSize);
    this.nameSize = nameSize;
  }
}

@Component
struct ViewA {
  label: string = 'ViewA';
  @ObjectLink bag: Bag;

  build() {
    Column() {
      Text(`ViewA [${this.label}] this.bag.size = ${this.bag.size}`)
        .fontColor('#ffffffff')
        .backgroundColor('#ff3d9dba')
        .width(320)
        .height(50)
        .borderRadius(25)
        .margin(10)
        .textAlign(TextAlign.Center)
      Button(`ViewA: this.bag.size add 1`)
        .width(320)
        .backgroundColor('#ff17a98d')
        .margin(10)
        .onClick(() => {
          this.bag.size += 1;
        })
    }
  }
}

@Component
struct ViewC {
  label: string = 'ViewC1';
  @ObjectLink bookName: BookName;

  build() {
    Row() {
      Column() {
        Text(`ViewC [${this.label}] this.bookName.size = ${this.bookName.size}`)
          .fontColor('#ffffffff')
          .backgroundColor('#ff3d9dba')
          .width(320)
          .height(50)
          .borderRadius(25)
          .margin(10)
          .textAlign(TextAlign.Center)
        Button(`ViewC: this.bookName.size add 1`)
          .width(320)
          .backgroundColor('#ff17a98d')
          .margin(10)
          .onClick(() => {
            this.bookName.size += 1;
            console.log('this.bookName.size:' + this.bookName.size)
          })
      }
      .width(320)
    }
  }
}

@Entry
@Component
struct ViewB {
  @State user: User = new User(new Bag(0));
  @State child: Book = new Book(new BookName(0));

  build() {
    Column() {
      ViewA({ label: 'ViewA #1', bag: this.user.bag })
        .width(320)
      ViewC({ label: 'ViewC #3', bookName: this.child.bookName })
        .width(320)
      Button(`ViewB: this.child.bookName.size add 10`)
        .width(320)
        .backgroundColor('#ff17a98d')
        .margin(10)
        .onClick(() => {
          this.child.bookName.size += 10
          console.log('this.child.bookName.size:' + this.child.bookName.size)
        })
      Button(`ViewB: this.user.bag = new Bag(10)`)
        .width(320)
        .backgroundColor('#ff17a98d')
        .margin(10)
        .onClick(() => {
          this.user.bag = new Bag(10);
        })
      Button(`ViewB: this.user = new User(new Bag(20))`)
        .width(320)
        .backgroundColor('#ff17a98d')
        .margin(10)
        .onClick(() => {
          this.user = new User(new Bag(20));
        })
    }
  }
}
```

<div style="text-align:center">
    <img src='../images/image-basic/v8.gif'>
</div>

The @Observed decorated **BookName** class can observe changes in the attributes inherited from the base class.


Event handles in **ViewB**:


- **this.user.bag = new Bag(10)** and **this.user = new User(new Bag(20))**: Change to the @State decorated variable **size** and its attributes.

- **this.child.bookName.size += ...**: Change at the second layer. Though @State cannot observe changes at the second layer, the change of an attribute of @Observed decorated **Bag**, which is attribute **size** in this example, can be observed by @ObjectLink.


Event handle in **ViewC**:


- **this.bookName.size += 1**: A change to the @ObjectLink decorated variable **size** causes the button label to be updated. Unlike @Prop, @ObjectLink does not have a copy of its source. Instead, @ObjectLink creates a reference to its source.

- The @ObjectLink decorated variable is read-only. Assigning **this.bookName = new bookName(...)** is not allowed. Once value assignment occurs, the reference to the data source is reset and the synchronization is interrupted.

### Additional Information
For additional information please refer to the [ArkTS Observed and ObjectLink Decorator](https://github.com/eclipse-oniro-mirrors/docs/blob/OpenHarmony-4.1-Release/en/application-dev/quick-start/arkts-observed-and-objectlink.md)