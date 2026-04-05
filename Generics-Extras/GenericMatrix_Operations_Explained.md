# GenericMatrix: A Deep Dive into Matrix Operations in Java

This document provides a comprehensive explanation of the `GenericMatrix` abstract class, which implements generic matrix addition and multiplication operations using Java generics and the Template Method design pattern.

---

## Table of Contents

1. [Class Overview](#class-overview)
2. [Abstract Methods](#abstract-methods)
3. [Matrix Addition Operation](#matrix-addition-operation)
4. [Matrix Multiplication Operation](#matrix-multiplication-operation)
5. [Print Result Method](#print-result-method)
6. [Complete Execution Example](#complete-execution-example)

---

## Class Overview

The `GenericMatrix<E extends Number>` class is an **abstract class** that provides a template for performing matrix operations on any numeric type. The type parameter `E` is bounded by `Number`, meaning it can work with `Integer`, `Double`, `Long`, `Float`, etc.

```java
public abstract class GenericMatrix<E extends Number> {
    // Abstract methods to be implemented by subclasses
    protected abstract E add(E o1, E o2);
    protected abstract E multiply(E o1, E o2);
    protected abstract E zero();
    
    // Concrete methods that use the abstract operations
    public E[][] addMatrix(E[][] matrix1, E[][] matrix2) { ... }
    public E[][] multiplyMatrix(E[][] matrix1, E[][] matrix2) { ... }
}
```

**Design Pattern Used**: Template Method Pattern
- The abstract class defines the algorithm skeleton (how to add/multiply matrices)
- Subclasses provide the concrete implementations for element-wise operations

---

## Abstract Methods

### 1. `add(E o1, E o2)` - Element Addition
Returns the sum of two matrix elements.

### 2. `multiply(E o1, E o2)` - Element Multiplication  
Returns the product of two matrix elements.

### 3. `zero()` - Zero Element
Returns the additive identity (zero) for the numeric type.

**Example Implementation for Integer:**
```java
public class IntegerMatrix extends GenericMatrix<Integer> {
    protected Integer add(Integer o1, Integer o2) { return o1 + o2; }
    protected Integer multiply(Integer o1, Integer o2) { return o1 * o2; }
    protected Integer zero() { return 0; }
}
```

---

## Matrix Addition Operation

### Method Signature
```java
public E[][] addMatrix(E[][] matrix1, E[][] matrix2)
```

### Step 1: Dimension Validation

```java
if ((matrix1.length != matrix2.length) ||
    (matrix1[0].length != matrix2[0].length)) {
    throw new RuntimeException("The matrices do not have the same size");
}
```

**Visualization of Valid vs Invalid Dimensions:**

```
✓ VALID: Same dimensions (2×3 + 2×3)
┌─────────────┐     ┌─────────────┐
│  2×3 Matrix │  +  │  2×3 Matrix │  ✓ OK
└─────────────┘     └─────────────┘

✗ INVALID: Different dimensions (2×3 + 3×2)
┌─────────────┐     ┌───────────┐
│  2×3 Matrix │  +  │ 3×2 Matrix│  ✗ RuntimeException!
└─────────────┘     └───────────┘
```

### Step 2: Result Matrix Creation

```java
E[][] result = (E[][])new Number[matrix1.length][matrix1[0].length];
```

The result matrix has the **same dimensions** as the input matrices.

### Step 3: Element-wise Addition

```java
for (int i = 0; i < result.length; i++)
    for (int j = 0; j < result[i].length; j++) {
        result[i][j] = add(matrix1[i][j], matrix2[i][j]);
    }
```

### Visual Walkthrough: Adding Two 2×3 Matrices

**Input Matrices:**

```
Matrix A (2×3)              Matrix B (2×3)
┌─────┬─────┬─────┐         ┌─────┬─────┬─────┐
│  1  │  2  │  3  │         │  7  │  8  │  9  │
├─────┼─────┼─────┤         ├─────┼─────┼─────┤
│  4  │  5  │  6  │         │ 10  │ 11  │ 12  │
└─────┴─────┴─────┘         └─────┴─────┴─────┘
```

**Iteration Process:**

```
i=0, j=0: result[0][0] = add(1, 7)  = 8
i=0, j=1: result[0][1] = add(2, 8)  = 10
i=0, j=2: result[0][2] = add(3, 9)  = 12
i=1, j=0: result[1][0] = add(4, 10) = 14
i=1, j=1: result[1][1] = add(5, 11) = 16
i=1, j=2: result[1][2] = add(6, 12) = 18
```

**Visual Step-by-Step:**

```
Step 1: i=0, j=0                    Step 2: i=0, j=1
┌─────┬─────┬─────┐                 ┌─────┬─────┬─────┐
│ [1] │  2  │  3  │    add(1,7)     │  8  │ [2] │  3  │    add(2,8)
├─────┼─────┼─────┤      ↓          ├─────┼─────┼─────┤      ↓
│  4  │  5  │  6  │    = 8          │  4  │  5  │  6  │    = 10
└─────┴─────┴─────┘                 └─────┴─────┴─────┘
       +                                   +
┌─────┬─────┬─────┐                 ┌─────┬─────┬─────┐
│ [7] │  8  │  9  │                 │  7  │ [8] │  9  │
├─────┼─────┼─────┤                 ├─────┼─────┼─────┤
│ 10  │ 11  │ 12  │                 │ 10  │ 11  │ 12  │
└─────┴─────┴─────┘                 └─────┴─────┴─────┘

Result after Step 1:                Result after Step 2:
┌─────┬─────┬─────┐                 ┌─────┬─────┬─────┐
│ [8] │  ?  │  ?  │                 │  8  │[10] │  ?  │
├─────┼─────┼─────┤                 ├─────┼─────┼─────┤
│  ?  │  ?  │  ?  │                 │  ?  │  ?  │  ?  │
└─────┴─────┴─────┘                 └─────┴─────┴─────┘
```

```
... continuing all iterations ...

Final Result Matrix (2×3):
┌─────┬─────┬─────┐
│  8  │ 10  │ 12  │
├─────┼─────┼─────┤
│ 14  │ 16  │ 18  │
└─────┴─────┴─────┘
```

### Addition Formula Summary

```
result[i][j] = matrix1[i][j] + matrix2[i][j]

For each position (i,j):
┌─────────────────────────────────────────────────────┐
│  result[i][j] = add(matrix1[i][j], matrix2[i][j])  │
└─────────────────────────────────────────────────────┘
```

---

## Matrix Multiplication Operation

### Method Signature
```java
public E[][] multiplyMatrix(E[][] matrix1, E[][] matrix2)
```

### Step 1: Compatibility Check

```java
if (matrix1[0].length != matrix2.length) {
    throw new RuntimeException("The matrices do not have compatible size");
}
```

**Matrix Multiplication Compatibility Rule:**
- Matrix A: m × n (m rows, n columns)
- Matrix B: n × p (n rows, p columns)  
- Result:   m × p (m rows, p columns)

**The number of COLUMNS in A must equal the number of ROWS in B**

```
Matrix A (m×n)          Matrix B (n×p)          Result (m×p)
┌─────────────┐         ┌─────────┐             ┌─────────┐
│             │         │         │             │         │
│   m rows    │    ×    │ n rows  │      =      │ m rows  │
│             │         │         │             │         │
│  n columns  │         │p columns│             │p columns│
└─────────────┘         └─────────┘             └─────────┘
      ↑                      ↑
      └──── MUST MATCH ──────┘
```

**Valid Example:**
```
A (2×3)  ×  B (3×4)  =  Result (2×4)  ✓
  ↑            ↑
  3     ==     3     ✓ Compatible!
```

**Invalid Example:**
```
A (2×3)  ×  B (2×4)  =  RuntimeException!  ✗
  ↑            ↑
  3     !=     2     ✗ Not compatible!
```

### Step 2: Result Matrix Creation

```java
E[][] result = (E[][])new Number[matrix1.length][matrix2[0].length];
```

Result dimensions: `matrix1.length` × `matrix2[0].length` (m × p)

### Step 3: Triple Nested Loop for Multiplication

```java
for (int i = 0; i < result.length; i++) {           // For each row of result
    for (int j = 0; j < result[0].length; j++) {    // For each column of result
        result[i][j] = zero();                       // Initialize to 0
        
        for (int k = 0; k < matrix1[0].length; k++) {   // Dot product
            result[i][j] = add(result[i][j],
                multiply(matrix1[i][k], matrix2[k][j]));
        }
    }
}
```

### Visual Walkthrough: Multiplying 2×3 and 3×2 Matrices

**Input Matrices:**

```
Matrix A (2×3)                      Matrix B (3×2)
     col0  col1  col2                    col0  col1
    ┌─────┬─────┬─────┐              ┌─────┬─────┐
row0│  1  │  2  │  3  │          row0│  7  │  8  │
    ├─────┼─────┼─────┤              ├─────┼─────┤
row1│  4  │  5  │  6  │          row1│  9  │ 10  │
    └─────┴─────┴─────┘              ├─────┼─────┤
                                 row2│ 11  │ 12  │
                                     └─────┴─────┘

Result Matrix (2×2)
     col0  col1
    ┌─────┬─────┐
row0│  ?  │  ?  │
    ├─────┼─────┤
row1│  ?  │  ?  │
    └─────┴─────┘
```

### Calculating result[0][0]

**Row 0 of A × Column 0 of B:**

```
Matrix A - Row 0              Matrix B - Column 0
┌─────┬─────┬─────┐           ┌─────┬─────┐
│ [1] │ [2] │ [3] │           │ [7] │  8  │
├─────┼─────┼─────┤           ├─────┼─────┤
│  4  │  5  │  6  │           │ [9] │ 10  │
└─────┴─────┴─────┘           ├─────┼─────┤
                              │[11] │ 12  │
                              └─────┴─────┘

Dot Product Calculation:
┌────────────────────────────────────────────────────────┐
│  result[0][0] = (1×7) + (2×9) + (3×11)                │
│               = 7 + 18 + 33                            │
│               = 58                                     │
└────────────────────────────────────────────────────────┘
```

**Step-by-step k-loop for result[0][0]:**

```
Initialize: result[0][0] = zero() = 0

k=0: result[0][0] = add(0, multiply(A[0][0], B[0][0]))
                  = add(0, multiply(1, 7))
                  = add(0, 7)
                  = 7

k=1: result[0][0] = add(7, multiply(A[0][1], B[1][0]))
                  = add(7, multiply(2, 9))
                  = add(7, 18)
                  = 25

k=2: result[0][0] = add(25, multiply(A[0][2], B[2][0]))
                  = add(25, multiply(3, 11))
                  = add(25, 33)
                  = 58

Final: result[0][0] = 58
```

### Calculating result[0][1]

**Row 0 of A × Column 1 of B:**

```
Matrix A - Row 0              Matrix B - Column 1
┌─────┬─────┬─────┐           ┌─────┬─────┐
│ [1] │ [2] │ [3] │           │  7  │ [8] │
├─────┼─────┼─────┤           ├─────┼─────┤
│  4  │  5  │  6  │           │  9  │[10] │
└─────┴─────┴─────┘           ├─────┼─────┤
                              │ 11  │[12] │
                              └─────┴─────┘

Dot Product Calculation:
┌────────────────────────────────────────────────────────┐
│  result[0][1] = (1×8) + (2×10) + (3×12)               │
│               = 8 + 20 + 36                            │
│               = 64                                     │
└────────────────────────────────────────────────────────┘
```

### Calculating result[1][0]

**Row 1 of A × Column 0 of B:**

```
Matrix A - Row 1              Matrix B - Column 0
┌─────┬─────┬─────┐           ┌─────┬─────┐
│  1  │  2  │  3  │           │ [7] │  8  │
├─────┼─────┼─────┤           ├─────┼─────┤
│ [4] │ [5] │ [6] │           │ [9] │ 10  │
└─────┴─────┴─────┘           ├─────┼─────┤
                              │[11] │ 12  │
                              └─────┴─────┘

Dot Product Calculation:
┌────────────────────────────────────────────────────────┐
│  result[1][0] = (4×7) + (5×9) + (6×11)                │
│               = 28 + 45 + 66                           │
│               = 139                                    │
└────────────────────────────────────────────────────────┘
```

### Calculating result[1][1]

**Row 1 of A × Column 1 of B:**

```
Matrix A - Row 1              Matrix B - Column 1
┌─────┬─────┬─────┐           ┌─────┬─────┐
│  1  │  2  │  3  │           │  7  │ [8] │
├─────┼─────┼─────┤           ├─────┼─────┤
│ [4] │ [5] │ [6] │           │  9  │[10] │
└─────┴─────┴─────┘           ├─────┼─────┤
                              │ 11  │[12] │
                              └─────┴─────┘

Dot Product Calculation:
┌────────────────────────────────────────────────────────┐
│  result[1][1] = (4×8) + (5×10) + (6×12)               │
│               = 32 + 50 + 72                           │
│               = 154                                    │
└────────────────────────────────────────────────────────┘
```

### Complete Multiplication Result

```
     Matrix A           Matrix B          Result
     (2×3)        ×      (3×2)      =     (2×2)

┌─────┬─────┬─────┐   ┌─────┬─────┐   ┌─────┬─────┐
│  1  │  2  │  3  │   │  7  │  8  │   │ 58  │ 64  │
├─────┼─────┼─────┤ × ├─────┼─────┤ = ├─────┼─────┤
│  4  │  5  │  6  │   │  9  │ 10  │   │ 139 │ 154 │
└─────┴─────┴─────┘   ├─────┼─────┤   └─────┴─────┘
                      │ 11  │ 12  │
                      └─────┴─────┘
```

### Multiplication Formula Summary

```
For result[i][j]:

result[i][j] = Σ(k=0 to n-1) matrix1[i][k] × matrix2[k][j]

Where n = matrix1[0].length = matrix2.length

Visual representation:
┌─────────────────────────────────────────────────────────┐
│  result[i][j] = ROW i of Matrix1  •  COLUMN j of Matrix2│
│                      (dot product)                       │
└─────────────────────────────────────────────────────────┘
```

### Index Movement Visualization

```
result[i][j] calculation uses:

Matrix1:                    Matrix2:
    k →                        j (fixed)
  ┌───┬───┬───┬───┐           ┌───┐
i │ × │ × │ × │ × │ ←row i    │ × │ k=0
  └───┴───┴───┴───┘           ├───┤
                              │ × │ k=1
                              ├───┤
                              │ × │ k=2
                              ├───┤
                              │ × │ k=3
                              └───┘
                              col j
                               ↑

As k iterates:
- matrix1[i][k] moves RIGHT along row i
- matrix2[k][j] moves DOWN along column j
```

---

## Print Result Method

### Method Signature
```java
public static void printResult(
    Number[][] m1, Number[][] m2, Number[][] m3, char op)
```

This method prints matrices in a visually appealing format with the operator and result.

### Code Breakdown

```java
for (int i = 0; i < m1.length; i++) {
    // Print row i of matrix 1
    for (int j = 0; j < m1[0].length; j++)
        System.out.print(" " + m1[i][j]);

    // Print operator in the middle row only
    if (i == m1.length / 2)
        System.out.print("  " + op + "  ");
    else
        System.out.print("     ");

    // Print row i of matrix 2
    for (int j = 0; j < m2.length; j++)
        System.out.print(" " + m2[i][j]);

    // Print equals sign in the middle row only
    if (i == m1.length / 2)
        System.out.print("  =  ");
    else
        System.out.print("     ");

    // Print row i of result matrix
    for (int j = 0; j < m3.length; j++)
        System.out.print(m3[i][j] + " ");

    System.out.println();
}
```

### Output Example for Addition

```
For matrices:
m1 = {{1, 2, 3}, {4, 5, 6}}
m2 = {{7, 8, 9}, {10, 11, 12}}
m3 = {{8, 10, 12}, {14, 16, 18}}
op = '+'

Output:
 1 2 3       7 8 9       8 10 12 
 4 5 6  +   10 11 12  =  14 16 18 
```

### Visual Flow of printResult

```
Row 0 (i=0):  m1.length/2 = 1 ≠ 0, so print spaces instead of operator
┌────────────────────────────────────────────────────────┐
│ " 1 2 3"  +  "     "  +  " 7 8 9"  +  "     "  +  ...  │
└────────────────────────────────────────────────────────┘

Row 1 (i=1):  m1.length/2 = 1 == 1, so print operator and equals
┌────────────────────────────────────────────────────────┐
│ " 4 5 6"  +  "  +  "  +  " 10 11 12"  +  "  =  "  + ...│
└────────────────────────────────────────────────────────┘
```

---

## Complete Execution Example

### Setting Up Concrete Classes

```java
// IntegerMatrix.java
public class IntegerMatrix extends GenericMatrix<Integer> {
    @Override
    protected Integer add(Integer o1, Integer o2) {
        return o1 + o2;
    }

    @Override
    protected Integer multiply(Integer o1, Integer o2) {
        return o1 * o2;
    }

    @Override
    protected Integer zero() {
        return 0;
    }
}
```

### Test Program

```java
public class TestIntegerMatrix {
    public static void main(String[] args) {
        // Create matrices
        Integer[][] m1 = {{1, 2, 3}, {4, 5, 6}, {1, 1, 1}};
        Integer[][] m2 = {{1, 1, 1}, {2, 2, 2}, {0, 0, 0}};

        // Create matrix operator
        IntegerMatrix intMatrix = new IntegerMatrix();

        // Perform addition
        Integer[][] addResult = intMatrix.addMatrix(m1, m2);
        System.out.println("Matrix Addition:");
        GenericMatrix.printResult(m1, m2, addResult, '+');

        System.out.println();

        // Perform multiplication
        Integer[][] m3 = {{1, 2}, {3, 4}, {5, 6}};
        Integer[][] mulResult = intMatrix.multiplyMatrix(m1, m3);
        System.out.println("Matrix Multiplication:");
        GenericMatrix.printResult(m1, m3, mulResult, '*');
    }
}
```

### Expected Output

```
Matrix Addition:
 1 2 3       1 1 1       2 3 4 
 4 5 6  +    2 2 2  =    6 7 8 
 1 1 1       0 0 0       1 1 1 

Matrix Multiplication:
 1 2 3       1 2       22 28 
 4 5 6  *    3 4  =    49 64 
 1 1 1       5 6       9 12 
```

### Verification of Multiplication Result

```
m1[0] • m3.col0 = (1×1) + (2×3) + (3×5) = 1 + 6 + 15 = 22  ✓
m1[0] • m3.col1 = (1×2) + (2×4) + (3×6) = 2 + 8 + 18 = 28  ✓
m1[1] • m3.col0 = (4×1) + (5×3) + (6×5) = 4 + 15 + 30 = 49 ✓
m1[1] • m3.col1 = (4×2) + (5×4) + (6×6) = 8 + 20 + 36 = 64 ✓
m1[2] • m3.col0 = (1×1) + (1×3) + (1×5) = 1 + 3 + 5 = 9    ✓
m1[2] • m3.col1 = (1×2) + (1×4) + (1×6) = 2 + 4 + 6 = 12   ✓
```

---

## Summary

### Key Concepts

| Operation | Requirement | Result Dimension | Complexity |
|-----------|-------------|------------------|------------|
| Addition | Same dimensions (m×n) | Same as inputs (m×n) | O(m×n) |
| Multiplication | A columns = B rows | A.rows × B.columns | O(m×n×p) |

### Loop Structure Comparison

```
ADDITION                           MULTIPLICATION
─────────                          ──────────────
for i (rows)                       for i (rows of result)
  for j (cols)                       for j (cols of result)
    result[i][j] = A[i][j] + B[i][j]   result[i][j] = 0
                                       for k (dot product)
                                         result[i][j] += A[i][k] * B[k][j]

2 nested loops                     3 nested loops
```

### Design Benefits

The Template Method pattern used in `GenericMatrix` provides several advantages such as code reusability (one implementation works for all numeric types), type safety (generic constraints ensure only Number subclasses are used), and extensibility (easy to add support for new numeric types like `BigInteger`, `Rational`, etc.).

---

*Document created for educational purposes - CENG114 Computer Programming II*