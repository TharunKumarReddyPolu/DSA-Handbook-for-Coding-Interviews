# Mathematical Algorithms and Concepts ➗

## Introduction
Mathematical algorithms form the foundation of many computer science problems. Understanding these concepts is crucial for solving problems efficiently and handling numerical computations correctly.

## Prerequisites & Related Topics

### Prerequisites
- Basic arithmetic operations
- Understanding of number systems (decimal, binary)
- Basic algebra concepts

### Related Topics
- **Builds on**: Fundamental mathematics
- **Used in**: [Dynamic Programming](dynamic-programming.md) (combinatorics, recurrences), [Bit Manipulation](bit-manipulation.md) (binary operations)
- **Supports**: [Hashing](hashing.md) (modular arithmetic), [Graph](graph.md) (shortest paths)
- **See also**: [Recursion](recursion.md) (mathematical induction), [Arrays](arrays.md) (number theory problems)

## Pattern Recognition Guide

### 🎯 When to Use Mathematical Approaches
**Keywords in problem**: "prime", "factorial", "GCD", "LCM", "modulo", "combinations", "power"

**Use Math techniques when you see**:
- Number theory problems (primes, divisors, GCD)
- Combinatorics (permutations, combinations)
- Modular arithmetic (large numbers, cryptography)
- Geometric calculations
- Probability or expected value

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Prime Numbers | "prime", "sieve", "factorization" | Count Primes, Prime Factorization |
| GCD/LCM | "greatest common", "least common", "simplify fraction" | GCD of Strings, Fraction to Decimal |
| Modular Arithmetic | "mod 10^9+7", "large numbers", "overflow" | Pow(x,n), Super Pow |
| Combinatorics | "number of ways", "combinations", "arrangements" | Unique Paths, Pascal's Triangle |
| Geometry | "distance", "area", "points on line", "rectangle" | Max Points on Line, Rectangle Area |
| Sequences | "fibonacci", "arithmetic/geometric", "pattern" | Fibonacci Number, Nth Digit |

### ❌ When NOT to Use
- Problem can be solved with simpler data structures
- No clear mathematical pattern or formula
- Input involves non-numeric relationships → consider [Graph](graph.md) or [String](strings.md)

### 💡 Common Math Optimizations
- **Fast exponentiation**: O(log n) instead of O(n)
- **Sieve of Eratosthenes**: Find all primes up to n efficiently
- **Euclidean algorithm**: GCD in O(log min(a,b))
- **Modular inverse**: For division under modulo

## Core Concepts

### Important Terminologies
- **GCD**: Greatest Common Divisor
- **LCM**: Least Common Multiple
- **Prime Number**: Number divisible only by 1 and itself
- **Modulo**: Remainder after division
- **Factorial**: Product of all positive integers ≤ n
- **Fibonacci**: Sequence where each number is sum of previous two
- **Permutation**: Arrangement of items
- **Combination**: Selection of items
- **Exponentiation**: Power operation
- **Divisor**: Number that divides another number exactly

### Mathematical Properties
1. **Number Theory**:
   - Divisibility rules
   - Prime factorization
   - Modular arithmetic
2. **Combinatorics**:
   - Permutations
   - Combinations
   - Pascal's triangle
3. **Series and Sequences**:
   - Arithmetic
   - Geometric
   - Fibonacci

### Time Complexity Analysis
| Operation               | Time Complexity    | Space Complexity |
|------------------------|-------------------|------------------|
| GCD (Euclidean)        | O(log min(a,b))   | O(1)            |
| Prime Check            | O(√n)             | O(1)            |
| Sieve of Eratosthenes | O(n log log n)    | O(n)            |
| Fast Exponentiation   | O(log n)          | O(1)            |
| Prime Factorization   | O(√n)             | O(log n)        |
| Factorial             | O(n)              | O(1)            |
| Fibonacci (Matrix)    | O(log n)          | O(1)            |

## Implementation Patterns

### 1. GCD and LCM

**Pseudocode:**
```
GCD (Euclidean Algorithm):
1. While b != 0:
   a. temp = b
   b. b = a mod b
   c. a = temp
2. Return a

LCM: return |a × b| / gcd(a, b)
```

```python
def gcd(a: int, b: int) -> int:
    while b:
        a, b = b, a % b
    return a

def lcm(a: int, b: int) -> int:
    return abs(a * b) // gcd(a, b)

def extended_gcd(a: int, b: int) -> tuple:
    """Returns (gcd, x, y) where ax + by = gcd"""
    if a == 0:
        return b, 0, 1
    
    gcd, x1, y1 = extended_gcd(b % a, a)
    x = y1 - (b // a) * x1
    y = x1
    
    return gcd, x, y
```

```java
public int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
public int lcm(int a, int b) {
    return Math.abs(a * b) / gcd(a, b);
}
```

```cpp
int gcd(int a, int b) {
    while (b) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
int lcm(int a, int b) {
    return abs(a * b) / gcd(a, b);
}
```

### 2. Prime Numbers

**Pseudocode:**
```
Is Prime:
1. If n < 2: return false
2. For i from 2 to sqrt(n):
   If n mod i == 0: return false
3. Return true

Sieve of Eratosthenes (all primes up to n):
1. Create boolean array sieve[0..n] = true
2. Set sieve[0] = sieve[1] = false
3. For i from 2 to sqrt(n):
   If sieve[i]: mark all multiples i², i²+i, i²+2i... as false
4. Return indices where sieve[i] is true
```

```python
def is_prime(n: int) -> bool:
    if n < 2:
        return False
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0:
            return False
    return True

def sieve_of_eratosthenes(n: int) -> List[int]:
    """Generate all primes up to n"""
    sieve = [True] * (n + 1)
    sieve[0] = sieve[1] = False
    
    for i in range(2, int(n ** 0.5) + 1):
        if sieve[i]:
            for j in range(i * i, n + 1, i):
                sieve[j] = False
    
    return [i for i in range(n + 1) if sieve[i]]
```

```java
public boolean isPrime(int n) {
    if (n < 2) return false;
    for (int i = 2; i <= Math.sqrt(n); i++)
        if (n % i == 0) return false;
    return true;
}
public List<Integer> sieveOfEratosthenes(int n) {
    boolean[] sieve = new boolean[n + 1];
    Arrays.fill(sieve, true);
    sieve[0] = sieve[1] = false;
    for (int i = 2; i * i <= n; i++)
        if (sieve[i])
            for (int j = i * i; j <= n; j += i) sieve[j] = false;
    List<Integer> primes = new ArrayList<>();
    for (int i = 2; i <= n; i++) if (sieve[i]) primes.add(i);
    return primes;
}
```

```cpp
bool isPrime(int n) {
    if (n < 2) return false;
    for (int i = 2; i <= sqrt(n); i++)
        if (n % i == 0) return false;
    return true;
}
vector<int> sieveOfEratosthenes(int n) {
    vector<bool> sieve(n + 1, true);
    sieve[0] = sieve[1] = false;
    for (int i = 2; i * i <= n; i++)
        if (sieve[i])
            for (int j = i * i; j <= n; j += i) sieve[j] = false;
    vector<int> primes;
    for (int i = 2; i <= n; i++) if (sieve[i]) primes.push_back(i);
    return primes;
}
```

**Pseudocode (Prime Factorization):**
```
1. Initialize empty factors list
2. Start with divisor d = 2
3. While n > 1:
   a. While n mod d == 0: add d to factors, n = n / d
   b. d++
   c. If d² > n and n > 1: add n to factors, break
4. Return factors
```

```python
def prime_factors(n: int) -> List[int]:
    factors = []
    d = 2
    while n > 1:
        while n % d == 0:
            factors.append(d)
            n //= d
        d += 1
        if d * d > n:
            if n > 1:
                factors.append(n)
            break
    return factors
```

```java
public List<Integer> primeFactors(int n) {
    List<Integer> factors = new ArrayList<>();
    for (int d = 2; d * d <= n; d++) {
        while (n % d == 0) { factors.add(d); n /= d; }
    }
    if (n > 1) factors.add(n);
    return factors;
}
```

```cpp
vector<int> primeFactors(int n) {
    vector<int> factors;
    for (int d = 2; d * d <= n; d++) {
        while (n % d == 0) { factors.push_back(d); n /= d; }
    }
    if (n > 1) factors.push_back(n);
    return factors;
}
```

### 3. Fast Exponentiation

**Pseudocode:**
```
Power(base, exp, mod):
1. result = 1, base = base mod modulus
2. While exp > 0:
   a. If exp is odd: result = (result × base) mod modulus
   b. base = (base × base) mod modulus
   c. exp = exp / 2 (integer division)
3. Return result
(Uses: x^n = (x^(n/2))² if even, x × x^(n-1) if odd)
```

```python
def power_mod(base: int, exponent: int, modulus: int) -> int:
    """Calculate (base ^ exponent) % modulus efficiently"""
    if modulus == 1:
        return 0
    
    result = 1
    base = base % modulus
    
    while exponent > 0:
        if exponent & 1:
            result = (result * base) % modulus
        base = (base * base) % modulus
        exponent >>= 1
    
    return result
```

```java
public long powerMod(long base, long exp, long mod) {
    long result = 1;
    base %= mod;
    while (exp > 0) {
        if ((exp & 1) == 1) result = (result * base) % mod;
        base = (base * base) % mod;
        exp >>= 1;
    }
    return result;
}
```

```cpp
long long powerMod(long long base, long long exp, long long mod) {
    long long result = 1;
    base %= mod;
    while (exp > 0) {
        if (exp & 1) result = (result * base) % mod;
        base = (base * base) % mod;
        exp >>= 1;
    }
    return result;
}
```

**Pseudocode (Matrix Exponentiation):**
```
1. If n == 0: return identity matrix
2. If n == 1: return matrix
3. half = matrix_power(matrix, n/2)
4. If n is even: return half × half
5. Else: return matrix × half × half
```

```python
def matrix_power(matrix: List[List[int]], n: int) -> List[List[int]]:
    """Calculate matrix ^ n efficiently"""
    def matrix_multiply(A, B):
        n = len(A)
        result = [[0] * n for _ in range(n)]
        for i in range(n):
            for j in range(n):
                for k in range(n):
                    result[i][j] += A[i][k] * B[k][j]
        return result
    
    if n == 0:
        return [[1 if i == j else 0 for j in range(len(matrix))] 
                for i in range(len(matrix))]
    
    if n == 1:
        return matrix
    
    half = matrix_power(matrix, n // 2)
    if n % 2 == 0:
        return matrix_multiply(half, half)
    else:
        return matrix_multiply(matrix_multiply(half, half), matrix)
```

```java
public long[][] matrixPower(long[][] matrix, int n, long mod) {
    int size = matrix.length;
    long[][] result = new long[size][size];
    for (int i = 0; i < size; i++) result[i][i] = 1; // Identity matrix
    while (n > 0) {
        if ((n & 1) == 1) result = matrixMultiply(result, matrix, mod);
        matrix = matrixMultiply(matrix, matrix, mod);
        n >>= 1;
    }
    return result;
}
private long[][] matrixMultiply(long[][] A, long[][] B, long mod) {
    int n = A.length;
    long[][] result = new long[n][n];
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            for (int k = 0; k < n; k++)
                result[i][j] = (result[i][j] + A[i][k] * B[k][j]) % mod;
    return result;
}
```

```cpp
vector<vector<long long>> matrixPower(vector<vector<long long>> matrix, int n, long long mod) {
    int size = matrix.size();
    vector<vector<long long>> result(size, vector<long long>(size, 0));
    for (int i = 0; i < size; i++) result[i][i] = 1; // Identity
    while (n > 0) {
        if (n & 1) result = matrixMultiply(result, matrix, mod);
        matrix = matrixMultiply(matrix, matrix, mod);
        n >>= 1;
    }
    return result;
}
vector<vector<long long>> matrixMultiply(vector<vector<long long>>& A, vector<vector<long long>>& B, long long mod) {
    int n = A.size();
    vector<vector<long long>> result(n, vector<long long>(n, 0));
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            for (int k = 0; k < n; k++)
                result[i][j] = (result[i][j] + A[i][k] * B[k][j]) % mod;
    return result;
}
```

### 4. Combinatorics

**Pseudocode:**
```
Factorial(n): return 1 × 2 × 3 × ... × n

Permutations P(n,r) = n! / (n-r)!

Combinations C(n,r):
1. r = min(r, n-r)  // optimization
2. numerator = n × (n-1) × ... × (n-r+1)
3. denominator = r!
4. Return numerator / denominator

Pascal's Triangle row i: C(i,0), C(i,1), ..., C(i,i)
```

```python
def factorial(n: int) -> int:
    result = 1
    for i in range(2, n + 1):
        result *= i
    return result

def permutations(n: int, r: int) -> int:
    return factorial(n) // factorial(n - r)

def combinations(n: int, r: int) -> int:
    r = min(r, n - r)  # Optimization
    num = den = 1
    for i in range(r):
        num *= (n - i)
        den *= (i + 1)
    return num // den

def pascal_triangle(n: int) -> List[List[int]]:
    triangle = [[1]]
    for i in range(1, n):
        row = [1]
        for j in range(1, i):
            row.append(triangle[i-1][j-1] + triangle[i-1][j])
        row.append(1)
        triangle.append(row)
    return triangle
```

```java
public long factorial(int n) {
    long result = 1;
    for (int i = 2; i <= n; i++) result *= i;
    return result;
}
public long permutations(int n, int r) { return factorial(n) / factorial(n - r); }
public long combinations(int n, int r) {
    r = Math.min(r, n - r);
    long num = 1, den = 1;
    for (int i = 0; i < r; i++) { num *= (n - i); den *= (i + 1); }
    return num / den;
}
public List<List<Integer>> pascalTriangle(int n) {
    List<List<Integer>> triangle = new ArrayList<>();
    for (int i = 0; i < n; i++) {
        List<Integer> row = new ArrayList<>();
        for (int j = 0; j <= i; j++)
            row.add(j == 0 || j == i ? 1 : triangle.get(i-1).get(j-1) + triangle.get(i-1).get(j));
        triangle.add(row);
    }
    return triangle;
}
```

```cpp
long long factorial(int n) {
    long long result = 1;
    for (int i = 2; i <= n; i++) result *= i;
    return result;
}
long long permutations(int n, int r) { return factorial(n) / factorial(n - r); }
long long combinations(int n, int r) {
    r = min(r, n - r);
    long long num = 1, den = 1;
    for (int i = 0; i < r; i++) { num *= (n - i); den *= (i + 1); }
    return num / den;
}
vector<vector<int>> pascalTriangle(int n) {
    vector<vector<int>> triangle(n);
    for (int i = 0; i < n; i++) {
        triangle[i].resize(i + 1);
        triangle[i][0] = triangle[i][i] = 1;
        for (int j = 1; j < i; j++)
            triangle[i][j] = triangle[i-1][j-1] + triangle[i-1][j];
    }
    return triangle;
}
```

### 5. Number Theory Utilities

**Pseudocode:**
```
Euler's Totient φ(n) - count of numbers coprime to n:
1. result = n
2. For each prime factor p of n:
   result = result - result/p
3. Return result

Modular Inverse (a^(-1) mod m):
Use Extended Euclidean: find x where ax + my = gcd(a,m)
If gcd = 1, x mod m is the inverse
```

```python
def euler_totient(n: int) -> int:
    """Count numbers coprime to n"""
    result = n
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0:
            while n % i == 0:
                n //= i
            result -= result // i
    if n > 1:
        result -= result // n
    return result

def modular_inverse(a: int, m: int) -> int:
    """Find multiplicative modular inverse of a under modulo m"""
    def extended_gcd(a, b):
        if a == 0:
            return b, 0, 1
        gcd, x1, y1 = extended_gcd(b % a, a)
        x = y1 - (b // a) * x1
        y = x1
        return gcd, x, y
    
    gcd, x, _ = extended_gcd(a, m)
    if gcd != 1:
        raise Exception('Modular inverse does not exist')
    return (x % m + m) % m
```

```java
public int eulerTotient(int n) {
    int result = n;
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            while (n % i == 0) n /= i;
            result -= result / i;
        }
    }
    if (n > 1) result -= result / n;
    return result;
}
public long modularInverse(long a, long m) {
    long[] ext = extendedGcd(a, m);
    if (ext[0] != 1) throw new RuntimeException("Inverse doesn't exist");
    return (ext[1] % m + m) % m;
}
private long[] extendedGcd(long a, long b) {
    if (a == 0) return new long[]{b, 0, 1};
    long[] res = extendedGcd(b % a, a);
    return new long[]{res[0], res[2] - (b / a) * res[1], res[1]};
}
```

```cpp
int eulerTotient(int n) {
    int result = n;
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            while (n % i == 0) n /= i;
            result -= result / i;
        }
    }
    if (n > 1) result -= result / n;
    return result;
}
long long modularInverse(long long a, long long m) {
    auto ext = extendedGcd(a, m);
    if (get<0>(ext) != 1) throw runtime_error("Inverse doesn't exist");
    return (get<1>(ext) % m + m) % m;
}
tuple<long long, long long, long long> extendedGcd(long long a, long long b) {
    if (a == 0) return {b, 0, 1};
    auto [g, x1, y1] = extendedGcd(b % a, a);
    return {g, y1 - (b / a) * x1, x1};
}
```

## Common Techniques

### 1. Modular Arithmetic Properties
```python
def modular_operations():
    # Addition
    (a + b) % m = ((a % m) + (b % m)) % m
    
    # Subtraction
    (a - b) % m = ((a % m) - (b % m) + m) % m
    
    # Multiplication
    (a * b) % m = ((a % m) * (b % m)) % m
    
    # Power
    (a ^ b) % m = ((a % m) ^ b) % m
```

### 2. Chinese Remainder Theorem

**Pseudocode:**
```
Solve: x ≡ r₁ (mod m₁), x ≡ r₂ (mod m₂), ...
1. Compute M = product of all moduli
2. For each equation i:
   a. Mᵢ = M / mᵢ
   b. yᵢ = modular_inverse(Mᵢ, mᵢ)
   c. x += rᵢ × Mᵢ × yᵢ
3. Return x mod M
```

```python
def chinese_remainder(remainders: List[int], moduli: List[int]) -> int:
    """Solve system of linear congruences"""
    total = 0
    product = 1
    for modulus in moduli:
        product *= modulus
    
    for remainder, modulus in zip(remainders, moduli):
        p = product // modulus
        total += remainder * modular_inverse(p, modulus) * p
    
    return total % product
```

```java
public long chineseRemainder(int[] remainders, int[] moduli) {
    long product = 1, total = 0;
    for (int m : moduli) product *= m;
    for (int i = 0; i < remainders.length; i++) {
        long p = product / moduli[i];
        total += remainders[i] * modularInverse(p, moduli[i]) * p;
    }
    return total % product;
}
```

```cpp
long long chineseRemainder(vector<int>& remainders, vector<int>& moduli) {
    long long product = 1, total = 0;
    for (int m : moduli) product *= m;
    for (int i = 0; i < remainders.size(); i++) {
        long long p = product / moduli[i];
        total += remainders[i] * modularInverse(p, moduli[i]) * p;
    }
    return total % product;
}
```

## Edge Cases to Consider
1. Zero input
2. Negative numbers
3. Large numbers (overflow)
4. Prime numbers
5. Perfect squares
6. Power of two
7. Integer limits
8. Floating point precision
9. Division by zero
10. Modulo with negative numbers

## Common Pitfalls
1. Integer overflow
2. Division by zero
3. Floating point precision
4. Negative number handling
5. Modulo sign issues
6. Large number computation
7. Recursive depth
8. Memory constraints

## Practice Problems by Difficulty

### Easy
1. [Count Primes](https://leetcode.com/problems/count-primes/) (LC #204)
2. [Power of Three](https://leetcode.com/problems/power-of-three/) (LC #326)
3. [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) (LC #509)

### Medium
1. [Pow(x, n)](https://leetcode.com/problems/powx-n/) (LC #50)
2. [Unique Paths](https://leetcode.com/problems/unique-paths/) (LC #62)
3. [Fraction to Recurring Decimal](https://leetcode.com/problems/fraction-to-recurring-decimal/) (LC #166)
4. [Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/) (LC #227)

### Hard
1. [Basic Calculator](https://leetcode.com/problems/basic-calculator/) (LC #224)
2. [Max Points on a Line](https://leetcode.com/problems/max-points-on-a-line/) (LC #149)
3. [Number of Digit One](https://leetcode.com/problems/number-of-digit-one/) (LC #233)

## Real-World Applications
1. **Cryptography**:
   - RSA algorithm
   - Prime factorization
2. **Financial Systems**:
   - Interest calculations
   - Currency conversion
3. **Graphics**:
   - Geometric computations
   - Transformations
4. **Scientific Computing**:
   - Numerical methods
   - Statistical analysis
5. **Game Development**:
   - Physics calculations
   - Probability systems
6. **Data Analysis**:
   - Statistical computations
   - Trend analysis

## Advanced Topics
1. **Number Theory**:
   - Euler's totient
   - Quadratic residues
2. **Combinatorics**:
   - Burnside's lemma
   - Inclusion-exclusion
3. **Linear Algebra**:
   - Matrix operations
   - Eigenvalues
4. **Probability**:
   - Expected value
   - Variance
5. **Numerical Methods**:
   - Newton's method
   - Numerical integration

## Important Resources
1. [Number Theory Concepts](https://www.geeksforgeeks.org/number-theory-competitive-programming/)
2. [Modular Arithmetic](https://www.hackerearth.com/practice/math/number-theory/basic-number-theory-1/tutorial/)
3. [Prime Numbers and Factorization](https://cp-algorithms.com/algebra/factorization.html)
4. [Combinatorics Tutorial](https://www.topcoder.com/thrive/articles/Basics%20of%20Combinatorics)
5. [Mathematical Algorithms](https://www.geeksforgeeks.org/mathematical-algorithms/)

## ❓ FAQ Section

**Q: How do I handle integer overflow in math problems?**
A: (1) Use modular arithmetic: `(a * b) % MOD`, (2) Use larger data types (long long in C++, BigInteger in Java), (3) Check before operations: if `a > MAX/b` then `a*b` will overflow, (4) Rearrange formula to avoid large intermediates.

**Q: When should I use modular arithmetic?**
A: When the problem says "return answer modulo 10^9+7" (or similar). Apply mod after each operation: `(a + b) % MOD`, `(a * b) % MOD`. For division, use modular inverse instead of direct division.

**Q: How do I find GCD of two numbers quickly?**
A: Use Euclidean algorithm: `gcd(a, b) = gcd(b, a % b)`, base case `gcd(a, 0) = a`. Time complexity: O(log(min(a,b))). LCM can be found as `lcm(a, b) = (a * b) / gcd(a, b)`.

**Q: How do I check if a number is prime efficiently?**
A: Check divisibility only up to √n. For multiple queries, use Sieve of Eratosthenes to precompute all primes up to n in O(n log log n). For very large numbers, use Miller-Rabin primality test.

**Q: What is fast exponentiation and when do I use it?**
A: Compute a^n in O(log n) by squaring: if n is even, a^n = (a^(n/2))^2; if odd, a^n = a * a^(n-1). Use for large exponents, especially with modular arithmetic (modular exponentiation).

## Interview Tips
1. Verify input constraints
2. Consider edge cases
3. Handle overflow
4. Use built-in functions wisely
5. Consider optimization
6. Test with examples
7. Explain mathematical intuition
8. Consider space-time trade-offs
9. Handle precision issues
10. Think about scalability