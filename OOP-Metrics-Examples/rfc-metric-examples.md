# RFC (Response For a Class) Metric

## RFC Definition & Calculation

**RFC (Response For a Class)** is a metric that measures the number of methods that CAN potentially execute when you call ANY method in a class.

### Formula:

```
RFC = (# of methods in the class) + (# of distinct external methods called)
```

**"External methods"** = methods from OTHER classes that are called

---

## Example 1: LOW RFC (Good - Simple)

```java
class SimpleCalculator {
    // Methods in this class: 2
    public int add(int a, int b) {
        return a + b;  // No external method calls
    }
    
    public int subtract(int a, int b) {
        return a - b;  // No external method calls
    }
}
```

### RFC Calculation for SimpleCalculator:

- **Methods in class:** 2 (add, subtract)
- **External methods called:** 0 (no calls to other classes)

**RFC = 2 + 0 = 2** ✅ **(Very Low - Excellent!)**

When you call `add()`, only `add()` executes. When you call `subtract()`, only `subtract()` executes. Very predictable and easy to test.

---

## Example 2: MEDIUM RFC (Moderate Complexity)

```java
class OrderService_Medium {
    private OrderRepository repository;
    private EmailService emailService;
    private Logger logger;
    
    public OrderService_Medium(OrderRepository repository, 
                               EmailService emailService,
                               Logger logger) {
        this.repository = repository;
        this.emailService = emailService;
        this.logger = logger;
    }
    
    // Method 1: Direct and indirect calls
    public void createOrder(Order order) {
        logger.log("Creating order");              // External call #1
        repository.save(order);                    // External call #2
        emailService.sendEmail(                    // External call #3
            order.getCustomerEmail(),              // External call #4 (Order.getCustomerEmail)
            "Order Confirmed"
        );
    }
    
    // Method 2
    public Order getOrder(String id) {
        logger.log("Fetching order");              // External call #1 (already counted)
        return repository.findById(id);            // External call #5
    }
    
    // Method 3
    public void deleteOrder(String id) {
        logger.log("Deleting order");              // External call #1 (already counted)
        repository.delete(id);                     // External call #6
    }
}
```

### RFC Calculation for OrderService_Medium:

**Methods in this class:** 3
- `createOrder()`
- `getOrder()`
- `deleteOrder()`

**DISTINCT external methods called:**
1. `Logger.log()`
2. `OrderRepository.save()`
3. `EmailService.sendEmail()`
4. `Order.getCustomerEmail()`
5. `OrderRepository.findById()`
6. `OrderRepository.delete()`

**RFC = 3 + 6 = 9** ⚠️ **(Medium)**

When you call `createOrder()`, potentially these methods execute:
- `createOrder()` itself
- `Logger.log()`
- `OrderRepository.save()`
- `EmailService.sendEmail()`
- `Order.getCustomerEmail()`

**Total response set for createOrder() = 5 methods**

---

## Example 3: HIGH RFC (Bad - Complex)

```java
class OrderProcessor_High {
    private OrderRepository orderRepository;
    private CustomerRepository customerRepository;
    private ProductRepository productRepository;
    private InventoryService inventoryService;
    private PaymentGateway paymentGateway;
    private ShippingService shippingService;
    private EmailService emailService;
    private SMSService smsService;
    private NotificationService notificationService;
    private AuditLogger auditLogger;
    private MetricsCollector metricsCollector;
    private CacheManager cacheManager;
    private TaxCalculator taxCalculator;
    private DiscountEngine discountEngine;
    
    // Method 1: Creates a cascade of calls
    public OrderResult processOrder(OrderRequest request) {
        // DIRECT CALLS (methods this class calls directly)
        auditLogger.logStart("processOrder", request.getId());        // External #1
        metricsCollector.startTimer("order_processing");              // External #2
        
        // Validate customer
        Customer customer = customerRepository.findById(               // External #3
            request.getCustomerId()                                    // External #4
        );
        
        if (!customer.isActive()) {                                    // External #5
            return OrderResult.failure("Inactive customer");
        }
        
        // Validate products
        List<Product> products = new ArrayList<>();
        for (String productId : request.getProductIds()) {             // External #6
            Product product = productRepository.findById(productId);   // External #7
            
            if (product == null) {
                return OrderResult.failure("Product not found");
            }
            
            // Check inventory (INDIRECT CALLS happen here!)
            boolean available = inventoryService.checkStock(           // External #8
                product.getSku(),                                      // External #9
                request.getQuantity(productId)                         // External #10
            );
            
            if (!available) {
                return OrderResult.failure("Out of stock");
            }
            
            products.add(product);
        }
        
        // Calculate totals
        double subtotal = calculateSubtotal(products, request);        // Internal method
        double discount = discountEngine.calculateDiscount(            // External #11
            customer.getTier(),                                        // External #12
            subtotal
        );
        double tax = taxCalculator.calculateTax(                       // External #13
            subtotal - discount,
            customer.getAddress().getState()                           // External #14, #15
        );
        double total = subtotal - discount + tax;
        
        // Process payment (MORE INDIRECT CALLS!)
        PaymentResult paymentResult = paymentGateway.charge(           // External #16
            customer.getPaymentMethod(),                               // External #17
            total
        );
        
        if (!paymentResult.isSuccess()) {                              // External #18
            return OrderResult.failure("Payment failed");
        }
        
        // Reserve inventory
        for (Product product : products) {
            inventoryService.reserveStock(                             // External #19
                product.getSku(),                                      // External #9 (counted)
                request.getQuantity(product.getId())                   // External #10 (counted)
            );
        }
        
        // Create order
        Order order = new Order(                                       // External #20 (constructor)
            generateOrderId(),                                         // Internal method
            customer.getId(),                                          // External #21
            products,
            total
        );
        
        Order savedOrder = orderRepository.save(order);                // External #22
        
        // Schedule shipping (EVEN MORE INDIRECT CALLS!)
        ShippingLabel label = shippingService.createLabel(             // External #23
            customer.getAddress(),                                     // External #14 (counted)
            calculateWeight(products)                                  // Internal method
        );
        
        shippingService.schedulePickup(                                // External #24
            label.getTrackingNumber()                                  // External #25
        );
        
        // Send notifications (STILL MORE CALLS!)
        emailService.sendOrderConfirmation(                            // External #26
            customer.getEmail(),                                       // External #27
            savedOrder
        );
        
        if (customer.hasPhoneNumber()) {                               // External #28
            smsService.sendSMS(                                        // External #29
                customer.getPhoneNumber(),                             // External #30
                "Order confirmed!"
            );
        }
        
        notificationService.notifyCustomer(                            // External #31
            customer,
            "Order #" + savedOrder.getId()                             // External #32
        );
        
        // Finalize
        cacheManager.invalidate("orders:" + customer.getId());         // External #33
        metricsCollector.stopTimer("order_processing");                // External #34
        metricsCollector.recordOrderValue(total);                      // External #35
        auditLogger.logEnd("processOrder", savedOrder.getId());        // External #36
        
        return OrderResult.success(savedOrder);
    }
    
    // Internal helper methods
    private double calculateSubtotal(List<Product> products, OrderRequest request) {
        return products.stream()
            .mapToDouble(p -> p.getPrice() * request.getQuantity(p.getId()))
            .sum();
    }
    
    private String generateOrderId() {
        return "ORDER-" + System.currentTimeMillis();
    }
    
    private double calculateWeight(List<Product> products) {
        return products.stream().mapToDouble(Product::getWeight).sum();
    }
}
```

### RFC Calculation for OrderProcessor_High:

**Methods in this class:** 4
- `processOrder()`
- `calculateSubtotal()` (private helper)
- `generateOrderId()` (private helper)
- `calculateWeight()` (private helper)

**DISTINCT external methods called:** 36+

1. `AuditLogger.logStart()`
2. `MetricsCollector.startTimer()`
3. `CustomerRepository.findById()`
4. `OrderRequest.getCustomerId()`
5. `Customer.isActive()`
6. `OrderRequest.getProductIds()`
7. `ProductRepository.findById()`
8. `InventoryService.checkStock()`
9. `Product.getSku()`
10. `OrderRequest.getQuantity()`
11. `DiscountEngine.calculateDiscount()`
12. `Customer.getTier()`
13. `TaxCalculator.calculateTax()`
14. `Customer.getAddress()`
15. `Address.getState()`
16. `PaymentGateway.charge()`
17. `Customer.getPaymentMethod()`
18. `PaymentResult.isSuccess()`
19. `InventoryService.reserveStock()`
20. `Order` constructor
21. `Customer.getId()`
22. `OrderRepository.save()`
23. `ShippingService.createLabel()`
24. `ShippingService.schedulePickup()`
25. `ShippingLabel.getTrackingNumber()`
26. `EmailService.sendOrderConfirmation()`
27. `Customer.getEmail()`
28. `Customer.hasPhoneNumber()`
29. `SMSService.sendSMS()`
30. `Customer.getPhoneNumber()`
31. `NotificationService.notifyCustomer()`
32. `Order.getId()`
33. `CacheManager.invalidate()`
34. `MetricsCollector.stopTimer()`
35. `MetricsCollector.recordOrderValue()`
36. `AuditLogger.logEnd()`

**RFC = 4 + 36+ = 40+** 🔥 **(Critical - Way too complex!)**

When you call `processOrder()`, **40+ different methods** could potentially execute! This creates:
- Massive testing burden (need to mock 14+ dependencies)
- Hard to understand control flow
- Brittle code (many points of failure)
- Difficult debugging

---

## Refactored Solution: LOW RFC with Facade Pattern

### Improved Design:

```java
// FACADE - Simple API with LOW RFC
class OrderFacade {
    private OrderWorkflow workflow;
    
    public OrderFacade(OrderWorkflow workflow) {
        this.workflow = workflow;
    }
    
    // Only calls ONE external method!
    public OrderResult processOrder(OrderRequest request) {
        return workflow.execute(request);  // External call #1
    }
}
```

**RFC for OrderFacade:**
- Methods in class: 1 (`processOrder`)
- External methods called: 1 (`workflow.execute`)
- **RFC = 1 + 1 = 2** ✅ **(Excellent!)**

### The Workflow (Hidden Complexity):

```java
// WORKFLOW - Orchestrates the complex process
class OrderWorkflow {
    private CustomerValidator customerValidator;
    private InventoryChecker inventoryChecker;
    private PriceCalculator priceCalculator;
    private PaymentProcessor paymentProcessor;
    private OrderPersistence orderPersistence;
    private ShippingCoordinator shippingCoordinator;
    private NotificationManager notificationManager;
    
    public OrderResult execute(OrderRequest request) {
        // Step 1: Validate
        ValidationResult validation = customerValidator.validate(request);
        if (!validation.isValid()) {
            return OrderResult.failure(validation.getError());
        }
        
        // Step 2: Check inventory
        if (!inventoryChecker.hasStock(request)) {
            return OrderResult.failure("Out of stock");
        }
        
        // Step 3: Calculate price
        double total = priceCalculator.calculateTotal(request);
        
        // Step 4: Process payment
        PaymentResult payment = paymentProcessor.process(request, total);
        if (!payment.isSuccess()) {
            return OrderResult.failure("Payment failed");
        }
        
        // Step 5: Create order
        Order order = orderPersistence.createOrder(request, total);
        
        // Step 6: Arrange shipping
        shippingCoordinator.arrangeShipping(order);
        
        // Step 7: Notify customer
        notificationManager.sendConfirmation(order);
        
        return OrderResult.success(order);
    }
}
```

**RFC for OrderWorkflow:**
- Methods in class: 1 (`execute`)
- External methods called: 7 (one per step)
- **RFC = 1 + 7 = 8** ⚠️ **(Moderate - acceptable for a coordinator)**

---

## RFC Interpretation Guidelines

```
RFC < 10:  ✅ Good - Easy to test and understand
RFC 10-20: ⚠️ Moderate - Consider refactoring
RFC 20-30: ❌ High - Definitely refactor
RFC > 30:  🔥 Critical - Urgent refactoring needed
```

---

## Benefits of Low RFC

✓ **Smaller testing surface** - fewer dependencies to mock

✓ **Easier to understand** - simpler control flow

✓ **Less brittle** - fewer points of failure

✓ **Easier to debug** - shorter call chains

✓ **Better maintainability** - isolated changes

---

## Strategies to Reduce RFC

1. **Use Facade pattern** to hide complexity
2. **Break large classes** into smaller ones
3. **Apply Single Responsibility Principle**
4. **Prefer composition** over deep call chains
5. **Hide internal call graphs** behind simple APIs

---

## Testing Implications

### Easy to Test - Low RFC

```java
@Test
public void testSimpleCalculator() {
    SimpleCalculator calc = new SimpleCalculator();
    
    // Only need to test the calculator itself
    // No mocks needed!
    int result = calc.add(2, 3);
    assert result == 5;
    
    // Simple, fast, reliable
}
```

### Harder to Test - High RFC

```java
@Test
public void testOrderProcessor() {
    // Need to mock 10+ dependencies!
    OrderRepository mockOrderRepo = mock(OrderRepository.class);
    CustomerRepository mockCustomerRepo = mock(CustomerRepository.class);
    ProductRepository mockProductRepo = mock(ProductRepository.class);
    InventoryService mockInventory = mock(InventoryService.class);
    PaymentGateway mockPayment = mock(PaymentGateway.class);
    ShippingService mockShipping = mock(ShippingService.class);
    EmailService mockEmail = mock(EmailService.class);
    SMSService mockSMS = mock(SMSService.class);
    NotificationService mockNotification = mock(NotificationService.class);
    AuditLogger mockAudit = mock(AuditLogger.class);
    // ... 5 more mocks ...
    
    OrderProcessor_High processor = new OrderProcessor_High(
        mockOrderRepo, mockCustomerRepo, mockProductRepo,
        mockInventory, mockPayment, mockShipping,
        mockEmail, mockSMS, mockNotification, mockAudit
        // ... more dependencies ...
    );
    
    // Need to set up expectations for dozens of method calls
    // when(...).thenReturn(...)
    // ... 50+ lines of setup ...
    
    // Complex, slow, brittle
}
```

### Easy to Test - Low RFC (Facade)

```java
@Test
public void testOrderFacade() {
    // Only need to mock the workflow
    OrderWorkflow mockWorkflow = mock(OrderWorkflow.class);
    OrderFacade facade = new OrderFacade(mockWorkflow);
    
    OrderRequest request = new OrderRequest();
    when(mockWorkflow.execute(request))
        .thenReturn(OrderResult.success(new Order()));
    
    OrderResult result = facade.processOrder(request);
    
    assert result.isSuccess();
    verify(mockWorkflow).execute(request);
    
    // Simple, fast, focused
}
```

---

## Key Takeaways

✓ RFC measures the **potential response set** when calling a method

✓ **Low RFC** (< 10) = easy to test, understand, and maintain

✓ **High RFC** (> 30) = complex, brittle, hard to test

✓ Use **Facade pattern** to hide complexity and reduce RFC

✓ Break down **god classes** into smaller, focused components

✓ Each class should have a **clear, single responsibility**
