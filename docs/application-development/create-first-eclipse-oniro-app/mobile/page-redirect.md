You can achieve page navigation using the **page router**, which locates the target page based on its URL. To implement this, first import the router module and follow the steps below.  

To deliver better transition effects, use **Navigation**(Recommended).  

### Redirection from the first page to the second page
In the `Index.ets` file of the first page, bind the **onClick** event to the **Next** button, allowing users to navigate to the second page when clicked. The sample code in `Index.ets` is shown below:

```typescript
// Index.ets
// Import the router module.
import { BusinessError } from '@kit.BasicServicesKit';

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
        // Bind the onClick event to the Next button so that clicking the button redirects the user to the second page.
        .onClick(() => {
          console.info(`Succeeded in clicking the 'Next' button.`)
          // Go to the second page.
          this.getUIContext().getRouter().pushUrl({ url: 'pages/SecondPage' }).then(() => {
            console.info('Succeeded in jumping to the second page.')
          }).catch((err: BusinessError) => {
            console.error(`Failed to jump to the second page.Code is ${err.code}, message is ${err.message}`)
          })
        })
      }
      .width('100%')
    }
    .height('100%')
  }
}
```
Click the `Next` button now and page will redirect to the `SecondPage`. You can also observe the printed information in console as well.
<div style="text-align:center">
    <img src='../images/image25.png'>
</div> 

### Redirection from the second page to the first page
In the `SecondPage.ets` file of the second page, bind the **onClick** event to the **Back** button, enabling users to navigate back to the first page when clicked. The sample code in `SecondPage.ets` is shown below:  
```typescript
// SecondPage.ets
// Import the router module.
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct SecondPage {
  @State message: string = 'Hi there';

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
        // Bind the onClick event to the Back button so that clicking the button redirects the user back to the first page.
        .onClick(() => {
          console.info(`Succeeded in clicking the 'Back' button.`)
          try {
            // Return to the first page.
            this.getUIContext().getRouter().back()
            console.info('Succeeded in returning to the first page.')
          } catch (err) {
            let code = (err as BusinessError).code;
            let message = (err as BusinessError).message;
            console.error(`Failed to return to the first page.Code is ${code}, message is ${message}`)
          }
        })
      }
      .width('100%')
    }
    .height('100%')
  }
}
```
Click the `Back` button on the page or the triangle icon on the Previewer, the page will redirect back to the `Index`. You can also observe the printed information in console as well.
<div style="text-align:center">
    <img src='../images/image26.png'>
    <img src='../images/image27.png'>
</div> 