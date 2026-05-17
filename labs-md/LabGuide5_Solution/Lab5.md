Ankara Yıldırım Beyazıt University
Department of Computer Engineering

CENG114 – Computer Programming II
Lab Guide #5: Polymorphism
TechBazaar Online Marketplace

Instructor: Yusuf Evren AYKAÇ

Week: 7 (Spring 2025–2026)

Assistants: Çağın ÖZKAYA, Hatice UYSAL, Yusuf Ekrem KEÇİLİOĞLU

Lab: 5

Learning Objectives

By the end of this lab, you will be able to:

Implement inheritance hierarchies with a base class and multiple subclasses

•
•  Use method overriding to provide type-specific behavior (polymorphism)
•  Store objects of different subclass types in a single ArrayList<Product>
•  Use the instanceof operator to identify object types at runtime
•  Perform downcasting to access subclass-specific methods after type checking
•  Understand why polymorphic method calls resolve to the actual object’s type, not the

reference type

Scenario: TechBazaar Online Marketplace

Background Information
You have been hired as a junior developer at TechBazaar, a rapidly growing online
marketplace. The platform sells everything from electronics to furniture, digital downloads to
organic food. Your task is to build the inventory management module that can handle all
product types through a unified system.

The key challenge: all products share common attributes (ID, name, price, seller), but each
product type has unique fields and behaviors. For instance, shipping cost calculation varies
wildly — digital products cost nothing to ship, while furniture shipping depends on weight.
Your system must handle these differences elegantly using polymorphism.

UML Class Diagrams

Figure 1: Product Class Hierarchy

Study this diagram carefully. You must implement ALL classes exactly as shown, including every field, constructor, getter,
and method.

Figure 2: Marketplace Association

The Marketplace class aggregates Product objects. Notice the 1-to-many (1..*) relationship. Each method in Marketplace
is described in detail in Section 4.

Implementation Requirements

Part A: Product Base Class

Create the Product class as the foundation of your hierarchy.

Fields

productId (String), name (String), price (double), seller (String) — all
private

Constructor

Takes all four fields as parameters and initializes them.

Getters/Setters

Getters for all fields. A setPrice(double) method for price updates.

calculateShippingC
ost()

Returns price * 0.05 (default: 5% of price). Subclasses will override
this.

toString()

Returns: [productId] name - $price (Seller: seller)

Part B: Six Subclasses (Inheritance + Override)

Create all six subclasses listed below. Each extends Product, adds its own fields, and overrides
both calculateShippingCost() and toString(). Pay careful attention to the shipping cost logic —
it is different for every type.

B.1  Electronics

Extra Fields

brand (String), warrantyYears (int), powerWatts (double)

Shipping Cost

Always returns 15.0 (flat rate — fragile items need special packaging).

Returns true if warrantyYears > 2.

Appends  | Electronics - Brand: X, Warranty: Y yrs to parent’s
toString.

hasExtendedWarrant
y()

toString()

B.2  Clothing

Extra Fields

size (String: S/M/L/XL/XXL), material (String), color (String)

Shipping Cost

Always returns 5.0 (lightweight, flat rate).

isPremiumMaterial(
)

Returns true if material is "Silk", "Cashmere", or "Leather". Use
equalsIgnoreCase() for comparison.

toString()

Appends  | Clothing - Size: X, Material: Y, Color: Z to
parent’s toString.

B.3  Book

Extra Fields

author (String), pageCount (int), genre (String)

Shipping Cost

isLongRead()

toString()

B.4  FoodItem

Extra Fields

If price > 25.0, returns 0.0 (free shipping on expensive books).
Otherwise, returns 3.0.

Returns true if pageCount > 500.

Appends  | Book - Author: X, Pages: Y, Genre: Z to parent’s
toString.

expirationDate (String, format: "YYYY-MM-DD"), isOrganic (boolean),
caloriesPerServing (int)

Shipping Cost

Always returns 12.0 (requires refrigerated transport).

isExpired(String
currentDate)

Returns true if currentDate.compareTo(expirationDate) > 0. Since
dates are in "YYYY-MM-DD" format, lexicographic comparison works
correctly.

isLowCalorie()

Returns true if caloriesPerServing < 150.

toString()

Appends  | Food - Exp: X, Organic: Y, Cal: Z to parent’s toString.

B.5  DigitalProduct

Extra Fields

fileSizeMB (double), fileFormat (String, e.g., "PDF", "MP4", "ZIP")

Shipping Cost

Always returns 0.0 (no physical shipping needed).

isLargeFile()

Returns true if fileSizeMB > 1000.

toString()

Appends  | Digital - Size: X MB, Format: Y to parent’s toString.

B.6  Furniture

Extra Fields

weightKg (double), material (String), requiresAssembly (boolean)

Shipping Cost

Returns weightKg * 2.5 (weight-based pricing, heavy items cost more).

isHeavy()

toString()

Returns true if weightKg > 30.

Appends  | Furniture - Weight: Xkg, Material: Y, Assembly: Z
to parent’s toString.

Part C: The Marketplace Class (Polymorphism + instanceof)

Why instanceof?
Since all products are stored in a single ArrayList<Product>, the compiler only knows
each element is a Product. To access subclass-specific methods (like
hasExtendedWarranty() on Electronics or isExpired() on FoodItem), you must first check
the actual type using instanceof, then downcast to the correct subclass type.

Pattern: if (product instanceof Electronics) { Electronics e = (Electronics)
product; ... }

Fields: name (String) and products (ArrayList<Product>). Initialize the ArrayList in the constructor.

C.1  addProduct(Product p) : void

Adds the given product to the products list. Simple and direct.

C.2  listAllProducts() : void

Iterates through the list and prints each product using toString(). This demonstrates
polymorphism: the JVM calls the correct overridden toString() based on the actual object type,
even though the reference type is Product.

C.3  getProductsByType(String type) : ArrayList<Product>

Returns a new ArrayList containing only products matching the given type name. The type
parameter will be one of: "Electronics", "Clothing", "Book", "FoodItem", "DigitalProduct",
"Furniture".

Implementation: Use a chain of if/else if blocks with instanceof to check each product. For
example, if type.equals("Electronics") and the product instanceof Electronics, add it to the
result list.

C.4  calculateTotalShipping() : double

Sums up calculateShippingCost() for every product in the list. This is pure polymorphism —
you do NOT need instanceof here! The correct overridden method is called automatically. Just call
product.calculateShippingCost() on each item.

C.5  getFreeShippingProducts() : ArrayList<Product>

Returns a new ArrayList of products where calculateShippingCost() == 0.0. Again, no
instanceof needed — polymorphism handles it. Think about which types of products will appear in
this list.

C.6  printDetailedReport() : void

This is the most challenging method. It prints a formatted report grouped by product type. For
each product, it prints general information AND type-specific details by using instanceof and
downcasting.

Required Output Format
=== TechBazaar Detailed Inventory Report ===

--- ELECTRONICS (X items) ---
  [E001] Samsung Galaxy S24 - $899.99
    Brand: Samsung | Warranty: 2 yrs | Extended: No
    Shipping: $15.00

--- CLOTHING (X items) ---
  [C001] Silk Evening Dress - $250.00
    Size: M | Material: Silk | Premium: Yes
    Shipping: $5.00
  ... (same pattern for all 6 types)

Hint: First, count how many items belong to each type (using instanceof). Then, loop through the
list 6 times — once per type — printing only matching items. After the instanceof check, downcast
to access type-specific getters.

C.7  applySeasonalDiscount() : void

Applies different discount rates based on product type using instanceof:

Product Type

Electronics

Clothing

Book

FoodItem

DigitalProduct

Furniture

Discount

10%

20%

5%

30%

15%

10%

Formula: newPrice = price - (price * discountRate). Use setPrice() to update.

C.8  findCheapestByType(String type) : Product

Returns the cheapest product of the given type. First, use getProductsByType() to get the filtered
list. Then iterate to find the minimum price. If the list is empty, return null.

C.9  getSpecialProducts() : ArrayList<Product>

Returns products that are “special” based on type-specific criteria. A product is special if:

•  Electronics → hasExtendedWarranty() returns true
•  Clothing → isPremiumMaterial() returns true
•  Book → isLongRead() returns true
•  FoodItem → getIsOrganic() returns true AND isLowCalorie() returns true
•  DigitalProduct → isLargeFile() returns true
•  Furniture → isHeavy() returns true AND getRequiresAssembly() returns false

(pre-assembled heavy items are rare)

This method requires instanceof + downcasting for EVERY type. It is the ultimate test of your
understanding.

Part D: Test Program (Main Class)

Create a Main class with a main method. Use the exact data below so your output matches the
expected example run.

Test Data

#

1

2

3

4

5

6

7

8

9

10

11

12

Type

Electronics

Electronics

Clothing

Clothing

Book

Book

FoodItem

FoodItem

Digital

Digital

Furniture

Furniture

ID

E001

E002

C001

C002

B001

B002

F001

F002

D001

D002

U001

U002

Price

Details

$899.99

Samsung Galaxy S24, "Samsung", 2 yrs, 15W

$1299.99  MacBook Air M3, "Apple", 3 yrs, 30W

$250.00

Silk Evening Dress, size M, "Silk", "Red"

$45.99

Cotton T-Shirt, size L, "Cotton", "Blue"

$45.00

Clean Code, R.C. Martin, 464 pg, "Technology"

$18.99

The Hobbit, J.R.R. Tolkien, 310 pg, "Fantasy"

$12.50

Organic Quinoa, exp "2026-08-15", organic, 120 cal

$8.99

Chocolate Bar, exp "2026-01-10", not organic, 250
cal

$9.99

Java Masterclass, 1500.0 MB, "MP4"

$29.99

Photo Editor Pro, 850.0 MB, "ZIP"

$599.99

Oak Dining Table, 45.0 kg, "Oak", assembly: false

$249.99

Bookshelf, 22.0 kg, "Pine", assembly: true

Required Main Method Steps

Your main method must execute these steps in order:

1.  Create a Marketplace named "TechBazaar"
2.  Add all 12 products from the table above
3.  Call listAllProducts() — prints all 12 products via polymorphic toString()

4.  Call getProductsByType("Electronics") and print the size of the result
5.  Call calculateTotalShipping() and print the total
6.  Call getFreeShippingProducts() and print each product
7.  Call printDetailedReport()
8.  Call applySeasonalDiscount() then print all products again to see updated prices
9.  Call findCheapestByType("Book") and print the result
10. Call getSpecialProducts() and print each one

Example Run:
========================================
 TechBazaar - All Products
========================================
[E001] Samsung Galaxy S24 - $899.99 (Seller: TechWorld) | Electronics - Brand: Samsung,
Warranty: 2 yrs
[E002] MacBook Air M3 - $1299.99 (Seller: TechWorld) | Electronics - Brand: Apple, Warranty: 3
yrs
[C001] Silk Evening Dress - $250.0 (Seller: FashionHub) | Clothing - Size: M, Material: Silk,
Color: Red
[C002] Cotton T-Shirt - $45.99 (Seller: FashionHub) | Clothing - Size: L, Material: Cotton,
Color: Blue
[B001] Clean Code - $45.0 (Seller: BookNest) | Book - Author: Robert C. Martin, Pages: 464,
Genre: Technology
[B002] The Hobbit - $18.99 (Seller: BookNest) | Book - Author: J.R.R. Tolkien, Pages: 310,
Genre: Fantasy
[F001] Organic Quinoa - $12.5 (Seller: GreenMarket) | Food - Exp: 2026-08-15, Organic: true,
Cal: 120
[F002] Chocolate Bar - $8.99 (Seller: GreenMarket) | Food - Exp: 2026-01-10, Organic: false,
Cal: 250
[D001] Java Masterclass - $9.99 (Seller: EduStore) | Digital - Size: 1500.0 MB, Format: MP4
[D002] Photo Editor Pro - $29.99 (Seller: EduStore) | Digital - Size: 850.0 MB, Format: ZIP
[U001] Oak Dining Table - $599.99 (Seller: HomeDecor) | Furniture - Weight: 45.0kg, Material:
Oak, Assembly: false
[U002] Bookshelf - $249.99 (Seller: HomeDecor) | Furniture - Weight: 22.0kg, Material: Pine,
Assembly: true

========================================
 Electronics count: 2
========================================

========================================
 Total Shipping Cost: $234.50
========================================

========================================
 Free Shipping Products:
========================================
[B001] Clean Code - $45.0 (Seller: BookNest) | Book - Author: Robert C. Martin, Pages: 464,
Genre: Technology
[D001] Java Masterclass - $9.99 (Seller: EduStore) | Digital - Size: 1500.0 MB, Format: MP4
[D002] Photo Editor Pro - $29.99 (Seller: EduStore) | Digital - Size: 850.0 MB, Format: ZIP

=== TechBazaar Detailed Inventory Report ===

--- ELECTRONICS (2 items) ---
  [E001] Samsung Galaxy S24 - $899.99
    Brand: Samsung | Warranty: 2 yrs | Extended: No
    Shipping: $15.00
  [E002] MacBook Air M3 - $1299.99
    Brand: Apple | Warranty: 3 yrs | Extended: Yes
    Shipping: $15.00

--- CLOTHING (2 items) ---

  [C001] Silk Evening Dress - $250.00
    Size: M | Material: Silk | Premium: Yes
    Shipping: $5.00
  [C002] Cotton T-Shirt - $45.99
    Size: L | Material: Cotton | Premium: No
    Shipping: $5.00

--- BOOKS (2 items) ---
  [B001] Clean Code - $45.00
    Author: Robert C. Martin | Pages: 464 | Long Read: No
    Shipping: $0.00
  [B002] The Hobbit - $18.99
    Author: J.R.R. Tolkien | Pages: 310 | Long Read: No
    Shipping: $3.00

--- FOOD ITEMS (2 items) ---
  [F001] Organic Quinoa - $12.50
    Exp: 2026-08-15 | Organic: Yes | Low Calorie: Yes
    Expired (as of 2026-03-22): No
    Shipping: $12.00
  [F002] Chocolate Bar - $8.99
    Exp: 2026-01-10 | Organic: No | Low Calorie: No
    Expired (as of 2026-03-22): Yes
    Shipping: $12.00

--- DIGITAL PRODUCTS (2 items) ---
  [D001] Java Masterclass - $9.99
    Size: 1500.0 MB | Format: MP4 | Large File: Yes
    Shipping: $0.00
  [D002] Photo Editor Pro - $29.99
    Size: 850.0 MB | Format: ZIP | Large File: No
    Shipping: $0.00

--- FURNITURE (2 items) ---
  [U001] Oak Dining Table - $599.99
    Weight: 45.0 kg | Material: Oak | Assembly: No | Heavy: Yes
    Shipping: $112.50
  [U002] Bookshelf - $249.99
    Weight: 22.0 kg | Material: Pine | Assembly: Yes | Heavy: No
    Shipping: $55.00

========================================
 Seasonal Discounts Applied!
========================================
[E001] Samsung Galaxy S24 - $809.99 (was $899.99)
[E002] MacBook Air M3 - $1169.99 (was $1299.99)
[C001] Silk Evening Dress - $200.0 (was $250.0)
[C002] Cotton T-Shirt - $36.79 (was $45.99)
[B001] Clean Code - $42.75 (was $45.0)
[B002] The Hobbit - $18.04 (was $18.99)
[F001] Organic Quinoa - $8.75 (was $12.5)
[F002] Chocolate Bar - $6.29 (was $8.99)
[D001] Java Masterclass - $8.49 (was $9.99)
[D002] Photo Editor Pro - $25.49 (was $29.99)
[U001] Oak Dining Table - $539.99 (was $599.99)
[U002] Bookshelf - $224.99 (was $249.99)

========================================
 Cheapest Book: [B002] The Hobbit - $18.04
========================================

========================================
 Special Products:
========================================
[E002] MacBook Air M3 (Extended Warranty)
[C001] Silk Evening Dress (Premium Material)
[F001] Organic Quinoa (Organic + Low Calorie)
[D001] Java Masterclass (Large File)
[U001] Oak Dining Table (Heavy + Pre-assembled)

Hints and Important Notes

Hint 1: Calling super.toString()
When overriding toString() in subclasses, call the parent’s version first:

@Override
public String toString() {
    return super.toString() + " | Electronics - Brand: "
           + brand + ", Warranty: " + warrantyYears + " yrs";
}

Hint 2: The instanceof + Downcast Pattern
This is the core pattern you will use repeatedly:

for (Product p : products) {
    if (p instanceof Electronics) {
        Electronics e = (Electronics) p; // downcast
        System.out.println(e.getBrand());
        System.out.println(e.hasExtendedWarranty());
    } else if (p instanceof Clothing) {
        Clothing c = (Clothing) p; // downcast
        System.out.println(c.isPremiumMaterial());
    }
    // ... other types
}

Hint 3: Polymorphism WITHOUT instanceof
Methods like calculateTotalShipping() and getFreeShippingProducts() do NOT need
instanceof. Since calculateShippingCost() is defined in Product and overridden in each
subclass, the JVM automatically calls the correct version. This is the power of
polymorphism!

// This calls the correct override automatically!
double total = 0;
for (Product p : products) {
    total += p.calculateShippingCost(); // polymorphic call
}

Hint 4: String.format for Clean Output
Use String.format() for formatted output:

String.format("$%.2f", price)       // $899.99
String.format("%-30s $%8.2f", name, price)  // aligned columns

Hint 5: Seller Names for Test Data
Use these seller names to match the expected output: Electronics → "TechWorld", Clothing
→ "FashionHub", Book → "BookNest", FoodItem → "GreenMarket", DigitalProduct →
"EduStore", Furniture → "HomeDecor".

Common Mistakes to Avoid

•  Forgetting @Override annotation — always use it, the compiler will catch typos
•  Not calling super() in constructors — subclass constructors must call

super(productId, name, price, seller) first
•  Using == instead of .equals() for String comparison
•  Casting without instanceof check — will cause ClassCastException at runtime
•  Checking order matters: always check the most specific type first if classes are in a

hierarchy

