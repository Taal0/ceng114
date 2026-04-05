# LCOM (Lack of Cohesion of Methods) Examples

## Bad Example - Low Cohesion (High LCOM)

This class has **LOW COHESION** - it's doing too many unrelated things! LCOM will be HIGH, indicating poor design.

```java
class UserManager_Bad {
    // User-related fields
    private String username;
    private String email;
    private String password;
    
    // Email-related fields
    private String smtpHost;
    private int smtpPort;
    
    // Report-related fields
    private String reportFormat;
    private String reportPath;
    
    // Payment-related fields
    private String creditCardNumber;
    private String paymentGateway;
    
    // Logging-related fields
    private String logFile;
    private String logLevel;
    
    // Group 1: User management methods (use username, email, password)
    public void createUser() {
        System.out.println("Creating user: " + username);
        System.out.println("Email: " + email);
        System.out.println("Password: " + password);
    }
    
    public void validateUser() {
        if (username == null || email == null) {
            throw new IllegalStateException("Invalid user");
        }
    }
    
    // Group 2: Email methods (use smtpHost, smtpPort)
    public void sendEmail(String message) {
        System.out.println("Sending email via " + smtpHost + ":" + smtpPort);
        System.out.println("Message: " + message);
    }
    
    public void configureEmailServer() {
        System.out.println("Configuring SMTP: " + smtpHost);
    }
    
    // Group 3: Report methods (use reportFormat, reportPath)
    public void generateReport() {
        System.out.println("Generating report in format: " + reportFormat);
        System.out.println("Saving to: " + reportPath);
    }
    
    public void exportReport() {
        System.out.println("Exporting report to: " + reportPath);
    }
    
    // Group 4: Payment methods (use creditCardNumber, paymentGateway)
    public void processPayment(double amount) {
        System.out.println("Processing payment of $" + amount);
        System.out.println("Using gateway: " + paymentGateway);
        System.out.println("Card: " + creditCardNumber);
    }
    
    public void refundPayment(String transactionId) {
        System.out.println("Refunding via: " + paymentGateway);
    }
    
    // Group 5: Logging methods (use logFile, logLevel)
    public void logMessage(String message) {
        System.out.println("Logging to " + logFile + " at level " + logLevel);
    }
    
    public void setLogLevel(String level) {
        this.logLevel = level;
    }
}
```

## LCOM Calculation for UserManager_Bad

**Calculating LCOM (Chidamber & Kemerer Method):**

```
LCOM = P - Q (if P > Q, else 0)
```

Where:
- **P** = number of method pairs that DON'T share any fields
- **Q** = number of method pairs that DO share at least one field

### Fields:
- username, email, password (User group)
- smtpHost, smtpPort (Email group)
- reportFormat, reportPath (Report group)
- creditCardNumber, paymentGateway (Payment group)
- logFile, logLevel (Logging group)

### Methods (10 total):
1. `createUser()` - uses: username, email, password
2. `validateUser()` - uses: username, email
3. `sendEmail()` - uses: smtpHost, smtpPort
4. `configureEmailServer()` - uses: smtpHost
5. `generateReport()` - uses: reportFormat, reportPath
6. `exportReport()` - uses: reportPath
7. `processPayment()` - uses: creditCardNumber, paymentGateway
8. `refundPayment()` - uses: paymentGateway
9. `logMessage()` - uses: logFile, logLevel
10. `setLogLevel()` - uses: logLevel

### Calculation:
- Total method pairs = C(10,2) = 10×9/2 = **45 pairs**

**Method pairs that SHARE fields (Q):**
- (createUser, validateUser): share username, email → 1
- (sendEmail, configureEmailServer): share smtpHost → 1
- (generateReport, exportReport): share reportPath → 1
- (processPayment, refundPayment): share paymentGateway → 1
- (logMessage, setLogLevel): share logLevel → 1
- **Total Q = 5**

**Method pairs that DON'T share fields (P):**
- P = 45 - 5 = **40**

**LCOM = P - Q = 40 - 5 = 35**

⚠️ **HIGH LCOM (35) = LOW COHESION = BAD DESIGN!**

This indicates the class is doing too many unrelated things. The class mixes 5 unrelated responsibilities:
1. User management
2. Email operations
3. Report generation
4. Payment processing
5. Logging

---

## Good Example - High Cohesion (Low LCOM)

Refactored into separate, cohesive classes. Each class has LOW LCOM, indicating HIGH cohesion.

### Class 1: User Management (HIGH COHESION)

```java
class UserManager_Good {
    // All fields are related to User
    private String username;
    private String email;
    private String password;
    private boolean isActive;
    
    public UserManager_Good(String username, String email, String password) {
        this.username = username;
        this.email = email;
        this.password = password;
        this.isActive = false;
    }
    
    // All methods use the same fields - HIGH COHESION!
    public void createUser() {
        System.out.println("Creating user: " + username);
        this.isActive = true;
    }
    
    public void validateUser() {
        if (username == null || email == null || password == null) {
            throw new IllegalStateException("Invalid user data");
        }
    }
    
    public void activateUser() {
        validateUser();
        this.isActive = true;
        System.out.println("User activated: " + username);
    }
    
    public void updateEmail(String newEmail) {
        this.email = newEmail;
        System.out.println("Email updated for user: " + username);
    }
    
    public boolean authenticate(String password) {
        return this.password.equals(password) && this.isActive;
    }
    
    public String getUsername() { return username; }
    public String getEmail() { return email; }
}
```

### LCOM for UserManager_Good:

**Fields:** username, email, password, isActive (4 fields)

**Methods:** createUser, validateUser, activateUser, updateEmail, authenticate (5 methods)

Method pairs = C(5,2) = **10**

**ALL methods share at least one field:**
- createUser uses: username, isActive
- validateUser uses: username, email, password
- activateUser uses: username, email, password, isActive
- updateEmail uses: username, email
- authenticate uses: password, isActive

- Q (pairs sharing fields) = **10** (ALL pairs share fields!)
- P (pairs not sharing) = **0**

**LCOM = P - Q = 0 - 10 = 0** (LCOM can't be negative, so = 0)

✓ **LOW LCOM (0) = HIGH COHESION = EXCELLENT DESIGN!**

### Class 2: Email Service (HIGH COHESION)

```java
class EmailService {
    private String smtpHost;
    private int smtpPort;
    private String username;
    private String password;
    
    public EmailService(String smtpHost, int smtpPort) {
        this.smtpHost = smtpHost;
        this.smtpPort = smtpPort;
    }
    
    // All methods relate to email operations
    public void authenticate(String username, String password) {
        this.username = username;
        this.password = password;
    }
    
    public void sendEmail(String to, String subject, String body) {
        System.out.println("Sending via " + smtpHost + ":" + smtpPort);
        System.out.println("To: " + to);
    }
    
    public void configureServer(String host, int port) {
        this.smtpHost = host;
        this.smtpPort = port;
    }
    
    public boolean testConnection() {
        System.out.println("Testing connection to " + smtpHost);
        return true;
    }
}
```

**LCOM for EmailService:**
- Methods: 4, Pairs: 6
- All methods use smtpHost and/or smtpPort
- **LCOM = 0 (HIGH COHESION)**

### Class 3: Report Generator (HIGH COHESION)

```java
class ReportGenerator {
    private String format;
    private String outputPath;
    private String reportTitle;
    
    public ReportGenerator(String format, String outputPath) {
        this.format = format;
        this.outputPath = outputPath;
    }
    
    // All methods relate to report generation
    public void setTitle(String title) {
        this.reportTitle = title;
    }
    
    public void generateReport() {
        System.out.println("Generating " + format + " report: " + reportTitle);
        System.out.println("Output: " + outputPath);
    }
    
    public void exportReport() {
        System.out.println("Exporting to: " + outputPath + " in " + format);
    }
    
    public void changeFormat(String newFormat) {
        this.format = newFormat;
    }
    
    public void changeOutputPath(String newPath) {
        this.outputPath = newPath;
    }
}
```

**LCOM for ReportGenerator:**
- Methods: 5, Pairs: 10
- All methods use format and/or outputPath
- **LCOM = 0 (HIGH COHESION)**

### Class 4: Payment Processor (HIGH COHESION)

```java
class PaymentProcessor {
    private String gateway;
    private String merchantId;
    private String apiKey;
    
    public PaymentProcessor(String gateway, String merchantId, String apiKey) {
        this.gateway = gateway;
        this.merchantId = merchantId;
        this.apiKey = apiKey;
    }
    
    // All methods relate to payment processing
    public PaymentResult processPayment(String cardNumber, double amount) {
        System.out.println("Processing $" + amount + " via " + gateway);
        return new PaymentResult(true, "TXN-" + System.currentTimeMillis());
    }
    
    public void refund(String transactionId, double amount) {
        System.out.println("Refunding $" + amount + " via " + gateway);
    }
    
    public void configureGateway(String newGateway, String newApiKey) {
        this.gateway = newGateway;
        this.apiKey = newApiKey;
    }
    
    public boolean verifyConnection() {
        System.out.println("Verifying " + gateway + " connection");
        return true;
    }
}
```

**LCOM for PaymentProcessor:**
- Methods: 4, Pairs: 6
- All methods use gateway and/or apiKey
- **LCOM = 0 (HIGH COHESION)**

### Class 5: Application Logger (HIGH COHESION)

```java
class ApplicationLogger {
    private String logFile;
    private String logLevel;
    private boolean enableConsole;
    
    public ApplicationLogger(String logFile, String logLevel) {
        this.logFile = logFile;
        this.logLevel = logLevel;
        this.enableConsole = true;
    }
    
    // All methods relate to logging
    public void log(String message) {
        if ("DEBUG".equals(logLevel) || "INFO".equals(logLevel)) {
            writeToFile(message);
        }
    }
    
    public void error(String message) {
        writeToFile("[ERROR] " + message);
        if (enableConsole) {
            System.err.println(message);
        }
    }
    
    public void setLogLevel(String level) {
        this.logLevel = level;
        log("Log level changed to: " + level);
    }
    
    public void enableConsoleOutput(boolean enable) {
        this.enableConsole = enable;
    }
    
    private void writeToFile(String message) {
        System.out.println("Writing to " + logFile + ": " + message);
    }
}
```

**LCOM for ApplicationLogger:**
- Methods: 4, Pairs: 6
- All methods use logFile and/or logLevel
- **LCOM = 0 (HIGH COHESION)**

---

## Visual LCOM Comparison

### Refactored Classes LCOM Analysis:

**UserManager_Good:**
- Methods: 5, Pairs: 10
- Shared field pairs (Q): 10
- Non-shared pairs (P): 0
- **LCOM = 0 ✓ (HIGH COHESION)**

**EmailService:**
- Methods: 4, Pairs: 6
- **LCOM = 0 ✓ (HIGH COHESION)**

**ReportGenerator:**
- Methods: 5, Pairs: 10
- **LCOM = 0 ✓ (HIGH COHESION)**

**PaymentProcessor:**
- Methods: 4, Pairs: 6
- **LCOM = 0 ✓ (HIGH COHESION)**

**ApplicationLogger:**
- Methods: 4, Pairs: 6
- **LCOM = 0 ✓ (HIGH COHESION)**

---

## Another Example: Medium Cohesion

Example showing **MEDIUM cohesion** (some shared, some not):

```java
class ShoppingCart {
    // Cart-related fields
    private List<Item> items;
    private String userId;
    
    // Discount-related fields
    private double discountPercentage;
    private String couponCode;
    
    // Shipping-related fields
    private String shippingAddress;
    private String shippingMethod;
    
    public ShoppingCart(String userId) {
        this.userId = userId;
        this.items = new ArrayList<>();
    }
    
    // Group 1: Cart operations (use items, userId)
    public void addItem(Item item) {
        items.add(item);
    }
    
    public void removeItem(Item item) {
        items.remove(item);
    }
    
    public double calculateTotal() {
        return items.stream().mapToDouble(Item::getPrice).sum();
    }
    
    // Group 2: Discount operations (use discountPercentage, couponCode, items)
    public void applyCoupon(String coupon) {
        this.couponCode = coupon;
        this.discountPercentage = 10.0;
    }
    
    public double getTotalWithDiscount() {
        double total = calculateTotal();
        return total - (total * discountPercentage / 100);
    }
    
    // Group 3: Shipping operations (use shippingAddress, shippingMethod)
    public void setShipping(String address, String method) {
        this.shippingAddress = address;
        this.shippingMethod = method;
    }
    
    public double calculateShipping() {
        return "EXPRESS".equals(shippingMethod) ? 20.0 : 5.0;
    }
}
```

### LCOM for ShoppingCart:

**Methods:** 7

**Pairs:** C(7,2) = 21

**Some methods share fields, some don't:**
- (addItem, removeItem): share items → Q
- (addItem, calculateTotal): share items → Q
- (removeItem, calculateTotal): share items → Q
- (applyCoupon, getTotalWithDiscount): share discountPercentage → Q
- (setShipping, calculateShipping): share shippingMethod → Q
- But: (addItem, setShipping): NO shared fields → P
- (calculateTotal, calculateShipping): NO shared fields → P
- etc.

**Approximate Q = 8, P = 13**

**LCOM = 13 - 8 = 5 (MEDIUM COHESION)**

This is better than UserManager_Bad (LCOM=35) but worse than refactored classes (LCOM=0). Consider splitting into: Cart, Discount, Shipping classes.

---

## Complete Example with Usage

```java
public class LCOMExample {
    public static void main(String[] args) {
        System.out.println("=== USAGE EXAMPLE ===\n");
        
        // Bad way (one god class)
        UserManager_Bad badManager = new UserManager_Bad();
        // Everything mixed together - hard to understand and maintain
        
        // Good way (separate cohesive classes)
        UserManager_Good userManager = new UserManager_Good(
            "john_doe", 
            "john@example.com", 
            "password123"
        );
        EmailService emailService = new EmailService("smtp.gmail.com", 587);
        ReportGenerator reportGen = new ReportGenerator("PDF", "/reports");
        PaymentProcessor paymentProcessor = new PaymentProcessor(
            "Stripe", 
            "merchant-123", 
            "api-key-xyz"
        );
        ApplicationLogger logger = new ApplicationLogger("/var/log/app.log", "INFO");
        
        // Each class has a clear, single responsibility
        userManager.createUser();
        emailService.sendEmail("john@example.com", "Welcome", "Hello!");
        reportGen.generateReport();
        paymentProcessor.processPayment("4111111111111111", 99.99);
        logger.log("Application started");
    }
}
```

### Supporting Classes:

```java
class Item {
    private String name;
    private double price;
    
    public Item(String name, double price) {
        this.name = name;
        this.price = price;
    }
    
    public double getPrice() { return price; }
}

class PaymentResult {
    private boolean success;
    private String transactionId;
    
    public PaymentResult(boolean success, String transactionId) {
        this.success = success;
        this.transactionId = transactionId;
    }
}
```

---

## LCOM Formula Summary

```
LCOM = P - Q (if P > Q, else 0)

P = Method çiftlerinin sayısı (ortak field kullanmayan)
Q = Method çiftlerinin sayısı (en az 1 ortak field kullanan)
```

---

## Key Takeaways

✓ **Low LCOM (0-5) = High Cohesion = Good Design**

✓ **High LCOM (>10) = Low Cohesion = Refactor Needed**

✓ Methods should share fields

✓ Each class should have ONE clear responsibility

✓ Split large classes into smaller, focused ones
