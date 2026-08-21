### Create the Second Page
1. Right-click the `entry > ets > pages` folder, select **New**, and choose **Page**.
<div style="text-align:center">
    <img src='../images_common/image22.png'>
</div> 

2. Enter `SecondPage` as the new page name.
<div style="text-align:center">
    <img src='../images_common/image23.png'>
</div> 

DevEco Studio creates the `SecondPage` page.

Navigate to `entry > resources > base > profile` and open `main_pages.json`. The page route is configured automatically:
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
    If you create the page by another method, configure its route manually in `entry > resources > base > profile > main_pages.json`.

### Add `Text` and `Button` Components
Add `Text` and `Button` components with styled properties, using the first page as a reference. The following sample shows `SecondPage.ets`:
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
          .fontSize(50)
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
        .height('5%')
      }
      .width('100%')
    }
    .height('100%')
  }
}
```
The Previewer displays the second page as follows:
<div style="text-align:center">
    <img src='../images_mobile/image24.png'>
    <img src='../images_wearable/image24.png' width='30%'>
</div>
