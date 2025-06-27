# @Watch Decorator


@Watch is used to listen for state variables. If your application needs watch for value changes of a state variable, you can decorate the variable with @Watch.


!!! note
    Since API version 9, this decorator is supported in ArkTS widgets.


## Overview

An application can request to be notified whenever the value of the @Watch decorated variable changes. The @Watch callback is called when the value change has occurred. @Watch uses strict equality (===) to determine whether a value is updated in the ArkUI framework. If **false** is returned, the @Watch callback is triggered.

## Restrictions

- Pay attention to the risk of infinite loops. Loops can be caused by the @Watch callback directly or indirectly mutating the same variable. To avoid loops, avoid mutating the @Watch decorated state variable inside the callback handler.

- Pay attention to performance. The attribute value update function delays component re-render (see the preceding behavior description). The callback should only perform quick computations.

- Calling **async await** from an @Watch function is not recommended, because asynchronous behavior may cause performance issues of re-rendering.


## Application Scenarios

### @Watch and Custom Component Update

This example is used to clarify the processing steps of custom component updates and @Watch. **count** is decorated by @State in **CountModifier** and @Prop in **TotalView**.


```ts
@Component
struct TotalView {
  @Prop @Watch('onCountUpdated') count: number = 0;
  @State total: number = 0;
  // @Watch callback
  onCountUpdated(propName: string): void {
    this.total += this.count;
  }

  build() {
    Text(`Total: ${this.total}`)
  }
}

@Entry
@Component
struct CountModifier {
  @State count: number = 0;

  build() {
    Column() {
      Button('add to basket')
        .onClick(() => {
          this.count++
        })
      TotalView({ count: this.count })
    }
  }
}
```

Processing steps:

1. The click event **Button.onClick** of the **CountModifier** custom component increases the value of **count**.

2. In response to the change of the @State decorated variable **count**, @Prop in the child component **TotalView** is updated, and its **@Watch('onCountUpdated')** callback is triggered, which updates the **total** variable in **TotalView**.

3. The **Text** component in the child component **TotalView** is re-rendered.

### Additional Information
For additional information please refer to the [ArkTS Watch Decorator](https://github.com/eclipse-oniro-mirrors/docs/blob/OpenHarmony-5.1.0-Release/en/application-dev/quick-start/arkts-watch.md)
