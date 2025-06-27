# $$ Syntax: Two-Way Synchronization of Built-in Components

The $$ operator provides a TypeScript variable by-reference to a built-in component so that the variable value and the internal state of that component are kept in sync.


What the internal state is depends on the component. For example, for the **TextInput** component, it is the **text** parameter.

## Example

This example uses the **text** parameter of the **TextInput** component.


```ts
// xxx.ets
@Entry
@Component
struct TextInputExample {
  @State text: string = ''
  controller: TextInputController = new TextInputController()

  build() {
    Column({ space: 20 }) {
      Text(this.text)
      TextInput({ text: $$this.text, placeholder: 'input your word...', controller: this.controller })
        .placeholderColor(Color.Grey)
        .placeholderFont({ size: 14, weight: 400 })
        .caretColor(Color.Blue)
        .width(300)
    }.width('100%').height('100%').justifyContent(FlexAlign.Center)
  }
}
```
<div style="text-align:center">
    <img src='../../images/image-basic/v10.gif'>
</div>

### Additional Information
For additional information please refer to the [ArkTS Two Way Sync](https://github.com/eclipse-oniro-mirrors/docs/blob/OpenHarmony-5.1.0-Release/en/application-dev/quick-start/arkts-two-way-sync.md)