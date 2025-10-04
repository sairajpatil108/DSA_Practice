# String Algorithms

## 📖 Table of Contents
- [Pattern Matching](#pattern-matching)
- [String Manipulation](#string-manipulation)
- [Advanced Algorithms](#advanced-algorithms)

---

## Pattern Matching

### 1. KMP (Knuth-Morris-Pratt) Algorithm

```java
// Pattern matching - O(n + m) time
public int KMP(String text, String pattern) {
    int[] lps = computeLPS(pattern);
    int i = 0;  // Index for text
    int j = 0;  // Index for pattern
    
    while (i < text.length()) {
        if (text.charAt(i) == pattern.charAt(j)) {
            i++;
            j++;
        }
        
        if (j == pattern.length()) {
            return i - j;  // Pattern found
        } else if (i < text.length() && text.charAt(i) != pattern.charAt(j)) {
            if (j != 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
    }
    
    return -1;  // Pattern not found
}

// Compute Longest Proper Prefix which is also Suffix
private int[] computeLPS(String pattern) {
    int[] lps = new int[pattern.length()];
    int len = 0;
    int i = 1;
    
    while (i < pattern.length()) {
        if (pattern.charAt(i) == pattern.charAt(len)) {
            len++;
            lps[i] = len;
            i++;
        } else {
            if (len != 0) {
                len = lps[len - 1];
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }
    
    return lps;
}
```

### 2. Rabin-Karp Algorithm

```java
// Pattern matching using hashing - O(n + m) average
public int rabinKarp(String text, String pattern) {
    int n = text.length();
    int m = pattern.length();
    int prime = 101;
    int d = 256;  // Number of characters
    
    int patternHash = 0;
    int textHash = 0;
    int h = 1;
    
    // Calculate h = d^(m-1) % prime
    for (int i = 0; i < m - 1; i++) {
        h = (h * d) % prime;
    }
    
    // Calculate initial hash values
    for (int i = 0; i < m; i++) {
        patternHash = (d * patternHash + pattern.charAt(i)) % prime;
        textHash = (d * textHash + text.charAt(i)) % prime;
    }
    
    // Slide pattern over text
    for (int i = 0; i <= n - m; i++) {
        if (patternHash == textHash) {
            // Check character by character
            boolean match = true;
            for (int j = 0; j < m; j++) {
                if (text.charAt(i + j) != pattern.charAt(j)) {
                    match = false;
                    break;
                }
            }
            
            if (match) return i;
        }
        
        // Calculate hash for next window
        if (i < n - m) {
            textHash = (d * (textHash - text.charAt(i) * h) + 
                       text.charAt(i + m)) % prime;
            
            if (textHash < 0) {
                textHash += prime;
            }
        }
    }
    
    return -1;
}
```

---

## String Manipulation

### 1. Longest Common Prefix

```java
// Find longest common prefix - O(n * m) time
public String longestCommonPrefix(String[] strs) {
    if (strs.length == 0) return "";
    
    String prefix = strs[0];
    
    for (int i = 1; i < strs.length; i++) {
        while (strs[i].indexOf(prefix) != 0) {
            prefix = prefix.substring(0, prefix.length() - 1);
            if (prefix.isEmpty()) return "";
        }
    }
    
    return prefix;
}
```

### 2. Implement strStr()

```java
// Find first occurrence of needle in haystack - O(n * m) time
public int strStr(String haystack, String needle) {
    if (needle.isEmpty()) return 0;
    
    int n = haystack.length();
    int m = needle.length();
    
    for (int i = 0; i <= n - m; i++) {
        int j = 0;
        while (j < m && haystack.charAt(i + j) == needle.charAt(j)) {
            j++;
        }
        if (j == m) return i;
    }
    
    return -1;
}
```

### 3. String Compression

```java
// Compress string - O(n) time
public String compress(String s) {
    StringBuilder result = new StringBuilder();
    int count = 1;
    
    for (int i = 1; i <= s.length(); i++) {
        if (i < s.length() && s.charAt(i) == s.charAt(i - 1)) {
            count++;
        } else {
            result.append(s.charAt(i - 1));
            if (count > 1) {
                result.append(count);
            }
            count = 1;
        }
    }
    
    return result.length() < s.length() ? result.toString() : s;
}
```

### 4. Reverse Words in String

```java
// Reverse words - O(n) time
public String reverseWords(String s) {
    String[] words = s.trim().split("\\s+");
    StringBuilder result = new StringBuilder();
    
    for (int i = words.length - 1; i >= 0; i--) {
        result.append(words[i]);
        if (i > 0) result.append(" ");
    }
    
    return result.toString();
}
```

### 5. Longest Repeating Character Replacement

```java
// Longest substring with same chars after k replacements - O(n) time
public int characterReplacement(String s, int k) {
    int[] count = new int[26];
    int maxCount = 0;
    int maxLength = 0;
    int left = 0;
    
    for (int right = 0; right < s.length(); right++) {
        maxCount = Math.max(maxCount, ++count[s.charAt(right) - 'A']);
        
        // If replacements needed > k, shrink window
        while (right - left + 1 - maxCount > k) {
            count[s.charAt(left) - 'A']--;
            left++;
        }
        
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

---

## Advanced Algorithms

### 1. Z-Algorithm

```java
// Pattern matching using Z-array - O(n + m) time
public int[] computeZArray(String s) {
    int n = s.length();
    int[] z = new int[n];
    int left = 0, right = 0;
    
    for (int i = 1; i < n; i++) {
        if (i > right) {
            left = right = i;
            while (right < n && s.charAt(right) == s.charAt(right - left)) {
                right++;
            }
            z[i] = right - left;
            right--;
        } else {
            int k = i - left;
            if (z[k] < right - i + 1) {
                z[i] = z[k];
            } else {
                left = i;
                while (right < n && s.charAt(right) == s.charAt(right - left)) {
                    right++;
                }
                z[i] = right - left;
                right--;
            }
        }
    }
    
    return z;
}
```

### 2. Manacher's Algorithm (Longest Palindrome)

```java
// Find longest palindromic substring - O(n) time
public String longestPalindrome(String s) {
    if (s == null || s.length() == 0) return "";
    
    // Transform string
    String t = "#";
    for (char c : s.toCharArray()) {
        t += c + "#";
    }
    
    int n = t.length();
    int[] p = new int[n];  // Palindrome radius
    int center = 0;
    int right = 0;
    
    for (int i = 0; i < n; i++) {
        int mirror = 2 * center - i;
        
        if (i < right) {
            p[i] = Math.min(right - i, p[mirror]);
        }
        
        // Try to expand
        int a = i + (1 + p[i]);
        int b = i - (1 + p[i]);
        while (a < n && b >= 0 && t.charAt(a) == t.charAt(b)) {
            p[i]++;
            a++;
            b--;
        }
        
        // Update center and right
        if (i + p[i] > right) {
            center = i;
            right = i + p[i];
        }
    }
    
    // Find maximum palindrome
    int maxLen = 0;
    int centerIndex = 0;
    for (int i = 0; i < n; i++) {
        if (p[i] > maxLen) {
            maxLen = p[i];
            centerIndex = i;
        }
    }
    
    int start = (centerIndex - maxLen) / 2;
    return s.substring(start, start + maxLen);
}
```

### 3. Suffix Array

```java
// Build suffix array - O(n log n) time
public int[] buildSuffixArray(String s) {
    int n = s.length();
    Integer[] suffixes = new Integer[n];
    
    for (int i = 0; i < n; i++) {
        suffixes[i] = i;
    }
    
    Arrays.sort(suffixes, (i, j) -> 
        s.substring(i).compareTo(s.substring(j)));
    
    return Arrays.stream(suffixes).mapToInt(Integer::intValue).toArray();
}
```

---

## Key Takeaways

1. **KMP**: O(n+m) pattern matching with preprocessing
2. **Rabin-Karp**: Hashing for pattern matching
3. **Two Pointers**: Common for string manipulation
4. **Sliding Window**: For substring problems
5. **Z-Algorithm**: Linear time pattern matching

**Interview Tip**: Consider preprocessing for multiple queries!

