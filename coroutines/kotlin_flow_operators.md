
# Kotlin Flow Operators

## 1. Transformation Operators

These operators transform the emitted data from a `Flow`.

- **`map`**: Transforms each value emitted by the upstream `Flow`.
  ```kotlin
  flowOf(1, 2, 3).map { it * 2 }.collect { println(it) } 
  // Output: 2, 4, 6
  ```

- **`filter`**: Filters the values emitted by the upstream `Flow` based on a predicate.
  ```kotlin
  flowOf(1, 2, 3, 4).filter { it % 2 == 0 }.collect { println(it) }
  // Output: 2, 4
  ```

- **`transform`**: General-purpose operator that allows transforming, emitting multiple values, or none.
  ```kotlin
  flowOf(1, 2, 3).transform { emit(it * 2); emit(it + 1) }.collect { println(it) }
  // Output: 2, 2, 4, 3, 6, 4
  ```

- **`take`**: Takes only the first N elements from the `Flow`.
  ```kotlin
  flowOf(1, 2, 3, 4).take(2).collect { println(it) }
  // Output: 1, 2
  ```

- **`distinctUntilChanged`**: Emits only if the current item is different from the last emitted item.
  ```kotlin
  flowOf(1, 1, 2, 2, 3).distinctUntilChanged().collect { println(it) }
  // Output: 1, 2, 3
  ```

- **`distinctUntilChangedBy`**: Emits distinct items based on a specific key.
  ```kotlin
  flowOf(User(1, "John"), User(1, "Jane")).distinctUntilChangedBy { it.id }.collect { println(it) }
  // Output: User(1, "John")
  ```

- **`flatMapConcat`**: Maps each emitted value to a new `Flow`, and concatenates the results.
  ```kotlin
  flowOf(1, 2).flatMapConcat { flowOf(it * 2) }.collect { println(it) }
  // Output: 2, 4
  ```

- **`flatMapLatest`**: Maps to a new `Flow` and cancels the previous flow if a new value is emitted.
  ```kotlin
  flowOf(1, 2).flatMapLatest { flowOf(it * 2) }.collect { println(it) }
  // Output: 4 (because the first flow is canceled when 2 is emitted)
  ```

- **`scan`**: Reduces the flow to a running value, emitting each intermediate result.
  ```kotlin
  flowOf(1, 2, 3).scan(0) { acc, value -> acc + value }.collect { println(it) }
  // Output: 0, 1, 3, 6
  ```

- **`combine`**: Combines the latest values from multiple flows.
  ```kotlin
  val flow1 = flowOf(1, 2)
  val flow2 = flowOf("A", "B")
  flow1.combine(flow2) { a, b -> "$a$b" }.collect { println(it) }
  // Output: 1A, 2B
  ```

- **`zip`**: Combines the values from two flows into pairs, emitting them as a new flow.
  ```kotlin
  flowOf(1, 2).zip(flowOf("A", "B")) { a, b -> "$a$b" }.collect { println(it) }
  // Output: 1A, 2B
  ```

---

## 2. Terminal Operators

These operators are used to collect and consume the values from a `Flow`.

- **`collect`**: Collects the flow and triggers the execution of the flow. This is the most commonly used terminal operator.
  ```kotlin
  flowOf(1, 2, 3).collect { println(it) }
  // Output: 1, 2, 3
  ```

- **`toList`**: Collects all the values emitted by the flow into a list.
  ```kotlin
  val result = flowOf(1, 2, 3).toList()
  // Output: [1, 2, 3]
  ```

- **`toSet`**: Collects all the values into a set, which removes duplicates.
  ```kotlin
  val result = flowOf(1, 1, 2, 2, 3).toSet()
  // Output: [1, 2, 3]
  ```

- **`first`**: Collects the first value and cancels the flow.
  ```kotlin
  val result = flowOf(1, 2, 3).first()
  // Output: 1
  ```

- **`single`**: Collects a single value, and throws an error if there are more or fewer values.
  ```kotlin
  val result = flowOf(1).single()
  // Output: 1
  ```

- **`reduce`**: Reduces the flow to a single value by combining emissions.
  ```kotlin
  val result = flowOf(1, 2, 3).reduce { acc, value -> acc + value }
  // Output: 6
  ```

- **`fold`**: Similar to `reduce`, but starts with an initial value.
  ```kotlin
  val result = flowOf(1, 2, 3).fold(10) { acc, value -> acc + value }
  // Output: 16
  ```

---

## 3. Side-effect Operators

These operators are used to perform side-effects, such as logging or updating state, without modifying the flow.

- **`onEach`**: Executes a given action on each value emitted, without modifying it.
  ```kotlin
  flowOf(1, 2, 3).onEach { println("Received: $it") }.collect()
  // Output: Received: 1, Received: 2, Received: 3
  ```

- **`onCompletion`**: Executes an action when the flow completes (either successfully or with an exception).
  ```kotlin
  flowOf(1, 2, 3).onCompletion { println("Flow completed") }.collect { println(it) }
  // Output: 1, 2, 3, Flow completed
  ```

- **`onStart`**: Executes an action when the flow starts, before any emissions.
  ```kotlin
  flowOf(1, 2, 3).onStart { println("Flow started") }.collect { println(it) }
  // Output: Flow started, 1, 2, 3
  ```

- **`catch`**: Catches exceptions that occur in the flow and handles them.
  ```kotlin
  flow { emit(1); throw RuntimeException("Error") }
      .catch { println("Caught: $it") }
      .collect { println(it) }
  // Output: 1, Caught: java.lang.RuntimeException: Error
  ```

---

### Quick Reference for Common Operators

#### Transformation Operators:
- `map`: Transforms values.
- `filter`: Filters values.
- `transform`: General-purpose transformation.
- `flatMapConcat`: Flattens flows in order.
- `flatMapLatest`: Cancels the previous flow if a new one starts.
- `scan`: Accumulates a value over emissions.
- `distinctUntilChanged`: Emits only distinct values.
- `combine`: Combines multiple flows.
- `zip`: Pairs emissions from two flows.

#### Terminal Operators:
- `collect`: Collects and consumes values.
- `toList`: Converts flow to a list.
- `toSet`: Converts flow to a set.
- `first`: Retrieves the first value.
- `single`: Ensures the flow has exactly one value.
- `reduce`: Reduces the flow to a single value.
- `fold`: Reduces the flow with an initial value.

#### Side-effect Operators:
- `onEach`: Performs an action on each emission.
- `onStart`: Executes an action when the flow starts.
- `onCompletion`: Executes an action when the flow completes.
- `catch`: Handles exceptions.

By knowing these operators, you can efficiently process data streams using Kotlin `Flow` and leverage its reactive capabilities!
