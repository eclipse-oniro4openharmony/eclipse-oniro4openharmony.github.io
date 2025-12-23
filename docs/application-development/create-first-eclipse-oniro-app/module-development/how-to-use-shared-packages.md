### Overview

After creating and configuring your shared packages, the next step is to actually use them.

### Method Call  

In the **shared_library** module, there is a utility class **Calc** that contains a simple addition method **add**.  
So, how can the **add** method in **sharelibrary** be called from **entry**?
<div style="text-align:center">
    <img src='../images/image14.png'>
</div>  

You can access the resources or code in the shared package.  
For example, to call the **add** method, you can implement it as follows:  

```bash
import { add } from "@ohos/shared_library"
```  

The following example demonstrates a very simple usage: a click event is added to a text component, which directly calls the **add** method in the utility class **Calc** from the shared package **shared_library**.

<div style="text-align:center">
    <img src='../images/image15.png'>
</div>  

The effect of using **Previewer** is as following:  
<div style="text-align:center">
    <img src='../images/image16.png'>
</div> 

### Class Call  
Define the utility class that needs to be exposed. Here, a simple `Log` utility class is created.
Note that the **export** keyword must be used to expose it for external access.

```bash
import { hilog } from "@kit.PerformanceAnalysisKit";

export class Log {
    private static readonly DOMAIN = 0x0230;
    private static readonly TAG: string = '[Voice]';
    public static readonly LEVEL_DEBUG = hilog.LogLevel.DEBUG;
    public static readonly LEVEL_INFO = hilog.LogLevel.INFO;
    public static readonly LEVEL_WARN = hilog.LogLevel.WARN;
    public static readonly LEVEL_ERROR = hilog.LogLevel.ERROR;
    public static readonly LEVEL_FATAL = hilog.LogLevel.FATAL;
    public static LOG_LEVEL = Log.LEVEL_DEBUG;

    public static debug(TAG: string, ...arg: Array<string | object>) {
        let message = ''
        arg.forEach(item=>{
            let msg = item
            if (typeof msg !== 'string') {
                msg = JSON.stringify(msg)
            }
            message += msg
        })
        if (this.LOG_LEVEL <= this.LEVEL_DEBUG) {
            hilog.debug(this.DOMAIN, this.TAG, "[" + TAG + "]: " + message);
        }
    }

    public static info(TAG: string, ...arg: Array<any>) {
        let message = ''
        arg.forEach(item=>{
            let msg = item
            if (typeof msg !== 'string') {
                msg = JSON.stringify(msg)
            }
            message += msg
        })
        if (this.LOG_LEVEL <= this.LEVEL_INFO) {
            hilog.info(this.DOMAIN, this.TAG, "[" + TAG + "]: " + message);
        }
    }

    public static warn(TAG: string, message: string | object) {
        if (typeof message !== 'string') {
            message = JSON.stringify(message)
        }
        if (this.LOG_LEVEL <= this.LEVEL_WARN) {
            hilog.warn(this.DOMAIN, this.TAG, "[" + TAG + "]: " + message);
        }
    }

    public static error(TAG: string, ...arg: Array<string | object>) {
        let message = ''
        arg.forEach(item=>{
            let msg = item
            if (typeof msg !== 'string') {
                msg = JSON.stringify(msg)
            }
            message += msg
        })
        if (this.LOG_LEVEL <= this.LEVEL_ERROR) {
            hilog.error(this.DOMAIN, this.TAG, "[" + TAG + "]: " + message);
        }
    }

    public static fatal(TAG: string, message: string | object) {
        if (typeof message !== 'string') {
            message = JSON.stringify(message)
        }
        if (this.LOG_LEVEL <= this.LEVEL_FATAL) {
            hilog.info(this.DOMAIN, this.TAG, "[" + TAG + "]: " + message);
        }
    }
}
```

!!! info
    The ports that are exposed to other modules must be declared in the shared package entry file **index.ets** like:
    ```bash
    export { Log } from './src/main/ets/utils/Log';
    ```

<div style="text-align:center">
    <img src='../images/image17.png'>
</div> 
The usage follows the same process as described before.
<div style="text-align:center">
    <img src='../images/image18.png'>
</div> 

### Component Call

Component call is the same as class/method call.
As shown below, a simple text component is defined. Remember to use the **export** keyword to expose it externally.

```bash 
@Component
export struct TextWidget {
  @State message: string = 'Text string'

  build() {
    Text(this.message)
      .fontSize(20)
      .fontWeight(FontWeight.Bold)
  }
}
```

<div style="text-align:center">
    <img src='../images/image19.png'>
</div> 

!!! info
    The components that are exposed to other modules must be declared in the shared package entry file **index.ets** like:
    ```bash
    export { TextWidget } from './src/main/ets/component/textWidget';
    ```

<div style="text-align:center">
    <img src='../images/image20.png'>
</div> 
Component call result is as follows:
<div style="text-align:center">
    <img src='../images/image21.png'>
</div> 

### How to use resources from shared package  
As a shared package intended for reuse by other modules, reuse is not limited to code—resources are equally important. After checking a large number of materials and documents, the question of how to expose **resources** turns out to be quite tricky. It can be stated responsibly that **HarmonyOS currently does not support exposing resources from a shared package**.

The solution is actually quite simple. Since resources themselves cannot be exposed, **classes and methods can be**. Therefore, resource attributes can be wrapped and exposed through **utility classes**, acting as an intermediate layer to access those resources indirectly.

The demostration is divided into 3 steps.

#### Step 1: Define shared resource

First let's define a shared text resource in `shared_library` module as following: 
<div style="text-align:center">
    <img src='../images/image23.png'>
</div> 

#### Step 2: Define resource tool class  
Define an intermediate class tool as following and expose the port in `Index.ets` page:

```bash
export class ResourceTool {
  static getTestResource(): Resource {
    return $r("app.string.shared_string_resource")
  }
}
```
!!!info
    One thing to note about the utility class is that the file must end with the **`.ets`** extension.
    This is required in order to access **Resource** objects.

<div style="text-align:center">
    <img src='../images/image24.png'>
</div> 

#### Step 3: Call shared resource  
After importing the package, you can call it directly.
<div style="text-align:center">
    <img src='../images/image25.png'>
</div> 