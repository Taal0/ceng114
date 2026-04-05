# Cyclomatic Complexity (CC) and WMC

## Cyclomatic Complexity Basics

**CC (Cyclomatic Complexity) Formula:**

```
CC = Number of decision points + 1
```

### Decision Points:
- `if`, `else if`
- `while`, `for`, `do-while`
- `case` in switch
- `&&` (and), `||` (or) in conditions
- `catch` blocks
- Ternary operator (`? :`)

**Alternative Formula (Graph Theory):**
```
CC = E - N + 2P
```
Where: E = edges, N = nodes, P = connected components

**Simpler approach:** Count decision keywords + 1

---

## Example 1: CC Calculation Step by Step

```java
class CCExamples {
    
    // CC = 1 (No decisions, straight line)
    public int add(int a, int b) {
        return a + b;
        // Paths: 1 (straight through)
        // CC = 1 ✅ Simple
    }
    
    // CC = 2 (One if = 1 decision)
    public String checkAge(int age) {
        if (age >= 18) {           // +1 decision
            return "Adult";
        }
        return "Minor";
        // Paths: 2
        // Path 1: age >= 18 → return "Adult"
        // Path 2: age < 18 → return "Minor"
        // CC = 1 (base) + 1 (if) = 2 ✅
    }
    
    // CC = 3 (Two branches = 2 decisions)
    public String checkGrade(int score) {
        if (score >= 90) {         // +1 decision
            return "A";
        } else if (score >= 80) {  // +1 decision
            return "B";
        }
        return "F";
        // Paths: 3
        // Path 1: score >= 90 → "A"
        // Path 2: 80 <= score < 90 → "B"
        // Path 3: score < 80 → "F"
        // CC = 1 + 1 + 1 = 3 ✅
    }
    
    // CC = 4 (Nested if = 3 decisions)
    public String checkEligibility(int age, boolean hasLicense) {
        if (age >= 18) {           // +1 decision
            if (hasLicense) {      // +1 decision
                return "Can drive";
            } else {               // +1 decision (else counts)
                return "Need license";
            }
        }
        return "Too young";
        // Paths: 4
        // CC = 1 + 3 = 4 ✅
    }
    
    // CC = 3 (Loop + condition)
    public int countPositive(int[] numbers) {
        int count = 0;
        for (int num : numbers) {  // +1 decision (loop)
            if (num > 0) {         // +1 decision
                count++;
            }
        }
        return count;
        // Paths: Multiple (loop can iterate 0, 1, 2... times)
        // CC = 1 + 1 (for) + 1 (if) = 3 ✅
    }
    
    // CC = 4 (Complex conditions with && and ||)
    public boolean canVote(int age, boolean isCitizen, boolean isRegistered) {
        if (age >= 18 && isCitizen && isRegistered) {  // +3 decisions (2 && operators)
            return true;
        }
        return false;
        // Each && or || adds +1
        // CC = 1 + 1 (if) + 2 (&&) = 4 ✅
    }
    
    // CC = 8 (Switch statement)
    public String getDayName(int day) {
        switch (day) {          // +1 decision (switch)
            case 1:             // +1 decision
                return "Monday";
            case 2:             // +1 decision
                return "Tuesday";
            case 3:             // +1 decision
                return "Wednesday";
            case 4:             // +1 decision
                return "Thursday";
            case 5:             // +1 decision
                return "Friday";
            default:            // +1 decision
                return "Weekend";
        }
        // CC = 1 + 6 (cases) + 1 (default) = 8 ⚠️
    }
}
```

---

## Visual Path Representation

```java
class PathVisualization {
    
    /**
     * Method with CC = 4
     * 
     * Visual representation of independent paths:
     */
    public String classifyNumber(int num) {
        if (num > 0) {              // Decision 1
            if (num % 2 == 0) {     // Decision 2
                return "Positive Even";
            } else {                // Decision 3
                return "Positive Odd";
            }
        }
        return "Non-positive";
    }
}
```

### Independent Paths (CC = 4):

**Path 1:** `num > 0 = false`
- → return "Non-positive"

**Path 2:** `num > 0 = true, num % 2 == 0 = true`
- → return "Positive Even"

**Path 3:** `num > 0 = true, num % 2 == 0 = false`
- → return "Positive Odd"

**Path 4:** (base path through all decisions)

**To fully test: need 4 test cases minimum!**

---

## WMC (Weighted Methods per Class)

**WMC = Sum of CC of all methods in a class**

```
WMC = CC(method1) + CC(method2) + CC(method3) + ...
```

### LOW WMC - Simple Class

```java
class SimpleValidator {
    
    // CC = 2
    public boolean isValidEmail(String email) {
        if (email.contains("@")) {
            return true;
        }
        return false;
    }
    
    // CC = 2
    public boolean isValidPhone(String phone) {
        if (phone.length() == 10) {
            return true;
        }
        return false;
    }
    
    // CC = 1
    public boolean isNotEmpty(String value) {
        return value != null && !value.isEmpty();
    }
}
```

**WMC for SimpleValidator:**

```
WMC = CC(isValidEmail) + CC(isValidPhone) + CC(isNotEmpty)
WMC = 2 + 2 + 1 = 5 ✅ (Low - Good!)
```

Easy to understand, test, and maintain.

---

## HIGH WMC - Complex Class

```java
class OrderValidator_Bad {
    
    // CC = 15 (Many nested conditions)
    public ValidationResult validateOrder(Order order) {
        if (order == null) {                                    // +1
            return ValidationResult.error("Order is null");
        }
        
        if (order.getCustomer() == null) {                      // +1
            return ValidationResult.error("Customer required");
        }
        
        Customer customer = order.getCustomer();
        if (customer.getId() == null || customer.getId().isEmpty()) { // +2 (||)
            return ValidationResult.error("Customer ID required");
        }
        
        if (order.getItems() == null || order.getItems().isEmpty()) { // +2 (||)
            return ValidationResult.error("Order items required");
        }
        
        for (OrderItem item : order.getItems()) {               // +1 (loop)
            if (item.getProduct() == null) {                    // +1
                return ValidationResult.error("Product required");
            }
            
            if (item.getQuantity() <= 0) {                      // +1
                return ValidationResult.error("Invalid quantity");
            }
            
            if (item.getPrice() < 0) {                          // +1
                return ValidationResult.error("Invalid price");
            }
            
            if (item.getProduct().isDiscontinued()) {           // +1
                if (!customer.isAdmin()) {                      // +1
                    return ValidationResult.error("Product discontinued");
                }
            }
        }
        
        if (order.getTotal() <= 0) {                            // +1
            return ValidationResult.error("Invalid total");
        }
        
        if (order.getPaymentMethod() == null) {                 // +1
            return ValidationResult.error("Payment method required");
        }
        
        if (order.getPaymentMethod().isExpired()) {             // +1
            return ValidationResult.error("Payment method expired");
        }
        
        return ValidationResult.success();
        // CC = 1 + 14 = 15 🔥 (Too complex!)
    }
}
```

**WMC for OrderValidator_Bad:**
- Single method: CC = 15
- **WMC = 15** 🔥 (High complexity!)

This requires approximately **15 test cases** to cover all paths!

---

## Refactored Solution: LOW CC per Method

```java
class OrderValidator_Good {
    
    // CC = 2 ✅
    public ValidationResult validateOrder(Order order) {
        if (order == null) {                                    // +1
            return ValidationResult.error("Order is null");
        }
        
        ValidationResult result;
        
        result = validateCustomer(order.getCustomer());
        if (!result.isValid()) return result;                                    // +1
        
        result = validateItems(order.getItems(), order.getCustomer());
        if (!result.isValid()) return result;                                    // +1
        
        result = validateTotal(order.getTotal());
        if (!result.isValid()) return result;                                    // +1
        
        result = validatePaymentMethod(order.getPaymentMethod());
        if (!result.isValid()) return result;                                    // +1
        
        return ValidationResult.success();
        // CC = 1 + 5 = 6 ✅
    }
    
    // CC = 3 ✅
    private ValidationResult validateCustomer(Customer customer) {
        if (customer == null) {                                 // +1
            return ValidationResult.error("Customer required");
        }
        
        if (customer.getId() == null || customer.getId().isEmpty()) { // +2
            return ValidationResult.error("Customer ID required");
        }
        
        return ValidationResult.success();
        // CC = 1 + 2 = 3 ✅
    }
    
    // CC = 6 ✅
    private ValidationResult validateItems(List<OrderItem> items, Customer customer) {
        if (items == null || items.isEmpty()) {                 // +2
            return ValidationResult.error("Order items required");
        }
        
        for (OrderItem item : items) {                          // +1
            if (item.getProduct() == null) {                    // +1
                return ValidationResult.error("Product required");
            }
            
            if (item.getQuantity() <= 0) {                      // +1
                return ValidationResult.error("Invalid quantity");
            }
            
            if (item.getPrice() < 0) {                          // +1
                return ValidationResult.error("Invalid price");
            }
            
            if (item.getProduct().isDiscontinued() && !customer.isAdmin()) { // +2
                return ValidationResult.error("Product discontinued");
            }
        }
        
        return ValidationResult.success();
        // CC = 1 + 7 = 8 ✅
    }
    
    // CC = 2 ✅
    private ValidationResult validateTotal(double total) {
        if (total <= 0) {                                       // +1
            return ValidationResult.error("Invalid total");
        }
        return ValidationResult.success();
        // CC = 1 + 1 = 2 ✅
    }
    
    // CC = 3 ✅
    private ValidationResult validatePaymentMethod(PaymentMethod paymentMethod) {
        if (paymentMethod == null) {                            // +1
            return ValidationResult.error("Payment method required");
        }
        
        if (paymentMethod.isExpired()) {                        // +1
            return ValidationResult.error("Payment method expired");
        }
        
        return ValidationResult.success();
        // CC = 1 + 2 = 3 ✅
    }
}
```

**WMC for OrderValidator_Good:**

```
WMC = 6 + 3 + 8 + 2 + 3 = 22
```

**Comparison:**
- `OrderValidator_Bad`: Single method with CC = 15, WMC = 15
- `OrderValidator_Good`: Five methods, max CC = 8, WMC = 22

**Key insight:** While WMC is slightly higher, each method is simpler and can be tested independently!

---

## CC Interpretation Guidelines

```
CC = 1-5:   ✅ Simple - Easy to test
CC = 6-10:  ⚠️ Moderate - Acceptable
CC = 11-20: ❌ Complex - Consider refactoring
CC > 20:    🔥 Very Complex - Must refactor
```

---

## WMC Interpretation Guidelines

```
WMC = 1-10:  ✅ Simple class
WMC = 11-20: ⚠️ Moderate - Monitor
WMC = 21-40: ❌ Complex - Consider splitting
WMC > 40:    🔥 Very Complex - Split immediately
```

---

## Testing Implications

- **Method with CC = 5** → Need ~5 test cases minimum
- **Method with CC = 15** → Need ~15 test cases minimum
- **Method with CC = 30** → Need ~30 test cases minimum 😱

---

## Refactoring Strategies

1. **Extract complex conditions** into helper methods
2. **Replace nested if-else** with polymorphism
3. **Use strategy pattern** for complex branching
4. **Break large methods** into smaller ones
5. **Use early returns** to reduce nesting
6. **Replace switch** with lookup tables or maps

---

## Practical Example: Reducing CC

### Before Refactoring - High CC

```java
class DiscountCalculator_Bad {
    // CC = 17 🔥 (Too complex!)
    public double calculateDiscount(Customer customer, Order order) {
        if (customer == null || order == null) {                          // +2
            return 0;
        }
        
        double discount = 0;
        
        if ("PREMIUM".equals(customer.getTier())) {                       // +1
            discount = 0.20;
            if (order.getTotal() > 1000) {                                // +1
                discount = 0.30;
            }
        } else if ("GOLD".equals(customer.getTier())) {                   // +1
            discount = 0.15;
            if (order.getTotal() > 500) {                                 // +1
                discount = 0.20;
            }
            if (customer.getOrderCount() > 10) {                          // +1
                discount += 0.05;
            }
        } else if ("SILVER".equals(customer.getTier())) {                 // +1
            discount = 0.10;
            if (order.getTotal() > 200) {                                 // +1
                discount = 0.12;
            }
        }
        
        if (customer.hasLoyaltyPoints()) {                                // +1
            if (customer.getLoyaltyPoints() > 1000) {                     // +1
                discount += 0.05;
            } else if (customer.getLoyaltyPoints() > 500) {               // +1
                discount += 0.03;
            }
        }
        
        if (order.getItems().size() > 5) {                                // +1
            discount += 0.02;
        }
        
        if (customer.isBirthdayMonth()) {                                 // +1
            discount += 0.05;
        }
        
        if (discount > 0.50) {                                            // +1
            discount = 0.50;
        }
        
        return discount;
        // CC = 1 + 16 = 17 🔥
    }
}
```

### After Refactoring - Low CC

```java
class DiscountCalculator_Good {
    // CC = 3 ✅ (Much simpler!)
    public double calculateDiscount(Customer customer, Order order) {
        if (customer == null || order == null) {                          // +2
            return 0;
        }
        
        double discount = getTierDiscount(customer, order)
                        + getLoyaltyDiscount(customer)
                        + getBulkDiscount(order)
                        + getBirthdayDiscount(customer);
        
        return Math.min(discount, 0.50); // Cap at 50%
        // CC = 1 + 2 = 3 ✅
    }
    
    // CC = 7 ✅
    private double getTierDiscount(Customer customer, Order order) {
        String tier = customer.getTier();
        double total = order.getTotal();
        
        if ("PREMIUM".equals(tier)) {                                     // +1
            return total > 1000 ? 0.30 : 0.20;                           // +1 (ternary)
        } else if ("GOLD".equals(tier)) {                                 // +1
            double base = total > 500 ? 0.20 : 0.15;                     // +1
            return customer.getOrderCount() > 10 ? base + 0.05 : base;   // +1
        } else if ("SILVER".equals(tier)) {                               // +1
            return total > 200 ? 0.12 : 0.10;                            // +1
        }
        
        return 0;
        // CC = 1 + 6 = 7 ✅
    }
    
    // CC = 4 ✅
    private double getLoyaltyDiscount(Customer customer) {
        if (!customer.hasLoyaltyPoints()) {                               // +1
            return 0;
        }
        
        int points = customer.getLoyaltyPoints();
        if (points > 1000) {                                              // +1
            return 0.05;
        } else if (points > 500) {                                        // +1
            return 0.03;
        }
        
        return 0;
        // CC = 1 + 3 = 4 ✅
    }
    
    // CC = 2 ✅
    private double getBulkDiscount(Order order) {
        return order.getItems().size() > 5 ? 0.02 : 0;                   // +1
        // CC = 1 + 1 = 2 ✅
    }
    
    // CC = 2 ✅
    private double getBirthdayDiscount(Customer customer) {
        return customer.isBirthdayMonth() ? 0.05 : 0;                    // +1
        // CC = 1 + 1 = 2 ✅
    }
}
```

### Comparison:

**DiscountCalculator_Bad:**
- WMC = 17 🔥
- 1 method with 17 independent paths
- Needs 17+ test cases for full coverage
- Hard to understand and maintain

**DiscountCalculator_Good:**
- WMC = 3 + 7 + 4 + 2 + 2 = 18 ⚠️
- But each method is simple (max CC = 7)
- Each method can be tested independently
- Much easier to understand and maintain

**Key insight:** Total WMC is similar, but complexity is **distributed** across smaller, manageable methods!

---

## Summary Formulas

### Cyclomatic Complexity (per method):
```
CC = # of decisions + 1
```

### Decisions count:
- `if`, `else if`: +1 each
- `while`, `for`, `do-while`: +1 each
- `case`: +1 each
- `&&`, `||`: +1 each
- `catch`: +1 each
- `?:` (ternary): +1

### WMC (per class):
```
WMC = Σ CC(all methods)
```

---

## Key Takeaways

✓ **Low CC** (< 10) = simple, testable, maintainable

✓ **High CC** (> 20) = complex, brittle, hard to test

✓ **Break down complex methods** into smaller helper methods

✓ **Distribute complexity** across multiple simple methods

✓ **Each decision point** requires at least one test case

✓ **WMC** gives overall class complexity assessment
