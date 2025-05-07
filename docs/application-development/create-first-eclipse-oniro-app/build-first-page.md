​Now that you're familiar with DevEco Studio, let's create a simple application that demonstrates page navigation.
### Use the `Text` Component
​After the project synchronization completes, navigate to `entry > ets > pages` in the **Ohos** window and open the `Index.ets` file. This file contains a Text component. The sample code in the `Index.ets` file is shown below:​
```typescript
// Index.ets
@Entry
@Component
struct Index {
  @State message: string = 'Hello World';

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)
      }
      .width('100%')
    }
    .height('100%')
  }
}
```
### Add a `Button` Component
On the `Index` page, add a **Button** component to handle user clicks and navigate to another page. The following sample code in `Index.ets` demonstrates this implementation:

```typescript
// Index.ets
@Entry
@Component
struct Index {
  @State message: string = 'Hello World';

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)
        // Add a button to respond to user clicks.
        Button() {
          Text('Next')
            .fontSize(30)
            .fontWeight(FontWeight.Bold)
        }
        .type(ButtonType.Capsule)
        .margin({
          top: 20
        })
        .backgroundColor('#0D9FFB')
        .width('40%')
        .height('5%')
      }
      .width('100%')
    }
    .height('100%')
  }
}
```
Now you can observe how the first page looks like in the `Previewer`
<div style="text-align:center">
    <img src='../images/image21.png'>
</div> 