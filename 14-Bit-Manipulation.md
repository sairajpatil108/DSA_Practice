# Bit Manipulation

## 📖 Table of Contents
- [Bit Operations](#bit-operations)
- [Common Tricks](#common-tricks)
- [Classic Problems](#classic-problems)

---

## Bit Operations

### Basic Operations

```java
public class BitOperations {
    
    // AND: Both bits must be 1
    public int and(int a, int b) {
        return a & b;  // 5 & 3 = 0101 & 0011 = 0001 = 1
    }
    
    // OR: At least one bit must be 1
    public int or(int a, int b) {
        return a | b;  // 5 | 3 = 0101 | 0011 = 0111 = 7
    }
    
    // XOR: Bits must be different
    public int xor(int a, int b) {
        return a ^ b;  // 5 ^ 3 = 0101 ^ 0011 = 0110 = 6
    }
    
    // NOT: Flip all bits
    public int not(int a) {
        return ~a;  // ~5 = ~0101 = 1010 (in 32-bit: -6)
    }
    
    // Left Shift: Multiply by 2^n
    public int leftShift(int a, int n) {
        return a << n;  // 5 << 2 = 0101 << 2 = 10100 = 20
    }
    
    // Right Shift: Divide by 2^n
    public int rightShift(int a, int n) {
        return a >> n;  // 5 >> 1 = 0101 >> 1 = 0010 = 2
    }
    
    // Unsigned Right Shift: Fill with 0
    public int unsignedRightShift(int a, int n) {
        return a >>> n;
    }
}
```

---

## Common Tricks

### 1. Check if Number is Power of 2

```java
// Check if power of 2 - O(1) time
public boolean isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}
```

### 2. Count Set Bits (Brian Kernighan's Algorithm)

```java
// Count number of 1s in binary - O(number of 1s) time
public int hammingWeight(int n) {
    int count = 0;
    while (n != 0) {
        n = n & (n - 1);  // Remove rightmost 1
        count++;
    }
    return count;
}

// Alternative: Loop through each bit
public int hammingWeightLoop(int n) {
    int count = 0;
    for (int i = 0; i < 32; i++) {
        if ((n & (1 << i)) != 0) {
            count++;
        }
    }
    return count;
}
```

### 3. Get, Set, Clear, Toggle Bit

```java
// Get bit at position i
public boolean getBit(int num, int i) {
    return (num & (1 << i)) != 0;
}

// Set bit at position i to 1
public int setBit(int num, int i) {
    return num | (1 << i);
}

// Clear bit at position i to 0
public int clearBit(int num, int i) {
    return num & ~(1 << i);
}

// Toggle bit at position i
public int toggleBit(int num, int i) {
    return num ^ (1 << i);
}

// Update bit at position i to value
public int updateBit(int num, int i, boolean value) {
    int mask = ~(1 << i);
    return (num & mask) | ((value ? 1 : 0) << i);
}
```

### 4. Clear Bits

```java
// Clear all bits from i to 0
public int clearBitsIThrough0(int num, int i) {
    int mask = (-1 << (i + 1));
    return num & mask;
}

// Clear all bits from MSB to i
public int clearBitsMSBThroughI(int num, int i) {
    int mask = (1 << i) - 1;
    return num & mask;
}
```

### 5. Swap Two Numbers

```java
// Swap without temp variable using XOR
public void swap(int[] arr, int i, int j) {
    arr[i] = arr[i] ^ arr[j];
    arr[j] = arr[i] ^ arr[j];
    arr[i] = arr[i] ^ arr[j];
}
```

---

## Classic Problems

### 1. Single Number

```java
// Find element that appears once (others appear twice) - O(n) time
public int singleNumber(int[] nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;  // XOR cancels out pairs
    }
    return result;
}

// Single number when others appear three times
public int singleNumberIII(int[] nums) {
    int ones = 0, twos = 0;
    
    for (int num : nums) {
        twos |= ones & num;
        ones ^= num;
        
        int threes = ones & twos;
        ones &= ~threes;
        twos &= ~threes;
    }
    
    return ones;
}
```

### 2. Missing Number

```java
// Find missing number in [0, n] - O(n) time, O(1) space
public int missingNumber(int[] nums) {
    int result = nums.length;
    
    for (int i = 0; i < nums.length; i++) {
        result ^= i ^ nums[i];
    }
    
    return result;
}
```

### 3. Reverse Bits

```java
// Reverse bits of 32-bit integer - O(1) time
public int reverseBits(int n) {
    int result = 0;
    
    for (int i = 0; i < 32; i++) {
        result <<= 1;  // Shift result left
        result |= (n & 1);  // Add rightmost bit of n
        n >>= 1;  // Shift n right
    }
    
    return result;
}
```

### 4. Hamming Distance

```java
// Number of positions where bits differ - O(1) time
public int hammingDistance(int x, int y) {
    int xor = x ^ y;
    int count = 0;
    
    while (xor != 0) {
        count++;
        xor &= (xor - 1);  // Remove rightmost 1
    }
    
    return count;
}
```

### 5. Power of Four

```java
// Check if power of 4 - O(1) time
public boolean isPowerOfFour(int n) {
    // Must be power of 2 AND 1s only at odd positions
    return n > 0 && 
           (n & (n - 1)) == 0 && 
           (n & 0xAAAAAAAA) == 0;
}
```

### 6. Sum of Two Integers (Without +/-)

```java
// Add two integers without arithmetic operators - O(1) time
public int getSum(int a, int b) {
    while (b != 0) {
        int carry = a & b;  // Find carry
        a = a ^ b;  // Add without carry
        b = carry << 1;  // Shift carry
    }
    return a;
}
```

### 7. Maximum XOR of Two Numbers

```java
// Find maximum XOR of two numbers in array - O(n) time
public int findMaximumXOR(int[] nums) {
    int max = 0;
    int mask = 0;
    
    for (int i = 31; i >= 0; i--) {
        mask |= (1 << i);
        Set<Integer> prefixes = new HashSet<>();
        
        for (int num : nums) {
            prefixes.add(num & mask);
        }
        
        int candidate = max | (1 << i);
        
        for (int prefix : prefixes) {
            if (prefixes.contains(candidate ^ prefix)) {
                max = candidate;
                break;
            }
        }
    }
    
    return max;
}
```

### 8. Bitwise AND of Range

```java
// Bitwise AND of all numbers in [left, right] - O(log n) time
public int rangeBitwiseAnd(int left, int right) {
    int shift = 0;
    
    // Find common prefix
    while (left < right) {
        left >>= 1;
        right >>= 1;
        shift++;
    }
    
    return left << shift;
}
```

### 9. Gray Code

```java
// Generate Gray code sequence - O(2ⁿ) time
public List<Integer> grayCode(int n) {
    List<Integer> result = new ArrayList<>();
    
    for (int i = 0; i < (1 << n); i++) {
        result.add(i ^ (i >> 1));
    }
    
    return result;
}
```

### 10. Subsets Using Bit Manipulation

```java
// Generate all subsets using bits - O(n * 2ⁿ) time
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    int n = nums.length;
    int totalSubsets = 1 << n;  // 2^n
    
    for (int mask = 0; mask < totalSubsets; mask++) {
        List<Integer> subset = new ArrayList<>();
        
        for (int i = 0; i < n; i++) {
            if ((mask & (1 << i)) != 0) {
                subset.add(nums[i]);
            }
        }
        
        result.add(subset);
    }
    
    return result;
}
```

---

## Important Bit Facts

```java
// Common patterns and identities
public class BitFacts {
    
    // x ^ 0 = x
    // x ^ x = 0
    // x ^ ~x = -1
    // x & 0 = 0
    // x & x = x
    // x & ~x = 0
    // x | 0 = x
    // x | x = x
    // x | ~x = -1
    
    // Multiply by 2: x << 1
    // Divide by 2: x >> 1
    // Multiply by 2^k: x << k
    // Divide by 2^k: x >> k
    
    // Check if odd: (x & 1) == 1
    // Check if even: (x & 1) == 0
    
    // Get rightmost 1: x & -x
    // Remove rightmost 1: x & (x - 1)
    // Get all 1s: ~0
    
    // Check if opposite signs
    public boolean oppositeSigns(int x, int y) {
        return (x ^ y) < 0;
    }
    
    // Add 1
    public int addOne(int x) {
        return -~x;
    }
    
    // Subtract 1
    public int subtractOne(int x) {
        return ~-x;
    }
    
    // Get absolute value
    public int abs(int x) {
        int mask = x >> 31;
        return (x + mask) ^ mask;
    }
    
    // Find min without if
    public int min(int x, int y) {
        return y ^ ((x ^ y) & -(x < y ? 1 : 0));
    }
    
    // Find max without if
    public int max(int x, int y) {
        return x ^ ((x ^ y) & -(x < y ? 1 : 0));
    }
}
```

---

## Key Takeaways

1. **XOR**: Cancels out pairs, useful for finding unique elements
2. **AND**: Check/clear bits, powers of 2
3. **OR**: Set bits
4. **Shift**: Multiply/divide by powers of 2
5. **n & (n-1)**: Remove rightmost 1 bit

**Interview Tip**: Draw bits on paper to visualize operations!

