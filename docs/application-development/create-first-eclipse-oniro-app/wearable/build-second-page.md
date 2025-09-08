### Create the Second Page
1. Right click `entry > ets > pages` folder, select `New` and choose `Page`.
<div style="text-align:center">
    <img src='../images/image22.png'>
</div> 

2. Type 'SecondPage' as the new Page name. 
<div style="text-align:center">
    <img src='../images/image23.png'>
</div> 

SecondPage created.

Navigate to `entry > resources > base > profile`, open `main_pages.json` you will find the page routes was configured automatically.
```typescript
// main_pages.json
{
  "src": [
    "pages/Index",
    "pages/SecondPage"
  ]
}
```

!!! note
    If you create the page in other methods, you have to manually configure the page routes in above path.

### Add `Text` and `Button` Components
Add **Text** and **Button** components with styled properties, using the first page as a reference. The sample code in `Second.ets` is shown below:
```typescript
// SecondPage.ets
@Entry
@Component
struct SecondPage {
  @State message: string = 'Second Page';

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(45)
          .fontWeight(FontWeight.Bold)
        Button() {
          Text('Back')
            .fontSize(25)
            .fontWeight(FontWeight.Bold)
        }
        .type(ButtonType.Capsule)
        .margin({
          top: 20
        })
        .backgroundColor('#0D9FFB')
        .width('40%')
        .height('20%')
      }
      .width('100%')
    }
    .height('100%')
  }
}
```
You can observe how the second page looks like in the `Previewer`
<div style="text-align:center">
    <img src='../images/image24.png'>
</div> 