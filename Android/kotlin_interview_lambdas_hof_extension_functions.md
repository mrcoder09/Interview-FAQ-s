
# Kotlin Interview Topics: Lambdas, Higher-Order Functions, and Extension Functions

## 1. Lambdas in Kotlin
Lambdas in Kotlin are anonymous functions that can be treated as expressions. They are concise and can be passed around as values.

- **Definition**: A lambda is a function literal (an unnamed function) that can be passed as an expression. It is commonly used when the logic is simple and we want to avoid defining a full function.
- **Syntax**: `{ parameter(s) -> body }`
  
  **Example**:
  ```kotlin
  val sum = { x: Int, y: Int -> x + y }
  println(sum(2, 3)) // Output: 5
  ```

- **Usage**: Lambdas are useful in scenarios such as collections (e.g., `map`, `filter`), event listeners, and callbacks.

## 2. Higher-Order Functions
Higher-order functions are functions that take other functions as parameters or return them as results. They enhance the flexibility and reusability of code.

- **Definition**: A higher-order function is a function that either takes a function as an argument or returns a function. This is a key feature in Kotlin, enabling a functional programming approach.

- **Example**:
  ```kotlin
  fun higherOrderFunction(operation: (Int, Int) -> Int, a: Int, b: Int): Int {
      return operation(a, b)
  }

  val result = higherOrderFunction({ x, y -> x + y }, 3, 5)
  println(result) // Output: 8
  ```

- **Usage**: Higher-order functions simplify code by allowing you to abstract common logic, such as applying an operation on a list or transforming data.

## 3. Extension Functions
Extension functions in Kotlin allow you to extend a class with new functionality without modifying its source code. This is achieved by prefixing the function with the class type.

- **Definition**: An extension function adds new functions to a class without inheriting from the class or modifying its code.
  
- **Syntax**: `fun ClassName.functionName(): ReturnType { }`
  
  **Example**:
  ```kotlin
  fun String.reverse(): String {
      return this.reversed()
  }

  println("hello".reverse()) // Output: "olleh"
  ```

- **Usage**: Extension functions are great for enhancing third-party libraries, organizing utility functions, and avoiding boilerplate code.

## Combining These Concepts
Often, these features are used together in Kotlin. For example, you might pass a lambda to a higher-order function that is added as an extension to a class:

**Example**:
```kotlin
fun String.transform(transformer: (String) -> String): String {
    return transformer(this)
}

val result = "hello".transform { it.uppercase() }
println(result) // Output: HELLO
```

### Follow-up Questions
- **Why Use Higher-Order Functions and Lambdas?**
  They promote code reusability, concise code, and help implement functional programming techniques such as map-reduce, filtering, etc.

- **Why Use Extension Functions?**
  They allow adding functionalities to existing classes in a clean way, without needing inheritance or utility methods.
