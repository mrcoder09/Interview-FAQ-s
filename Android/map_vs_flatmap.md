## 🔄 `map` vs `flatMap` in Kotlin

In Kotlin, both `map` and `flatMap` are used to transform collections, but they behave differently.

---

### ✅ Key Difference:
- **`map`** transforms **each item** and returns a collection **of results**.
- **`flatMap`** transforms each item **into a collection**, and then **flattens** the result into a **single list**.

---

### 🔹 `map` Example

```kotlin
val numbers = listOf(1, 2, 3)

val mapped = numbers.map { listOf(it, it * 10) }
// Output: [[1, 10], [2, 20], [3, 30]]
println(mapped)

val numbers = listOf(1, 2, 3)

val flatMapped = numbers.flatMap { listOf(it, it * 10) }
// Output: [1, 10, 2, 20, 3, 30]
println(flatMapped)
