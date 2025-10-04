# Math and Number Theory

## 📖 Table of Contents
- [Basic Math](#basic-math)
- [Prime Numbers](#prime-numbers)
- [GCD and LCM](#gcd-and-lcm)
- [Modular Arithmetic](#modular-arithmetic)
- [Number Theory Problems](#number-theory-problems)

---

## Basic Math

### 1. Power and Square Root

```java
// Calculate power - O(log n) time
public long power(long x, long n) {
    if (n == 0) return 1;
    
    long half = power(x, n / 2);
    
    if (n % 2 == 0) {
        return half * half;
    } else {
        return half * half * x;
    }
}

// Integer square root - O(log n) time
public int sqrt(int x) {
    if (x == 0) return 0;
    
    int left = 1;
    int right = x;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (mid == x / mid) {
            return mid;
        } else if (mid < x / mid) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return right;
}
```

### 2. Factorial

```java
// Calculate factorial - O(n) time
public long factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

// Factorial with modulo
public long factorialMod(int n, int mod) {
    long result = 1;
    for (int i = 2; i <= n; i++) {
        result = (result * i) % mod;
    }
    return result;
}
```

### 3. Fibonacci

```java
// Matrix exponentiation for Fibonacci - O(log n) time
public long fibonacci(int n) {
    if (n <= 1) return n;
    
    long[][] base = {{1, 1}, {1, 0}};
    long[][] result = matrixPower(base, n - 1);
    
    return result[0][0];
}

private long[][] matrixPower(long[][] matrix, int n) {
    if (n == 1) return matrix;
    
    long[][] half = matrixPower(matrix, n / 2);
    long[][] result = matrixMultiply(half, half);
    
    if (n % 2 == 1) {
        result = matrixMultiply(result, matrix);
    }
    
    return result;
}

private long[][] matrixMultiply(long[][] a, long[][] b) {
    long[][] result = new long[2][2];
    
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 2; j++) {
            for (int k = 0; k < 2; k++) {
                result[i][j] += a[i][k] * b[k][j];
            }
        }
    }
    
    return result;
}
```

---

## Prime Numbers

### 1. Check Prime

```java
// Check if number is prime - O(√n) time
public boolean isPrime(int n) {
    if (n <= 1) return false;
    if (n <= 3) return true;
    if (n % 2 == 0 || n % 3 == 0) return false;
    
    for (int i = 5; i * i <= n; i += 6) {
        if (n % i == 0 || n % (i + 2) == 0) {
            return false;
        }
    }
    
    return true;
}
```

### 2. Sieve of Eratosthenes

```java
// Find all primes up to n - O(n log log n) time
public List<Integer> sieveOfEratosthenes(int n) {
    boolean[] isPrime = new boolean[n + 1];
    Arrays.fill(isPrime, true);
    isPrime[0] = isPrime[1] = false;
    
    for (int i = 2; i * i <= n; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= n; j += i) {
                isPrime[j] = false;
            }
        }
    }
    
    List<Integer> primes = new ArrayList<>();
    for (int i = 2; i <= n; i++) {
        if (isPrime[i]) {
            primes.add(i);
        }
    }
    
    return primes;
}
```

### 3. Prime Factorization

```java
// Find prime factors - O(√n) time
public List<Integer> primeFactors(int n) {
    List<Integer> factors = new ArrayList<>();
    
    // Divide by 2
    while (n % 2 == 0) {
        factors.add(2);
        n /= 2;
    }
    
    // Check odd factors
    for (int i = 3; i * i <= n; i += 2) {
        while (n % i == 0) {
            factors.add(i);
            n /= i;
        }
    }
    
    // If n is prime > 2
    if (n > 2) {
        factors.add(n);
    }
    
    return factors;
}
```

---

## GCD and LCM

### 1. GCD (Euclidean Algorithm)

```java
// Greatest Common Divisor - O(log min(a,b)) time
public int gcd(int a, int b) {
    if (b == 0) return a;
    return gcd(b, a % b);
}

// Iterative GCD
public int gcdIterative(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```

### 2. LCM

```java
// Least Common Multiple - O(log min(a,b)) time
public int lcm(int a, int b) {
    return (a * b) / gcd(a, b);
}
```

### 3. Extended Euclidean Algorithm

```java
// Find x, y such that ax + by = gcd(a,b)
public int[] extendedGCD(int a, int b) {
    if (b == 0) {
        return new int[]{a, 1, 0};
    }
    
    int[] result = extendedGCD(b, a % b);
    int gcd = result[0];
    int x = result[2];
    int y = result[1] - (a / b) * result[2];
    
    return new int[]{gcd, x, y};
}
```

---

## Modular Arithmetic

### 1. Modular Exponentiation

```java
// Calculate (base^exp) % mod - O(log exp) time
public long modPower(long base, long exp, long mod) {
    long result = 1;
    base %= mod;
    
    while (exp > 0) {
        if (exp % 2 == 1) {
            result = (result * base) % mod;
        }
        base = (base * base) % mod;
        exp /= 2;
    }
    
    return result;
}
```

### 2. Modular Inverse

```java
// Calculate modular inverse using Fermat's theorem
// Works when mod is prime
public long modInverse(long a, long mod) {
    return modPower(a, mod - 2, mod);
}

// Modular inverse using Extended Euclidean
public long modInverseExtended(long a, long mod) {
    int[] result = extendedGCD((int)a, (int)mod);
    
    if (result[0] != 1) {
        return -1;  // Inverse doesn't exist
    }
    
    return (result[1] % mod + mod) % mod;
}
```

### 3. Chinese Remainder Theorem

```java
// Solve system of congruences
public long chineseRemainder(int[] remainders, int[] moduli) {
    long product = 1;
    for (int mod : moduli) {
        product *= mod;
    }
    
    long result = 0;
    
    for (int i = 0; i < remainders.length; i++) {
        long p = product / moduli[i];
        result += remainders[i] * modInverse(p, moduli[i]) * p;
    }
    
    return result % product;
}
```

---

## Number Theory Problems

### 1. Happy Number

```java
// Check if happy number - O(log n) time
public boolean isHappy(int n) {
    Set<Integer> seen = new HashSet<>();
    
    while (n != 1 && !seen.contains(n)) {
        seen.add(n);
        n = getNext(n);
    }
    
    return n == 1;
}

private int getNext(int n) {
    int sum = 0;
    while (n > 0) {
        int digit = n % 10;
        sum += digit * digit;
        n /= 10;
    }
    return sum;
}
```

### 2. Ugly Number

```java
// Check if ugly number (only factors 2, 3, 5) - O(log n) time
public boolean isUgly(int n) {
    if (n <= 0) return false;
    
    while (n % 2 == 0) n /= 2;
    while (n % 3 == 0) n /= 3;
    while (n % 5 == 0) n /= 5;
    
    return n == 1;
}

// Find nth ugly number - O(n) time
public int nthUglyNumber(int n) {
    int[] dp = new int[n];
    dp[0] = 1;
    
    int i2 = 0, i3 = 0, i5 = 0;
    
    for (int i = 1; i < n; i++) {
        int next2 = dp[i2] * 2;
        int next3 = dp[i3] * 3;
        int next5 = dp[i5] * 5;
        
        dp[i] = Math.min(next2, Math.min(next3, next5));
        
        if (dp[i] == next2) i2++;
        if (dp[i] == next3) i3++;
        if (dp[i] == next5) i5++;
    }
    
    return dp[n - 1];
}
```

### 3. Perfect Square

```java
// Check if perfect square - O(log n) time
public boolean isPerfectSquare(int num) {
    int left = 1;
    int right = num;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        long square = (long) mid * mid;
        
        if (square == num) {
            return true;
        } else if (square < num) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return false;
}
```

### 4. Count Primes

```java
// Count primes less than n - O(n log log n) time
public int countPrimes(int n) {
    if (n <= 2) return 0;
    
    boolean[] isPrime = new boolean[n];
    Arrays.fill(isPrime, true);
    
    for (int i = 2; i * i < n; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j < n; j += i) {
                isPrime[j] = false;
            }
        }
    }
    
    int count = 0;
    for (int i = 2; i < n; i++) {
        if (isPrime[i]) count++;
    }
    
    return count;
}
```

### 5. Trailing Zeros in Factorial

```java
// Count trailing zeros in n! - O(log n) time
public int trailingZeroes(int n) {
    int count = 0;
    
    while (n > 0) {
        n /= 5;
        count += n;
    }
    
    return count;
}
```

---

## Key Takeaways

1. **Modular Arithmetic**: Essential for large numbers
2. **GCD/LCM**: Euclidean algorithm is efficient
3. **Prime Numbers**: Sieve for multiple queries
4. **Power**: Use fast exponentiation
5. **Overflow**: Be careful with large products

**Interview Tip**: Consider mathematical properties to optimize brute force!

