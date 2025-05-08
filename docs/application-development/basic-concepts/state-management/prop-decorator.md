# @Prop Decorator
One-way synchronization is supported between an `@Prop` decorated variable a variable of its parent component. This means that, an `@Prop` decorated variable is mutable, and its changes will not be synchronized to the parent component.

## Features
For the `@Prop` decorated variable of a child component, the change synchronization to the parent component is uni-directional.

- An `@Prop` variable is allowed to be modified locally, but the change does not propagate back to its parent component.

- Whenever the data source changes, the `@Prop` decorated variable gets updated, and any locally made changes are overwritten. In other words, the change is synchronized from the parent component to the (owning) child component, but not the other way around.

## Restrictions
- When decorating variables, `@Prop` makes a deep copy, during which all types, except primitive types, Map, Set, Date, and Array, will be lost. For example, for complex types provided by N-API, such as `PixelMap`, because they are partially implemented in the native code, complete data cannot be obtained through a deep copy in ArkTS.

- The `@Prop` decorator cannot be used in custom components decorated by `@Entry`.

## Use Scenarios
### Simple Type Sync from @State of the Parent Component to @Prop of the Child Component
In this example, the `@Prop` decorated count variable in the `CountDownComponent` child component is initialized from the `@State` decorated `countDownStartValue` variable in the `ParentComponent`. When Try again is touched, the value of the count variable is modified, but the change remains within the `CountDownComponent` and does not affect the `ParentComponent`.

Updating `countDownStartValue` in the `ParentComponent` will update the value of the `@Prop` decorated count.

```ts
@Component
struct CountDownComponent {
  @Prop count: number = 0;
  costOfOneAttempt: number = 1;

  build() {
    Column() {
      if (this.count > 0) {
        Text(`You have ${this.count} Nuggets left`)
      } else {
        Text('Game over!')
      }
      // Changes to the @Prop decorated variables are not synchronized to the parent component.
      Button(`Try again`).onClick(() => {
        this.count -= this.costOfOneAttempt;
      })
    }
  }
}

@Entry
@Component
struct ParentComponent {
  @State countDownStartValue: number = 10;

  build() {
    Column() {
      Text(`Grant ${this.countDownStartValue} nuggets to play.`)
      // Changes to the data source provided by the parent component are synchronized to the child component.
      Button(`+1 - Nuggets in New Game`).onClick(() => {
        this.countDownStartValue += 1;
      })
      // Updating the parent component will also update the child component.
      Button(`-1  - Nuggets in New Game`).onClick(() => {
        this.countDownStartValue -= 1;
      })

      CountDownComponent({ count: this.countDownStartValue, costOfOneAttempt: 2 })
    }
  }
}
```
The display is previewed as the following:
<div style="text-align:center">
    <img src='../../images/image-basic/image18.png'>
</div>



In the preceding example:

1. On initial render, when the **CountDownComponent** child component is created, its `@Prop` decorated **count** variable is initialized from the `@State` decorated **countDownStartValue** variable in the **ParentComponent**.

2. When the "+1" or "-1" button is touched, the `@State` decorated **countDownStartValue** of the **ParentComponent** changes. This will cause the **ParentComponent** to re-render. At the minimum, the **CountDownComponent** will be updated because of the change in the **count** variable value.

3. Because of the change in the **count** variable value, the **CountDownComponent** child component will re-render. At a minimum, the **if** statement's condition (**this.counter> 0**) is evaluated, and the **Text** child component inside the **if** statement would be updated.

4. When **Try again** in the **CountDownComponent** child component is touched, the value of the **count** variable is modified, but the change remains within the child component and does not affect the **countDownStartValue** in the parent component.

5. Updating **countDownStartValue** will overwrite the local value changes of the `@Prop` decorated **count** in the **CountDownComponent** child component.

### Simple Type @Prop Synced from @State Array Item in Parent Component
The `@State` decorated array an array item in the parent component can be used as data source to initialize and update a `@Prop` decorated variable. In the following example, the `@State` decorated array arr in the parent component Index initializes the `@Prop` decorated value variable in the child component Child.
```ts
@Component
struct Child {
  @Prop value: number = 0;

  build() {
    Text(`${this.value}`)
      .fontSize(50)
      .onClick(() => {
        this.value++
      })
  }
}

@Entry
@Component
struct Index {
  @State arr: number[] = [1, 2, 3];

  build() {
    Row() {
      Column() {
        Child({ value: this.arr[0] })
        Child({ value: this.arr[1] })
        Child({ value: this.arr[2] })

        Divider().height(5)

        ForEach(this.arr,
          (item: number) => {
            Child({ value: item })
          },
          (item: string) => item.toString()
        )
        Text('replace entire arr')
          .fontSize(50)
          .onClick(() => {
            // Both arrays contain item "3".
            this.arr = this.arr[0] == 1 ? [3, 4, 5] : [1, 2, 3];
          })
      }
    }
  }
}
```

<div style="text-align:center">
    <img src='../../images/image-basic/image19.png'>
</div>

Initial render creates six instances of the **Child** component. Each `@Prop` decorated variable is initialized with a copy of an array item. The **onclick** event handler of the **Child** component changes the local variable value.

Click **1** six times, **2** five times, and **3** four times on the page. The local values of all variables are then changed to **7**.

```
7
7
7
----
7
7
7
```

After **replace entire arr** is clicked, the following information is displayed:

```
3
4
5
----
7
4
5
```

- Changes made in the **Child** component are not synchronized to the parent component **Index**. Therefore, even if the values of the six instances of the **Child** component are **7**, the value of **this.arr** in the **Index** component is still **[1,2,3]**.

- After **replace entire arr** is clicked, if **this.arr[0] == 1** is true, **this.arr** is set to **[3, 4, 5]**.

- Because **this.arr[0]** has been changed, the **Child({value: this.arr[0]})** component synchronizes the update of **this.arr[0]** to the instance's @Prop decorated variable. The same happens for **Child({value: this.arr[1]})** and **Child({value: this.arr[2]})**.

- The change of **this.arr** causes **ForEach** to update: According to the diff algorithm, the array item with the ID **3** is retained in this update, array items with IDs **1** and **2** are deleted, and array items with IDs **4** and **5** are added. The array before and after the update is **[1, 2, 3]** and **[3, 4, 5]**, respectively. This implies that the **Child** instance generated for item **3** is moved to the first place, but not updated. In this case, the component value corresponding to **3** is **7**, and the final render result of **ForEach** is **7**, **4**, and **5**.

### Class Object Type @Prop Synced from @State Class Object Property in Parent Component
In a library with one book and two readers, each reader can mark the book as read, and the marking does not affect the other reader. Technically speaking, local changes to the @Prop decorated book object do not sync back to the `@State` decorated book in the Library component.

In this example, the `@Observed` decorator can be applied to the book class, but it is not mandatory. It is only needed for nested structures. 

```ts
class Book {
  public title: string;
  public pages: number;
  public readIt: boolean = false;

  constructor(title: string, pages: number) {
    this.title = title;
    this.pages = pages;
  }
}

@Component
struct ReaderComp {
  @Prop book: Book = new Book("", 0);

  build() {
    Row() {
      Text(this.book.title)
      Text(`...has${this.book.pages} pages!`)
      Text(`...${this.book.readIt ? "I have read" : 'I have not read it'}`)
        .onClick(() => this.book.readIt = true)
    }
  }
}

@Entry
@Component
struct Library {
  @State book: Book = new Book('100 secrets of C++', 765);

  build() {
    Column() {
      ReaderComp({ book: this.book })
      ReaderComp({ book: this.book })
    }
  }
}
```
The display is previewed as the following:
<div style="text-align:center">
    <img src='../../images/image-basic/image20.png'>
</div>

### Additional Information
For additional information please refer to the [ArkTS Prop Decorator](https://github.com/eclipse-oniro-mirrors/docs/blob/OpenHarmony-4.1-Release/en/application-dev/quick-start/arkts-prop.md)
