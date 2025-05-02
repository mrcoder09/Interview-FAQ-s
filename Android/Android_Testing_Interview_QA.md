# Android Testing Interview Q&A with Explanations and Code Examples

---

### 1. **What are the types of testing in Android?**

In Android, testing is categorized based on what part of the system is being tested:

- **Unit Testing**: Validates individual units of code such as functions or classes. It's fast and runs on the JVM using JUnit or MockK/Mockito.
- **Integration Testing**: Checks the integration between different units/modules like a ViewModel and a Repository.
- **UI Testing**: Focuses on user interactions and UI flows using frameworks like Espresso or Jetpack Compose Test APIs.
- **End-to-End Testing**: Simulates real-world usage on actual devices, often run on Firebase Test Lab or Appium.

---

### 2. **What is the difference between local unit tests and instrumented tests?**

- **Local Unit Tests** run on your development machine using the JVM. These are fast and used for business logic validation.

- **Instrumented Tests** run on real or emulated Android devices. These are necessary for UI, database, and framework-level testing.

```kotlin
// Example Local Unit Test
@Test
fun sum_isCorrect() {
    assertEquals(4, 2 + 2)
}
```

---

### 3. **What is Espresso in Android testing?**

Espresso is Google's UI testing framework for Android. It helps simulate user interactions with the UI.

```kotlin
// Click a button and check text visibility
onView(withId(R.id.loginButton)).perform(click())
onView(withText("Welcome")).check(matches(isDisplayed()))
```

Espresso provides automatic synchronization with UI thread which avoids flakiness.

---

### 4. **How do you write a unit test in Android using JUnit and Mockito?**

You use `@Test` from JUnit and mock dependencies using Mockito.

```kotlin
class LoginViewModelTest {

    private lateinit var viewModel: LoginViewModel
    private val repository = mock(LoginRepository::class.java)

    @Before
    fun setup() {
        viewModel = LoginViewModel(repository)
    }

    @Test
    fun `login success updates LiveData`() {
        `when`(repository.login("user", "pass")).thenReturn(true)
        val result = viewModel.login("user", "pass")
        assertTrue(result)
    }
}
```

This helps isolate your ViewModel logic from the real repository.

---

### 5. **What is Robolectric?**

Robolectric allows you to run Android tests on the JVM without an emulator or device. It shadows Android framework classes.

```kotlin
@RunWith(RobolectricTestRunner::class)
class MainActivityTest {

    @Test
    fun buttonClick_shouldShowText() {
        val activity = Robolectric.buildActivity(MainActivity::class.java).setup().get()
        activity.findViewById<Button>(R.id.button).performClick()
        val text = activity.findViewById<TextView>(R.id.textView).text
        assertEquals("Clicked!", text)
    }
}
```

---

### 6. **What libraries are commonly used for testing in Android?**

- **JUnit** – for basic unit testing.
- **Mockito/MockK** – for mocking dependencies.
- **Espresso** – for UI tests.
- **Robolectric** – to run Android tests on JVM.
- **Truth / AssertJ** – for fluent assertions.
- **Compose Test APIs** – for Jetpack Compose UI testing.

---

### 7. **How do you test LiveData and ViewModel?**

Use `InstantTaskExecutorRule` to make LiveData synchronous in tests.

```kotlin
@get:Rule
val rule = InstantTaskExecutorRule()

@Test
fun `LiveData emits expected value`() {
    viewModel.someLiveData.observeForever(observer)
    viewModel.loadData()
    verify(observer).onChanged(expectedValue)
}
```

This allows you to verify LiveData updates instantly.

---

### 8. **How do you test Jetpack Compose UI?**

Use `createComposeRule()` to set the Compose UI content and then query UI nodes.

```kotlin
@get:Rule
val composeTestRule = createComposeRule()

@Test
fun buttonClick_showsText() {
    composeTestRule.setContent {
        MyComposable()
    }

    composeTestRule.onNodeWithText("Click me").performClick()
    composeTestRule.onNodeWithText("You clicked").assertIsDisplayed()
}
```

Jetpack Compose testing is powerful and expressive for declarative UIs.

---

### 9. **How do you mock dependencies in Android tests?**

Use **Dependency Injection** to inject fake/mock versions of dependencies. With Hilt, you can use `@TestInstallIn` to override modules.

```kotlin
val fakeRepository = mock(MyRepository::class.java)
`when`(fakeRepository.getData()).thenReturn("Test Data")
viewModel = MyViewModel(fakeRepository)
```

Mocking isolates the unit under test from external components.

---

### 10. **Best practices for testing in Android?**

- Test **business logic** using local unit tests (fast and reliable).
- Use **mocking frameworks** to isolate dependencies.
- Prefer **MVVM or clean architecture** to increase testability.
- Limit **UI tests** to essential paths to reduce flakiness.
- Automate tests in **CI/CD** using GitHub Actions or Bitrise.

---