# SOLID Principles in Plain English for Android Developers

The SOLID principles are five design guidelines that help Android developers create maintainable, scalable, and flexible apps. Below, each principle is explained in plain English with Kotlin code examples tailored for Android development.

---

## 1. **S** - Single Responsibility Principle (SRP)

**Plain English**: A class should have only one job. If it does multiple things, it’s harder to maintain.

**Example**: A class handling both user data storage and UI updates violates SRP.

**Bad Code** (Violates SRP):
```kotlin
class UserManager {
    fun saveUserToDatabase(user: User) {
        // Save user to database
        println("Saving user: ${user.name}")
    }

    fun updateUserUI(user: User) {
        // Update UI with user data
        println("Updating UI for user: ${user.name}")
    }
}
```

**Good Code** (Follows SRP):
```kotlin
class UserRepository {
    fun saveUserToDatabase(user: User) {
        // Save user to database
        println("Saving user: ${user.name}")
    }
}

class UserViewUpdater {
    fun updateUserUI(user: User) {
        // Update UI with user data
        println("Updating UI for user: ${user.name}")
    }
}
```

**Why it’s better**: Each class has one responsibility (`UserRepository` for data, `UserViewUpdater` for UI). Changes to the database logic only affect `UserRepository`.

---

## 2. **O** - Open/Closed Principle (OCP)

**Plain English**: Code should be open for adding new features but closed for changing existing code.

**Example**: A discount calculator for different customer types. Hardcoding logic makes it hard to add new types.

**Bad Code** (Violates OCP):
```kotlin
class DiscountCalculator {
    fun calculateDiscount(customerType: String, amount: Double): Double {
        return when (customerType) {
            "Regular" -> amount * 0.1
            "Premium" -> amount * 0.2
            else -> 0.0
        }
    }
}
```

**Good Code** (Follows OCP):
```kotlin
interface Discount {
    fun calculate(amount: Double): Double
}

class RegularDiscount : Discount {
    override fun calculate(amount: Double): Double = amount * 0.1
}

class PremiumDiscount : Discount {
    override fun calculate(amount: Double): Double = amount * 0.2
}

class DiscountCalculator {
    fun calculateDiscount(discount: Discount, amount: Double): Double {
        return discount.calculate(amount)
    }
}
```

**Why it’s better**: To add a new discount (e.g., `VIPDiscount`), create a new class implementing `Discount` without modifying `DiscountCalculator`.

---

## 3. **L** - Liskov Substitution Principle (LSP)

**Plain English**: Subclasses should work seamlessly in place of their base class without breaking the app.

**Example**: A `PaymentMethod` base class with `CreditCard` and `PayPal` subclasses should allow substitution.

**Bad Code** (Violates LSP):
```kotlin
open class PaymentMethod {
    open fun processPayment(amount: Double) {
        println("Processing payment of $amount")
    }
}

class CreditCard : PaymentMethod() {
    override fun processPayment(amount: Double) {
        println("Processing credit card payment of $amount")
    }
}

class Cash : PaymentMethod() {
    override fun processPayment(amount: Double) {
        throw UnsupportedOperationException("Cash payments not supported online")
    }
}
```

**Problem**: `Cash` breaks code expecting `PaymentMethod` to work.

**Good Code** (Follows LSP):
```kotlin
interface OnlinePaymentMethod {
    fun processPayment(amount: Double)
}

class CreditCard : OnlinePaymentMethod {
    override fun processPayment(amount: Double) {
        println("Processing credit card payment of $amount")
    }
}

class PayPal : OnlinePaymentMethod {
    override fun processPayment(amount: Double) {
        println("Processing PayPal payment of $amount")
    }
}
```

**Why it’s better**: All `OnlinePaymentMethod` implementations can be used interchangeably. `Cash` is excluded as it doesn’t fit.

---

## 4. **I** - Interface Segregation Principle (ISP)

**Plain English**: Don’t force classes to implement methods they don’t need. Use small, specific interfaces.

**Example**: A media playback interface shouldn’t force audio players to implement video methods.

**Bad Code** (Violates ISP):
```kotlin
interface MediaPlayer {
    fun playAudio()
    fun playVideo()
    fun adjustVolume()
}

class AudioPlayer : MediaPlayer {
    override fun playAudio() {
        println("Playing audio")
    }

    override fun playVideo() {
        // AudioPlayer doesn't need this
        throw UnsupportedOperationException("Not supported")
    }

    override fun adjustVolume() {
        println("Adjusting volume")
    }
}
```

**Good Code** (Follows ISP):
```kotlin
interface AudioPlayable {
    fun playAudio()
}

interface VideoPlayable {
    fun playVideo()
}

interface VolumeAdjustable {
    fun adjustVolume()
}

class AudioPlayer : AudioPlayable, VolumeAdjustable {
    override fun playAudio() {
        println("Playing audio")
    }

    override fun adjustVolume() {
        println("Adjusting volume")
    }
}
```

**Why it’s better**: `AudioPlayer` only implements relevant methods. A `VideoPlayer` can implement `VideoPlayable` separately.

---

## 5. **D** - Dependency Inversion Principle (DIP)

**Plain English**: High-level modules (business logic) shouldn’t depend on low-level modules (e.g., database). Both should depend on interfaces.

**Example**: A `UserManager` fetching user data shouldn’t be tied to a specific database.

**Bad Code** (Violates DIP):
```kotlin
class SQLiteDatabase {
    fun fetchUser(): User = User("John")
}

class UserManager {
    private val database = SQLiteDatabase()

    fun getUser(): User {
        return database.fetchUser()
    }
}
```

**Problem**: `UserManager` is coupled to `SQLiteDatabase`.

**Good Code** (Follows DIP):
```kotlin
interface UserDataSource {
    fun fetchUser(): User
}

class SQLiteDatabase : UserDataSource {
    override fun fetchUser(): User = User("John")
}

class UserManager(private val dataSource: UserDataSource) {
    fun getUser(): User {
        return dataSource.fetchUser()
    }
}
```

**Why it’s better**: `UserManager` depends on `UserDataSource`. You can swap `SQLiteDatabase` for another implementation (e.g., `RoomDatabase`) without changing `UserManager`. Use Dagger or Hilt for injection in Android.

---

## Android Context

SOLID principles are crucial for Android development:

- **SRP**: Keeps ViewModels, Repositories, and UI components focused and testable.
- **OCP**: Allows adding new features (e.g., new screens) without rewriting code.
- **LSP**: Ensures Fragments or Adapters can be swapped without issues.
- **ISP**: Prevents bloated interfaces in media or permission systems.
- **DIP**: Enables modularity with dependency injection (e.g., Hilt).

## Example: User Profile Feature

Here’s a combined example for a user profile feature in an Android app:

```kotlin
// Data class for User
data class User(val name: String)

// SRP: Separate data and UI logic
interface UserRepository {
    fun getUser(): User
}

class RoomUserRepository : UserRepository {
    override fun getUser(): User = User("Alice")
}

class UserViewModel(private val repository: UserRepository) {
    fun fetchUser(): User = repository.getUser()
}

// OCP: Extendable user types
interface UserType {
    fun getUserInfo(): String
}

class RegularUser : UserType {
    override fun getUserInfo(): String = "Regular User"
}

class PremiumUser : UserType {
    override fun getUserInfo(): String = "Premium User"
}

// LSP: User types can be used interchangeably
class UserProfileDisplay {
    fun displayUserInfo(userType: UserType) {
        println(userType.getUserInfo())
    }
}

// ISP: Specific interfaces for UI
interface UserDisplay {
    fun showUser(user: User)
}

class ProfileFragment : UserDisplay {
    override fun showUser(user: User) {
        // Update UI (e.g., TextView)
        println("Showing user: ${user.name}")
    }
}

// DIP: Inject dependencies
class ProfileActivity : AppCompatActivity() {
    private val userRepository: UserRepository = RoomUserRepository()
    private val viewModel: UserViewModel = UserViewModel(userRepository)

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_profile)
        val user = viewModel.fetchUser()
        ProfileFragment().showUser(user)
    }
}
```

This structure is clean and modular, allowing easy swaps of components (e.g., repository or user type).

---

## Formatting SOLID Principles in Markdown

To include the SOLID principles in a Markdown file with proper formatting (e.g., for a blog or documentation), use this structure:

```markdown
# Understanding SOLID Principles for Android Developers

SOLID principles guide developers to write clean, maintainable code. Here's how they apply to Android development with Kotlin examples.

## 1. Single Responsibility Principle (SRP)

**Definition**: A class should have one responsibility.

**Example**:

*Bad Code* (Violates SRP):
\`\`\`kotlin
class UserManager {
    fun saveUserToDatabase(user: User) {
        println("Saving user: ${user.name}")
    }

    fun updateUserUI(user: User) {
        println("Updating UI for user: ${user.name}")
    }
}
\`\`\`

*Good Code* (Follows SRP):
\`\`\`kotlin
class UserRepository {
    fun saveUserToDatabase(user: User) {
        println("Saving user: ${user.name}")
    }
}

class UserViewUpdater {
    fun updateUserUI(user: User) {
        println("Updating UI for user: ${user.name}")
    }
}
\`\`\`

**Why it’s better**: Each class has a single responsibility, making maintenance easier.

## 2. Open/Closed Principle (OCP)

**Definition**: Code should be open for extension but closed for modification.

**Example**:

*Bad Code* (Violates OCP):
\`\`\`kotlin
class DiscountCalculator {
    fun calculateDiscount(customerType: String, amount: Double): Double {
        return when (customerType) {
            "Regular" -> amount * 0.1
            "Premium" -> amount * 0.2
            else -> 0.0
        }
    }
}
\`\`\`

*Good Code* (Follows OCP):
\`\`\`kotlin
interface Discount {
    fun calculate(amount: Double): Double
}

class RegularDiscount : Discount {
    override fun calculate(amount: Double): Double = amount * 0.1
}

class PremiumDiscount : Discount {
    override fun calculate(amount: Double): Double = amount * 0.2
}

class DiscountCalculator {
    fun calculateDiscount(discount: Discount, amount: Double): Double {
        return discount.calculate(amount)
    }
}
\`\`\`

**Why it’s better**: New discounts can be added without changing existing code.

## 3. Liskov Substitution Principle (LSP)

**Definition**: Subclasses should be substitutable for their base class.

**Example**:

*Bad Code* (Violates LSP):
\`\`\`kotlin
open class PaymentMethod {
    open fun processPayment(amount: Double) {
        println("Processing payment of $amount")
    }
}

class CreditCard : PaymentMethod() {
    override fun processPayment(amount: Double) {
        println("Processing credit card payment of $amount")
    }
}

class Cash : PaymentMethod() {
    override fun processPayment(amount: Double) {
        throw UnsupportedOperationException("Cash payments not supported online")
    }
}
\`\`\`

*Good Code* (Follows LSP):
\`\`\`kotlin
interface OnlinePaymentMethod {
    fun processPayment(amount: Double)
}

class CreditCard : OnlinePaymentMethod {
    override fun processPayment(amount: Double) {
        println("Processing credit card payment of $amount")
    }
}

class PayPal : OnlinePaymentMethod {
    override fun processPayment(amount: Double) {
        println("Processing PayPal payment of $amount")
    }
}
\`\`\`

**Why it’s better**: Subclasses work interchangeably without errors.

## 4. Interface Segregation Principle (ISP)

**Definition**: Classes shouldn’t implement methods they don’t need.

**Example**:

*Bad Code* (Violates ISP):
\`\`\`kotlin
interface MediaPlayer {
    fun playAudio()
    fun playVideo()
    fun adjustVolume()
}

class AudioPlayer : MediaPlayer {
    override fun playAudio() {
        println("Playing audio")
    }

    override fun playVideo() {
        throw UnsupportedOperationException("Not supported")
    }

    override fun adjustVolume() {
        println("Adjusting volume")
    }
}
\`\`\`

*Good Code* (Follows ISP):
\`\`\`kotlin
interface AudioPlayable {
    fun playAudio()
}

interface VideoPlayable {
    fun playVideo()
}

interface VolumeAdjustable {
    fun adjustVolume()
}

class AudioPlayer : AudioPlayable, VolumeAdjustable {
    override fun playAudio() {
        println("Playing audio")
    }

    override fun adjustVolume() {
        println("Adjusting volume")
    }
}
\`\`\`

**Why it’s better**: Classes only implement relevant methods.

## 5. Dependency Inversion Principle (DIP)

**Definition**: Depend on abstractions, not concrete implementations.

**Example**:

*Bad Code* (Violates DIP):
\`\`\`kotlin
class SQLiteDatabase {
    fun fetchUser(): User = User("John")
}

class UserManager {
    private val database = SQLiteDatabase()

    fun getUser(): User {
        return database.fetchUser()
    }
}
\`\`\`

*Good Code* (Follows DIP):
\`\`\`kotlin
interface UserDataSource {
    fun fetchUser(): User
}

class SQLiteDatabase : UserDataSource {
    override fun fetchUser(): User = User("John")
}

class UserManager(private val dataSource: UserDataSource) {
    fun getUser(): User {
        return dataSource.fetchUser()
    }
}
\`\`\`

**Why it’s better**: Swapping implementations is easy with dependency injection.

---

# SOLID Principles Resources

- [GeeksforGeeks: SOLID Principle in Programming (with Real-Life Examples)](https://www.geeksforgeeks.org/solid-principle-in-programming-understand-with-real-life-examples/)
- [YouTube Playlist: SOLID Principles Explained](https://www.youtube.com/watch?v=t8VTLxMsufU&list=PLaEHa-6HyZqn2SFCUhyRu7ZL8ZnUsyT69)
- [Baeldung: SOLID Principles](https://www.baeldung.com/solid-principles)
```
