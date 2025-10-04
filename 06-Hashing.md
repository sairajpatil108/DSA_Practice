# Hashing

## 📖 Table of Contents
- [Hash Table Basics](#hash-table-basics)
- [Hash Functions](#hash-functions)
- [Collision Resolution](#collision-resolution)
- [HashMap in Java](#hashmap-in-java)
- [HashSet in Java](#hashset-in-java)
- [Custom HashMap Implementation](#custom-hashmap-implementation)
- [Important Problems](#important-problems)

---

## Hash Table Basics

### What is Hashing?
Hashing is a technique to map keys to values using a hash function. It provides O(1) average time for insert, delete, and search operations.

**Key Components:**
1. **Hash Function**: Converts key to array index
2. **Hash Table**: Array to store key-value pairs
3. **Collision Handling**: Strategy when multiple keys hash to same index

**Time Complexity:**
- Average: O(1) for insert, delete, search
- Worst: O(n) when all elements collide

**Space Complexity:** O(n)

---

## Hash Functions

### Good Hash Function Properties:
1. **Deterministic**: Same key always produces same hash
2. **Uniform Distribution**: Minimizes collisions
3. **Efficient**: Fast to compute
4. **Avalanche Effect**: Small change in key drastically changes hash

### Common Hash Functions

```java
public class HashFunctions {
    
    // Simple hash for integers
    public int hashInt(int key, int tableSize) {
        return key % tableSize;
    }
    
    // Hash for strings (polynomial rolling hash)
    public int hashString(String key, int tableSize) {
        int hash = 0;
        int prime = 31;  // Prime number for better distribution
        
        for (int i = 0; i < key.length(); i++) {
            hash = (hash * prime + key.charAt(i)) % tableSize;
        }
        
        return Math.abs(hash);
    }
    
    // Division method
    public int divisionMethod(int key, int tableSize) {
        return key % tableSize;
    }
    
    // Multiplication method
    public int multiplicationMethod(int key, int tableSize) {
        double A = 0.6180339887;  // Golden ratio
        double fractional = (key * A) % 1;
        return (int) (tableSize * fractional);
    }
    
    // Universal hashing
    public int universalHash(int key, int tableSize, int a, int b, int p) {
        // h(k) = ((a*k + b) mod p) mod m
        // p is a prime larger than universe size
        return ((a * key + b) % p) % tableSize;
    }
}
```

---

## Collision Resolution

### 1. Chaining (Open Hashing)

Each bucket contains a linked list of elements that hash to same index.

```java
import java.util.LinkedList;

class HashNode<K, V> {
    K key;
    V value;
    
    public HashNode(K key, V value) {
        this.key = key;
        this.value = value;
    }
}

public class HashMapChaining<K, V> {
    private LinkedList<HashNode<K, V>>[] buckets;
    private int numBuckets;
    private int size;
    
    @SuppressWarnings("unchecked")
    public HashMapChaining(int capacity) {
        buckets = new LinkedList[capacity];
        numBuckets = capacity;
        size = 0;
        
        // Initialize buckets
        for (int i = 0; i < numBuckets; i++) {
            buckets[i] = new LinkedList<>();
        }
    }
    
    // Hash function
    private int getBucketIndex(K key) {
        int hashCode = key.hashCode();
        return Math.abs(hashCode) % numBuckets;
    }
    
    // Put key-value pair - Average O(1)
    public void put(K key, V value) {
        int bucketIndex = getBucketIndex(key);
        LinkedList<HashNode<K, V>> bucket = buckets[bucketIndex];
        
        // Update if key exists
        for (HashNode<K, V> node : bucket) {
            if (node.key.equals(key)) {
                node.value = value;
                return;
            }
        }
        
        // Add new node
        bucket.add(new HashNode<>(key, value));
        size++;
        
        // Rehash if load factor > 0.75
        if ((1.0 * size) / numBuckets >= 0.75) {
            rehash();
        }
    }
    
    // Get value by key - Average O(1)
    public V get(K key) {
        int bucketIndex = getBucketIndex(key);
        LinkedList<HashNode<K, V>> bucket = buckets[bucketIndex];
        
        for (HashNode<K, V> node : bucket) {
            if (node.key.equals(key)) {
                return node.value;
            }
        }
        
        return null;  // Key not found
    }
    
    // Remove key - Average O(1)
    public V remove(K key) {
        int bucketIndex = getBucketIndex(key);
        LinkedList<HashNode<K, V>> bucket = buckets[bucketIndex];
        
        HashNode<K, V> toRemove = null;
        for (HashNode<K, V> node : bucket) {
            if (node.key.equals(key)) {
                toRemove = node;
                break;
            }
        }
        
        if (toRemove == null) {
            return null;
        }
        
        bucket.remove(toRemove);
        size--;
        return toRemove.value;
    }
    
    // Rehash to larger table - O(n)
    @SuppressWarnings("unchecked")
    private void rehash() {
        LinkedList<HashNode<K, V>>[] temp = buckets;
        numBuckets = 2 * numBuckets;
        buckets = new LinkedList[numBuckets];
        size = 0;
        
        for (int i = 0; i < numBuckets; i++) {
            buckets[i] = new LinkedList<>();
        }
        
        // Rehash all entries
        for (LinkedList<HashNode<K, V>> bucket : temp) {
            for (HashNode<K, V> node : bucket) {
                put(node.key, node.value);
            }
        }
    }
    
    public int size() {
        return size;
    }
    
    public boolean isEmpty() {
        return size == 0;
    }
}
```

### 2. Open Addressing (Closed Hashing)

All elements stored in the hash table itself. When collision occurs, probe for next empty slot.

#### Linear Probing

```java
public class HashMapLinearProbing<K, V> {
    private K[] keys;
    private V[] values;
    private int capacity;
    private int size;
    
    @SuppressWarnings("unchecked")
    public HashMapLinearProbing(int capacity) {
        this.capacity = capacity;
        keys = (K[]) new Object[capacity];
        values = (V[]) new Object[capacity];
        size = 0;
    }
    
    private int hash(K key) {
        return Math.abs(key.hashCode()) % capacity;
    }
    
    // Put with linear probing - Average O(1)
    public void put(K key, V value) {
        if (size >= capacity / 2) {
            resize(2 * capacity);
        }
        
        int i = hash(key);
        
        // Linear probing
        while (keys[i] != null) {
            if (keys[i].equals(key)) {
                values[i] = value;  // Update existing
                return;
            }
            i = (i + 1) % capacity;  // Next slot
        }
        
        keys[i] = key;
        values[i] = value;
        size++;
    }
    
    // Get with linear probing - Average O(1)
    public V get(K key) {
        int i = hash(key);
        
        while (keys[i] != null) {
            if (keys[i].equals(key)) {
                return values[i];
            }
            i = (i + 1) % capacity;
        }
        
        return null;
    }
    
    // Remove with linear probing
    public void remove(K key) {
        int i = hash(key);
        
        // Find key
        while (keys[i] != null && !keys[i].equals(key)) {
            i = (i + 1) % capacity;
        }
        
        if (keys[i] == null) return;
        
        // Remove key
        keys[i] = null;
        values[i] = null;
        size--;
        
        // Rehash all keys in cluster
        i = (i + 1) % capacity;
        while (keys[i] != null) {
            K keyToRehash = keys[i];
            V valueToRehash = values[i];
            keys[i] = null;
            values[i] = null;
            size--;
            put(keyToRehash, valueToRehash);
            i = (i + 1) % capacity;
        }
    }
    
    @SuppressWarnings("unchecked")
    private void resize(int newCapacity) {
        HashMapLinearProbing<K, V> temp = 
            new HashMapLinearProbing<>(newCapacity);
        
        for (int i = 0; i < capacity; i++) {
            if (keys[i] != null) {
                temp.put(keys[i], values[i]);
            }
        }
        
        keys = temp.keys;
        values = temp.values;
        capacity = temp.capacity;
    }
}
```

#### Quadratic Probing

```java
// Quadratic probing: h(k, i) = (h(k) + c1*i + c2*i^2) mod m
public int quadraticProbe(int hash, int i, int capacity) {
    int c1 = 1;
    int c2 = 3;
    return (hash + c1 * i + c2 * i * i) % capacity;
}
```

#### Double Hashing

```java
// Double hashing: h(k, i) = (h1(k) + i*h2(k)) mod m
public int doubleHash(int key, int i, int capacity) {
    int h1 = key % capacity;
    int h2 = 1 + (key % (capacity - 1));  // Secondary hash
    return (h1 + i * h2) % capacity;
}
```

---

## HashMap in Java

```java
import java.util.HashMap;
import java.util.Map;

public class HashMapOperations {
    
    public void basicOperations() {
        HashMap<String, Integer> map = new HashMap<>();
        
        // Put - O(1) average
        map.put("apple", 5);
        map.put("banana", 3);
        map.put("orange", 7);
        
        // Get - O(1) average
        int count = map.get("apple");  // 5
        
        // Get with default
        int def = map.getOrDefault("grape", 0);  // 0
        
        // Contains key - O(1) average
        boolean hasApple = map.containsKey("apple");  // true
        
        // Contains value - O(n)
        boolean hasThree = map.containsValue(3);  // true
        
        // Remove - O(1) average
        map.remove("banana");
        
        // Size
        int size = map.size();
        
        // Check if empty
        boolean empty = map.isEmpty();
        
        // Clear all entries
        map.clear();
    }
    
    public void iterationMethods() {
        HashMap<String, Integer> map = new HashMap<>();
        map.put("apple", 5);
        map.put("banana", 3);
        map.put("orange", 7);
        
        // Iterate through keys
        for (String key : map.keySet()) {
            System.out.println(key + ": " + map.get(key));
        }
        
        // Iterate through values
        for (Integer value : map.values()) {
            System.out.println(value);
        }
        
        // Iterate through entries
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
        
        // Java 8+ forEach
        map.forEach((key, value) -> 
            System.out.println(key + ": " + value)
        );
    }
    
    public void advancedOperations() {
        HashMap<String, Integer> map = new HashMap<>();
        
        // putIfAbsent - only puts if key doesn't exist
        map.putIfAbsent("apple", 5);
        
        // compute - compute new value for key
        map.compute("apple", (key, val) -> val == null ? 1 : val + 1);
        
        // computeIfAbsent - compute only if absent
        map.computeIfAbsent("banana", key -> 0);
        
        // computeIfPresent - compute only if present
        map.computeIfPresent("apple", (key, val) -> val + 1);
        
        // merge - merge old and new values
        map.merge("apple", 5, (oldVal, newVal) -> oldVal + newVal);
        
        // replace - replace value for existing key
        map.replace("apple", 10);
        
        // replace with check
        map.replace("apple", 10, 15);
    }
}
```

---

## HashSet in Java

```java
import java.util.HashSet;
import java.util.Set;

public class HashSetOperations {
    
    public void basicOperations() {
        HashSet<String> set = new HashSet<>();
        
        // Add - O(1) average
        set.add("apple");
        set.add("banana");
        set.add("orange");
        set.add("apple");  // Duplicate, not added
        
        // Contains - O(1) average
        boolean hasApple = set.contains("apple");  // true
        
        // Remove - O(1) average
        set.remove("banana");
        
        // Size
        int size = set.size();  // 2
        
        // Check if empty
        boolean empty = set.isEmpty();
        
        // Clear
        set.clear();
    }
    
    public void setOperations() {
        HashSet<Integer> set1 = new HashSet<>();
        set1.add(1);
        set1.add(2);
        set1.add(3);
        
        HashSet<Integer> set2 = new HashSet<>();
        set2.add(2);
        set2.add(3);
        set2.add(4);
        
        // Union
        HashSet<Integer> union = new HashSet<>(set1);
        union.addAll(set2);  // {1, 2, 3, 4}
        
        // Intersection
        HashSet<Integer> intersection = new HashSet<>(set1);
        intersection.retainAll(set2);  // {2, 3}
        
        // Difference
        HashSet<Integer> difference = new HashSet<>(set1);
        difference.removeAll(set2);  // {1}
    }
    
    public void iteration() {
        HashSet<String> set = new HashSet<>();
        set.add("apple");
        set.add("banana");
        
        // Enhanced for loop
        for (String fruit : set) {
            System.out.println(fruit);
        }
        
        // Iterator
        Iterator<String> iterator = set.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
        
        // Java 8+ forEach
        set.forEach(System.out::println);
    }
}
```

---

## Custom HashMap Implementation

```java
// Complete custom HashMap implementation
public class CustomHashMap<K, V> {
    private static class Entry<K, V> {
        K key;
        V value;
        Entry<K, V> next;
        
        Entry(K key, V value) {
            this.key = key;
            this.value = value;
        }
    }
    
    private Entry<K, V>[] table;
    private int capacity;
    private int size;
    private static final int DEFAULT_CAPACITY = 16;
    private static final float LOAD_FACTOR = 0.75f;
    
    @SuppressWarnings("unchecked")
    public CustomHashMap() {
        table = new Entry[DEFAULT_CAPACITY];
        capacity = DEFAULT_CAPACITY;
        size = 0;
    }
    
    private int hash(K key) {
        return Math.abs(key.hashCode()) % capacity;
    }
    
    public void put(K key, V value) {
        if (key == null) return;
        
        int index = hash(key);
        Entry<K, V> entry = table[index];
        
        // Update existing key
        while (entry != null) {
            if (entry.key.equals(key)) {
                entry.value = value;
                return;
            }
            entry = entry.next;
        }
        
        // Add new entry at beginning of chain
        Entry<K, V> newEntry = new Entry<>(key, value);
        newEntry.next = table[index];
        table[index] = newEntry;
        size++;
        
        // Rehash if needed
        if (size >= capacity * LOAD_FACTOR) {
            rehash();
        }
    }
    
    public V get(K key) {
        if (key == null) return null;
        
        int index = hash(key);
        Entry<K, V> entry = table[index];
        
        while (entry != null) {
            if (entry.key.equals(key)) {
                return entry.value;
            }
            entry = entry.next;
        }
        
        return null;
    }
    
    public boolean containsKey(K key) {
        return get(key) != null;
    }
    
    public V remove(K key) {
        if (key == null) return null;
        
        int index = hash(key);
        Entry<K, V> entry = table[index];
        Entry<K, V> prev = null;
        
        while (entry != null) {
            if (entry.key.equals(key)) {
                if (prev == null) {
                    table[index] = entry.next;
                } else {
                    prev.next = entry.next;
                }
                size--;
                return entry.value;
            }
            prev = entry;
            entry = entry.next;
        }
        
        return null;
    }
    
    @SuppressWarnings("unchecked")
    private void rehash() {
        Entry<K, V>[] oldTable = table;
        capacity *= 2;
        table = new Entry[capacity];
        size = 0;
        
        for (Entry<K, V> entry : oldTable) {
            while (entry != null) {
                put(entry.key, entry.value);
                entry = entry.next;
            }
        }
    }
    
    public int size() {
        return size;
    }
    
    public boolean isEmpty() {
        return size == 0;
    }
}
```

---

## Important Problems

### 1. Two Sum

```java
// Find two numbers that add up to target - O(n) time, O(n) space
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        
        map.put(nums[i], i);
    }
    
    return new int[]{};
}
```

### 2. Group Anagrams

```java
// Group anagrams together - O(n * k log k) time
public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> map = new HashMap<>();
    
    for (String str : strs) {
        // Sort string as key
        char[] chars = str.toCharArray();
        Arrays.sort(chars);
        String key = String.valueOf(chars);
        
        map.putIfAbsent(key, new ArrayList<>());
        map.get(key).add(str);
    }
    
    return new ArrayList<>(map.values());
}
```

### 3. Longest Consecutive Sequence

```java
// Find longest consecutive sequence - O(n) time
public int longestConsecutive(int[] nums) {
    Set<Integer> set = new HashSet<>();
    for (int num : nums) {
        set.add(num);
    }
    
    int longest = 0;
    
    for (int num : set) {
        // Only start counting if it's the start of sequence
        if (!set.contains(num - 1)) {
            int current = num;
            int count = 1;
            
            while (set.contains(current + 1)) {
                current++;
                count++;
            }
            
            longest = Math.max(longest, count);
        }
    }
    
    return longest;
}
```

### 4. Subarray Sum Equals K

```java
// Count subarrays with sum = k - O(n) time
public int subarraySum(int[] nums, int k) {
    Map<Integer, Integer> prefixSumCount = new HashMap<>();
    prefixSumCount.put(0, 1);  // Base case
    
    int sum = 0;
    int count = 0;
    
    for (int num : nums) {
        sum += num;
        
        // Check if (sum - k) exists
        if (prefixSumCount.containsKey(sum - k)) {
            count += prefixSumCount.get(sum - k);
        }
        
        // Update prefix sum count
        prefixSumCount.put(sum, prefixSumCount.getOrDefault(sum, 0) + 1);
    }
    
    return count;
}
```

### 5. First Non-Repeating Character

```java
// Find first non-repeating character - O(n) time
public int firstUniqChar(String s) {
    Map<Character, Integer> freq = new HashMap<>();
    
    // Count frequencies
    for (char c : s.toCharArray()) {
        freq.put(c, freq.getOrDefault(c, 0) + 1);
    }
    
    // Find first with frequency 1
    for (int i = 0; i < s.length(); i++) {
        if (freq.get(s.charAt(i)) == 1) {
            return i;
        }
    }
    
    return -1;
}
```

### 6. LRU Cache (Covered in Stack/Queue section)

### 7. Design HashMap

```java
// Design custom HashMap - covered in previous section
```

### 8. Intersection of Two Arrays

```java
// Find intersection - O(m + n) time
public int[] intersection(int[] nums1, int[] nums2) {
    Set<Integer> set1 = new HashSet<>();
    for (int num : nums1) {
        set1.add(num);
    }
    
    Set<Integer> result = new HashSet<>();
    for (int num : nums2) {
        if (set1.contains(num)) {
            result.add(num);
        }
    }
    
    return result.stream().mapToInt(Integer::intValue).toArray();
}
```

---

## Key Takeaways

1. **HashMap**: O(1) average for insert/delete/search
2. **HashSet**: For unique elements and set operations
3. **Collision Resolution**: Chaining (Java) or open addressing
4. **Load Factor**: Rehash when load factor exceeds threshold
5. **Hash Function**: Critical for performance

**Interview Tip**: Use HashMap/HashSet for counting, grouping, and fast lookups!

