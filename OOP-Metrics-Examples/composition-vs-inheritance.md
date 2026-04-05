# Composition vs Inheritance - Complete Guide

## Problem: Inheritance Misuse

**BAD EXAMPLE:** Using inheritance for code reuse when composition is more appropriate

### Base Class with Utilities

```java
class Logger {
    public void log(String message) {
        System.out.println("[LOG] " + message);
    }
    
    public void logError(String message) {
        System.err.println("[ERROR] " + message);
    }
}
```

### ❌ BAD: Inheritance Just to Reuse Logging

```java
class UserService extends Logger {
    public void createUser(String username) {
        log("Creating user: " + username);
        // User creation logic
    }
}

class OrderService extends Logger {
    public void createOrder(String orderId) {
        log("Creating order: " + orderId);
        // Order creation logic
    }
}
```

### PROBLEMS:

1. **UserService is NOT a Logger** (violates 'is-a' relationship)
2. **Exposes all Logger methods publicly** (log, logError)
3. **Can only extend one class** (no multiple inheritance)
4. **Tight coupling** - changes to Logger affect all subclasses
5. **Cannot swap logging implementation** easily

---

## Solution 1: Composition for Behavior Assembly

**✅ GOOD:** Use composition when you need behavior reuse without true 'is-a' relationship

```java
class Logger_Good {
    public void log(String message) {
        System.out.println("[LOG] " + message);
    }
    
    public void logError(String message) {
        System.err.println("[ERROR] " + message);
    }
}

// ✅ GOOD: HAS-A relationship (composition)
class UserService_Good {
    private final Logger_Good logger;  // Composition
    
    public UserService_Good(Logger_Good logger) {
        this.logger = logger;
    }
    
    public void createUser(String username) {
        logger.log("Creating user: " + username);
        // User creation logic
    }
    
    // Logger methods NOT exposed publicly
}

// ✅ GOOD: HAS-A relationship
class OrderService_Good {
    private final Logger_Good logger;  // Composition
    
    public OrderService_Good(Logger_Good logger) {
        this.logger = logger;
    }
    
    public void createOrder(String orderId) {
        logger.log("Creating order: " + orderId);
        // Order creation logic
    }
}
```

### BENEFITS:

✅ **Clear relationship:** Service HAS-A Logger

✅ **Encapsulation:** Logger methods not exposed

✅ **Flexibility:** Can inject different logger implementations

✅ **Testability:** Can mock logger easily

✅ **Multiple behaviors:** Can compose multiple components

---

## Composition: Assembling Multiple Behaviors

Example: Building a rich service with multiple behaviors

```java
interface Logger {
    void log(String message);
}

interface Validator {
    boolean validate(String input);
}

interface Notifier {
    void notify(String message);
}

interface MetricsCollector {
    void recordMetric(String name, double value);
}
```

### Implementations

```java
class ConsoleLogger implements Logger {
    @Override
    public void log(String message) {
        System.out.println("[LOG] " + message);
    }
}

class EmailValidator implements Validator {
    @Override
    public boolean validate(String input) {
        return input != null && input.contains("@");
    }
}

class EmailNotifier implements Notifier {
    @Override
    public void notify(String message) {
        System.out.println("📧 Sending email: " + message);
    }
}

class PrometheusMetrics implements MetricsCollector {
    @Override
    public void recordMetric(String name, double value) {
        System.out.println("📊 Metric: " + name + " = " + value);
    }
}
```

### ✅ Service Composed of Multiple Behaviors

```java
class RegistrationService {
    private final Logger logger;
    private final Validator validator;
    private final Notifier notifier;
    private final MetricsCollector metrics;
    
    // Dependency Injection - assembling behaviors
    public RegistrationService(
        Logger logger,
        Validator validator,
        Notifier notifier,
        MetricsCollector metrics
    ) {
        this.logger = logger;
        this.validator = validator;
        this.notifier = notifier;
        this.metrics = metrics;
    }
    
    public boolean registerUser(String email, String password) {
        long startTime = System.currentTimeMillis();
        logger.log("Registration attempt for: " + email);
        
        // Use validator
        if (!validator.validate(email)) {
            logger.log("Invalid email: " + email);
            return false;
        }
        
        // Registration logic
        logger.log("User registered: " + email);
        
        // Use notifier
        notifier.notify("Welcome! Your account has been created.");
        
        // Use metrics
        long duration = System.currentTimeMillis() - startTime;
        metrics.recordMetric("registration_duration_ms", duration);
        
        return true;
    }
}
```

### Using the Composed Service

```java
class CompositionExample {
    public static void main(String[] args) {
        // Assemble the service with desired behaviors
        RegistrationService service = new RegistrationService(
            new ConsoleLogger(),
            new EmailValidator(),
            new EmailNotifier(),
            new PrometheusMetrics()
        );
        
        service.registerUser("john@example.com", "password123");
        
        // Easy to swap implementations
        RegistrationService testService = new RegistrationService(
            new NullLogger(),           // Different logger for testing
            new EmailValidator(),
            new MockNotifier(),         // Mock notifier
            new InMemoryMetrics()       // In-memory metrics
        );
    }
}
```

---

## True IS-A Relationship: When to Use Inheritance

**✅ Use inheritance for TRUE 'is-a' relationships** when specialization makes sense

```java
// Base class with stable contract
abstract class Animal {
    private String name;
    private int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // Common behavior
    public void eat() {
        System.out.println(name + " is eating");
    }
    
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
    
    // Abstract method - subclasses must implement
    public abstract void makeSound();
    
    // Template method pattern
    public final void dailyRoutine() {
        wakeUp();
        eat();
        makeSound();
        sleep();
    }
    
    protected void wakeUp() {
        System.out.println(name + " wakes up");
    }
    
    // Getters
    public String getName() { return name; }
    public int getAge() { return age; }
}
```

### ✅ Dog IS-A Animal (True Specialization)

```java
class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        super(name, age);
        this.breed = breed;
    }
    
    @Override
    public void makeSound() {
        System.out.println(getName() + " barks: Woof! Woof!");
    }
    
    // Dog-specific behavior
    public void fetch() {
        System.out.println(getName() + " is fetching the ball");
    }
    
    public String getBreed() { return breed; }
}
```

### ✅ Cat IS-A Animal (True Specialization)

```java
class Cat extends Animal {
    private boolean indoor;
    
    public Cat(String name, int age, boolean indoor) {
        super(name, age);
        this.indoor = indoor;
    }
    
    @Override
    public void makeSound() {
        System.out.println(getName() + " meows: Meow!");
    }
    
    // Cat-specific behavior
    public void scratch() {
        System.out.println(getName() + " is scratching");
    }
    
    public boolean isIndoor() { return indoor; }
}
```

### Using Inheritance Correctly

```java
class InheritanceExample {
    public static void main(String[] args) {
        Animal dog = new Dog("Buddy", 3, "Golden Retriever");
        Animal cat = new Cat("Whiskers", 2, true);
        
        // Polymorphism - treat all animals uniformly
        List<Animal> animals = List.of(dog, cat);
        
        for (Animal animal : animals) {
            animal.dailyRoutine();
            System.out.println();
        }
        
        // Specific behavior
        if (dog instanceof Dog d) {
            d.fetch();
        }
        
        if (cat instanceof Cat c) {
            c.scratch();
        }
    }
}
```

---

## Strategy Pattern: Composition over Inheritance

### ❌ BAD: Using Inheritance for Different Behaviors

```java
// Bad: Inheritance hierarchy for payment methods
abstract class Payment {
    public abstract void processPayment(double amount);
}

class CreditCardPayment extends Payment {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing credit card payment: $" + amount);
    }
}

class PayPalPayment extends Payment {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing PayPal payment: $" + amount);
    }
}

// Problem: Adding new payment method requires new subclass
// Cannot change payment method at runtime easily
```

### ✅ GOOD: Strategy Pattern with Composition

```java
// Strategy interface
interface PaymentStrategy {
    boolean processPayment(double amount);
}

// Concrete strategies
class CreditCardPayment implements PaymentStrategy {
    private String cardNumber;
    
    public CreditCardPayment(String cardNumber) {
        this.cardNumber = cardNumber;
    }
    
    @Override
    public boolean processPayment(double amount) {
        System.out.println("Processing credit card payment: $" + amount);
        System.out.println("Card: ****" + cardNumber.substring(cardNumber.length() - 4));
        return true;
    }
}

class PayPalPayment implements PaymentStrategy {
    private String email;
    
    public PayPalPayment(String email) {
        this.email = email;
    }
    
    @Override
    public boolean processPayment(double amount) {
        System.out.println("Processing PayPal payment: $" + amount);
        System.out.println("Account: " + email);
        return true;
    }
}

class CryptoPayment implements PaymentStrategy {
    private String walletAddress;
    
    public CryptoPayment(String walletAddress) {
        this.walletAddress = walletAddress;
    }
    
    @Override
    public boolean processPayment(double amount) {
        System.out.println("Processing crypto payment: $" + amount);
        System.out.println("Wallet: " + walletAddress);
        return true;
    }
}
```

### Context Class Using Strategy

```java
class ShoppingCart {
    private List<String> items = new ArrayList<>();
    private PaymentStrategy paymentStrategy;  // Composition
    
    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.paymentStrategy = strategy;
    }
    
    public void addItem(String item) {
        items.add(item);
    }
    
    public boolean checkout(double amount) {
        if (paymentStrategy == null) {
            System.out.println("Please select a payment method");
            return false;
        }
        
        return paymentStrategy.processPayment(amount);
    }
}
```

### Using the Strategy Pattern

```java
class StrategyExample {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();
        cart.addItem("Laptop");
        cart.addItem("Mouse");
        
        // Can change strategy at runtime!
        cart.setPaymentStrategy(new CreditCardPayment("1234567890123456"));
        cart.checkout(1200.0);
        
        // Switch to different payment method
        cart.setPaymentStrategy(new PayPalPayment("user@example.com"));
        cart.checkout(1200.0);
        
        // Add new payment method without changing existing code
        cart.setPaymentStrategy(new CryptoPayment("0x1234..."));
        cart.checkout(1200.0);
    }
}
```

---

## Fragile Base Class Problem

**Problem:** Changes to base class can break subclasses

### ❌ BAD: Fragile Base Class

```java
class ConnectionPool {
    protected int connectionCount = 0;
    
    public void addConnection() {
        connectionCount++;
        System.out.println("Connection added. Total: " + connectionCount);
    }
    
    public void removeConnection() {
        connectionCount--;
        System.out.println("Connection removed. Total: " + connectionCount);
    }
}

class DatabasePool extends ConnectionPool {
    @Override
    public void addConnection() {
        super.addConnection();
        System.out.println("Database-specific setup");
    }
    
    // Relies on internal implementation of parent
}

// PROBLEM: If ConnectionPool implementation changes, DatabasePool breaks!
```

### ✅ GOOD: Delegation via Composition

```java
interface Connection {
    void connect();
    void disconnect();
}

class DatabaseConnectionImpl implements Connection {
    private final String connectionString;
    private final int timeout;
    
    public DatabaseConnectionImpl(String connectionString, int timeout) {
        this.connectionString = connectionString;
        this.timeout = timeout;
    }
    
    @Override
    public void connect() {
        System.out.println("Connecting with timeout: " + timeout);
    }
    
    @Override
    public void disconnect() {
        System.out.println("Disconnecting");
    }
    
    // Can change implementation without breaking clients
    public void connectWithSSL(boolean useSSL) {
        System.out.println("Connecting with SSL: " + useSSL);
    }
}

// Delegation instead of inheritance
class MySQLConnectionWrapper implements Connection {
    private final DatabaseConnectionImpl delegate;
    
    public MySQLConnectionWrapper(DatabaseConnectionImpl delegate) {
        this.delegate = delegate;
    }
    
    @Override
    public void connect() {
        delegate.connect();  // Delegate to composed object
        System.out.println("MySQL specific setup");
    }
    
    @Override
    public void disconnect() {
        System.out.println("MySQL specific cleanup");
        delegate.disconnect();
    }
    
    // Not affected by changes to DatabaseConnectionImpl internals!
}
```

---

## Decision Tree: Composition vs Inheritance

### USE INHERITANCE WHEN:

✅ **True 'is-a' relationship** (Dog IS-A Animal)

✅ **Base class is stable** (rarely changes)

✅ **Specialization is natural and clear**

✅ **Liskov Substitution Principle applies**

✅ **Shallow hierarchy** (DIT ≤ 3)

✅ **Few subclasses** (NOC ≤ 7)

✅ **Need polymorphism**

### USE COMPOSITION WHEN:

✅ **'has-a' relationship** (Car HAS-A Engine)

✅ **Assembling behaviors** from multiple sources

✅ **Need flexibility** to swap implementations

✅ **Base class is volatile** (changes often)

✅ **Want to avoid** inheritance hierarchy

✅ **Need multiple "parents"** (behaviors)

✅ **Need better testability**

### EXAMPLES:

**Inheritance:**
- Animal → Dog, Cat, Bird
- Shape → Circle, Rectangle, Triangle
- Exception → IOException, SQLException
- Collection → List, Set, Map

**Composition:**
- Car HAS-A Engine, Wheels, Radio
- Service HAS-A Logger, Validator, Notifier
- Order HAS-A Customer, Items, Payment
- Application HAS-A Database, Cache, Queue

---

## Summary Guidelines

### FAVOR COMPOSITION:

✓ More flexible

✓ Easier to test

✓ Better encapsulation

✓ Avoids fragile base class problem

✓ Can combine multiple behaviors

### USE INHERITANCE SPARINGLY:

✓ Only for true 'is-a' relationships

✓ Keep hierarchies shallow (DIT ≤ 3)

✓ Limit subclasses (NOC ≤ 7)

✓ Ensure base class is stable

✓ Document contracts clearly

### RED FLAGS FOR INHERITANCE:

🚩 **Deep hierarchy** (DIT > 4)

🚩 **Many subclasses** (NOC > 10)

🚩 **Base class changes often**

🚩 **Inheritance just for code reuse**

🚩 **Not a true 'is-a' relationship**

---

## Real-World Example: E-Commerce System

**✅ GOOD DESIGN:** Mixing composition and inheritance appropriately

### Inheritance: True Specialization

```java
abstract class Product {
    private String id;
    private String name;
    private double price;
    
    public Product(String id, String name, double price) {
        this.id = id;
        this.name = name;
        this.price = price;
    }
    
    public abstract double calculateShipping();
    
    public String getId() { return id; }
    public String getName() { return name; }
    public double getPrice() { return price; }
}

class PhysicalProduct extends Product {
    private double weight;
    
    public PhysicalProduct(String id, String name, double price, double weight) {
        super(id, name, price);
        this.weight = weight;
    }
    
    @Override
    public double calculateShipping() {
        return weight * 2.0;  // $2 per kg
    }
}

class DigitalProduct extends Product {
    public DigitalProduct(String id, String name, double price) {
        super(id, name, price);
    }
    
    @Override
    public double calculateShipping() {
        return 0.0;  // No shipping for digital
    }
}
```

### Composition: Assembling Behaviors

```java
class Order {
    private final String orderId;
    private final Customer customer;           // HAS-A Customer
    private final List<Product> products;      // HAS-A Products
    private final PaymentStrategy payment;     // HAS-A PaymentStrategy
    private final ShippingCalculator shipping; // HAS-A ShippingCalculator
    private final Logger logger;               // HAS-A Logger
    
    public Order(
        String orderId,
        Customer customer,
        List<Product> products,
        PaymentStrategy payment,
        ShippingCalculator shipping,
        Logger logger
    ) {
        this.orderId = orderId;
        this.customer = customer;
        this.products = List.copyOf(products);
        this.payment = payment;
        this.shipping = shipping;
        this.logger = logger;
    }
    
    public boolean process() {
        logger.log("Processing order: " + orderId);
        
        double total = calculateTotal();
        boolean paid = payment.processPayment(total);
        
        if (paid) {
            logger.log("Order paid: " + orderId);
            return true;
        }
        
        return false;
    }
    
    private double calculateTotal() {
        double subtotal = products.stream()
            .mapToDouble(Product::getPrice)
            .sum();
        
        double shippingCost = shipping.calculate(products);
        
        return subtotal + shippingCost;
    }
}
```

### Supporting Classes

```java
class Customer {
    private String id;
    private String name;
    
    public Customer(String id, String name) {
        this.id = id;
        this.name = name;
    }
}

interface ShippingCalculator {
    double calculate(List<Product> products);
}

class StandardShipping implements ShippingCalculator {
    @Override
    public double calculate(List<Product> products) {
        return products.stream()
            .mapToDouble(Product::calculateShipping)
            .sum();
    }
}
```

### Using the E-Commerce System

```java
class ECommerceExample {
    public static void main(String[] args) {
        // Create order with composed behaviors
        List<Product> products = List.of(
            new PhysicalProduct("P1", "Laptop", 1000.0, 2.5),
            new DigitalProduct("D1", "Software License", 200.0)
        );
        
        Order order = new Order(
            "ORD-001",
            new Customer("C1", "John Doe"),
            products,
            new CreditCardPayment("1234567890123456"),
            new StandardShipping(),
            new ConsoleLogger()
        );
        
        boolean success = order.process();
        System.out.println("Order processed: " + success);
    }
}
```

---

## Key Takeaways

✓ **"Favor composition over inheritance"** is a fundamental OOP principle

✓ **Composition** provides flexibility, testability, and maintainability

✓ **Inheritance** should only be used for true specialization relationships

✓ Use **Strategy Pattern** to replace inheritance-based behavior variation

✓ Avoid the **Fragile Base Class Problem** by preferring delegation

✓ Keep inheritance hierarchies **shallow** (DIT ≤ 3) and **narrow** (NOC ≤ 7)

✓ **Test** your design by asking: "Is this truly an 'is-a' relationship?"
