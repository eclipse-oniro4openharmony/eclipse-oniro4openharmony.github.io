# @Link Decorator

An @Link decorated variable creates two-way synchronization with a variable of its parent component.

!!! note
    This decorator can be used in ArkTS widgets since API version 9.

## Feature and Constraint

- An @Link decorated variable in a child component shares the same value with a variable in its parent component.
- The @Link decorator cannot be used in custom components decorated by @Entry.

## Usage Scenarios
### Example for @Link with Simple and Class Types

In the following example, after **Parent View: Set yellowButton** and **Parent View: Set GreenButton** of the parent component **ShufflingContainer** are clicked, the change in the parent component is synchronized to the child components.

  1. After buttons of the child components **GreenButton** and **YellowButton** are clicked, the child components (`@Link` decorated variables) change accordingly. Due to the two-way synchronization relationship between `@Link` and `@State`, the changes are synchronized to the parent component.
  
  2. When a button in the parent component **ShufflingContainer** is clicked, the parent component (`@State` decorated variable) changes, and the changes are synchronized to the child components, which are then updated accordingly.

```ts
class GreenButtonState {
  width: number = 0;

  constructor(width: number) {
    this.width = width;
  }
}

@Component
struct GreenButton {
  @Link greenButtonState: GreenButtonState;

  build() {
    Button('Green Button')
      .width(this.greenButtonState.width)
      .height(40)
      .backgroundColor('#64bb5c')
      .fontColor('#FFFFFF, 90%')
      .onClick(() => {
        if (this.greenButtonState.width < 700) {
          // Update the attribute of the class. The change can be observed and synchronized back to the parent component.
          this.greenButtonState.width += 60;
        } else {
          // Update the class. The change can be observed and synchronized back to the parent component.
          this.greenButtonState = new GreenButtonState(180);
        }
      })
  }
}

@Component
struct YellowButton {
  @Link yellowButtonState: number;

  build() {
    Button('Yellow Button')
      .width(this.yellowButtonState)
      .height(40)
      .backgroundColor('#f7ce00')
      .fontColor('#FFFFFF, 90%')
      .onClick(() => {
        // The change of the decorated variable of a simple type in the child component can be synchronized back to the parent component.
        this.yellowButtonState += 40.0;
      })
  }
}

@Entry
@Component
struct ShufflingContainer {
  @State greenButtonState: GreenButtonState = new GreenButtonState(180);
  @State yellowButtonProp: number = 180;

  build() {
    Column() {
      Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Center }) {
        // Simple type @Link in the child component synchronized from @State in the parent component.
        Button('Parent View: Set yellowButton')
          .width(312)
          .height(40)
          .margin(12)
          .fontColor('#FFFFFF, 90%')
          .onClick(() => {
            this.yellowButtonProp = (this.yellowButtonProp < 700) ? this.yellowButtonProp + 40 : 100;
          })
        // Class type @Link in the child component synchronized from @State in the parent component.
        Button('Parent View: Set GreenButton')
          .width(312)
          .height(40)
          .margin(12)
          .fontColor('#FFFFFF, 90%')
          .onClick(() => {
            this.greenButtonState.width = (this.greenButtonState.width < 700) ? this.greenButtonState.width + 100 : 100;
          })
        // Initialize the class type @Link.
        GreenButton({ greenButtonState: $greenButtonState }).margin(12)
        // Initialize the simple type @Link.
        YellowButton({ yellowButtonState: $yellowButtonProp }).margin(12)
      }
    }
  }
}
```
<div style="text-align:center">
    <img src='../../images/image-basic/v6.gif'>
</div>

### Array Type @Link

The ArkUI framework can observe the addition, deletion, and replacement of array items. It should be noted that, in the following example, the type of the @Link and @State decorated variables is the same: number[]. It is not allowed to define the @Link decorated variable in the child component as type number (**@Link item: number**), and create child components for each array item in the @State decorated array in the parent component.

```ts
@Component
struct Child {
  @Link items: number[]; // Link array for bidirectional synchronization

  build() {
    Column({ space: 10 }) {
      Button(`Button1: push`).onClick(() => {
        // Add a new element to the array, synchronized back to the parent
        this.items.push(this.items.length + 1);
      })
        .width('80%')
        .height(40)
      Button(`Button2: replace whole item`).onClick(() => {
        // Replace the entire array, synchronized back to the parent
        this.items = [100, 200, 300];
      })
        .width('80%')
        .height(40)
    }
    .margin(10)
  }
}

@Entry
@Component
struct Parent {
  @State arr: number[] = [1, 2, 3]; // State array for synchronization

  build() {
    Column() {
      Child({ items: $arr }) // Initialize Child with @Link to parent state array
      ForEach(this.arr,
        (item: number) => {
          Row() {
            Text(`${item}`) // Display each item in the array
              .fontColor('#d7072858')
              .height(40)
          }
          .margin(10)
          .width('80%')
          .justifyContent(FlexAlign.Center)
          .backgroundColor('#1e000000')
          .borderRadius(20)
        },
        (item: number) => item.toString()
      )
    }
    .width('100%')
  }
}
```

<div style="text-align:center">
    <img src='../../images/image-basic/v7.gif'>
</div>

### Additional Information
For additional information please refer to the [ArkTS Link Decorator](https://github.com/eclipse-oniro-mirrors/docs/blob/OpenHarmony-5.0.2-Release/en/application-dev/quick-start/arkts-link.md)