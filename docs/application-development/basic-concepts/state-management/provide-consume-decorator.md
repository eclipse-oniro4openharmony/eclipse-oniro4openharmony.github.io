# @Provide and @Consume Decorators


@Provide and @Consume are used for two-way data synchronization with descendant components when state data needs to be transferred between multiple levels. They do not involve passing a variable from component to component multiple times.


- An @Provide decorated state variable exists in the ancestor component and is said to be "provided" to descendent components. 
- An @Consume decorated state variable is used in a descendent component. It is linked to ("consumes") the provided state variable in its ancestor component.

!!! note
    Since API version 9, these two decorators are supported in ArkTS widgets.


## Features

- An @Provide decorated state variable becomes available to all descendent components of the providing component automatically. The variable is said to be "provided" to other components. This means that you do not need to pass a variable from component to component multiple times.

- A descendent component gains access to the provided state variable by decorating a variable with @Consume. This establishes a two-way data synchronization between the provided and the consumed variable. This synchronization works in the same manner as a combination of @State and @Link does. The only difference is that the former allows transfer across multiple levels of the UI parent-child hierarchy.

- @Provide and @Consume can be bound using the same variable name or variable alias. Whenever possible, use the same variable types to prevent implicit type conversion and consequently application behavior exceptions.


```ts
// Binding through the same variable name
@Provide a: number = 0;
@Consume a: number;

// Binding through the same variable alias
@Provide('a') b: number = 0;
@Consume('a') c: number;
```


When @Provide and @Consume are bound through the same variable name or variable alias, the variables decorated by @Provide and @Consume are in a one-to-many relationship. A custom component, including its child components, should not contain multiple @Provide decorated variables under the same name or alias. Otherwise, a runtime error will occur.

## Usage Scenarios

The following example shows the two-way synchronization between @Provide and @Consume decorated variables. When the buttons in the **CompA** and **CompD** components are clicked, the changes to **reviewVotes** are synchronized to the **CompA** and **CompD** components.



```ts
@Component
struct CompD {
  // The @Consume decorated variable is bound to the @Provide decorated variable in its ancestor component CompA under the same attribute name.
  @Consume reviewVotes: number;

  build() {
    Column() {
      Text(`reviewVotes(${this.reviewVotes})`)
      Button(`reviewVotes(${this.reviewVotes}), give +1`)
        .onClick(() => this.reviewVotes += 1)
    }
    .width('50%')
  }
}

@Component
struct CompC {
  build() {
    Row({ space: 5 }) {
      CompD()
      CompD()
    }
  }
}

@Component
struct CompB {
  build() {
    CompC()
  }
}

@Entry
@Component
struct CompA {
  // @Provide decorated variable reviewVotes is provided by the entry component CompA.
  @Provide reviewVotes: number = 0;

  build() {
    Column() {
      Button(`reviewVotes(${this.reviewVotes}), give +1`)
        .onClick(() => this.reviewVotes += 1)
      CompB()
    }
  }
}
```
<div style="text-align:center">
    <img src='../../images/image-basic/image22.png'>
</div>

### Additional Information
For additional information please refer to the [ArkTS Provide and Consume Decorator](https://github.com/eclipse-oniro-mirrors/docs/blob/OpenHarmony-5.1.0-Release/en/application-dev/quick-start/arkts-provide-and-consume.md) 