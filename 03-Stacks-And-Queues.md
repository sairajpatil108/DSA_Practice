# Stacks and Queues

## 📖 Table of Contents
- [Stack](#stack)
- [Queue](#queue)
- [Deque (Double-Ended Queue)](#deque)
- [Priority Queue](#priority-queue)
- [Monotonic Stack/Queue](#monotonic-stack-queue)
- [Important Problems](#important-problems)

---

## Stack

### What is a Stack?
A Stack is a linear data structure that follows **LIFO** (Last In, First Out) principle. The last element added is the first one to be removed.

**Real-world Examples:** Stack of plates, Browser history, Undo functionality

**Key Operations:**
- `push()`: Add element to top - O(1)
- `pop()`: Remove element from top - O(1)
- `peek()/top()`: Get top element - O(1)
- `isEmpty()`: Check if empty - O(1)

### Stack Implementation Using Array

```java
public class StackUsingArray {
    private int[] arr;
    private int top;      // Index of top element
    private int capacity; // Maximum size
    
    // Constructor
    public StackUsingArray(int size) {
        arr = new int[size];
        capacity = size;
        top = -1;  // Empty stack
    }
    
    // Push element to stack - O(1)
    public void push(int data) {
        if (isFull()) {
            System.out.println("Stack Overflow");
            return;
        }
        arr[++top] = data;
    }
    
    // Pop element from stack - O(1)
    public int pop() {
        if (isEmpty()) {
            System.out.println("Stack Underflow");
            return -1;
        }
        return arr[top--];
    }
    
    // Peek top element - O(1)
    public int peek() {
        if (isEmpty()) {
            System.out.println("Stack is empty");
            return -1;
        }
        return arr[top];
    }
    
    // Check if stack is empty
    public boolean isEmpty() {
        return top == -1;
    }
    
    // Check if stack is full
    public boolean isFull() {
        return top == capacity - 1;
    }
    
    // Get size of stack
    public int size() {
        return top + 1;
    }
    
    // Display stack
    public void display() {
        if (isEmpty()) {
            System.out.println("Stack is empty");
            return;
        }
        System.out.print("Stack: ");
        for (int i = 0; i <= top; i++) {
            System.out.print(arr[i] + " ");
        }
        System.out.println();
    }
}
```

### Stack Implementation Using Linked List

```java
class StackNode {
    int data;
    StackNode next;
    
    public StackNode(int data) {
        this.data = data;
        this.next = null;
    }
}

public class StackUsingLinkedList {
    private StackNode top;
    
    public StackUsingLinkedList() {
        this.top = null;
    }
    
    // Push element - O(1)
    public void push(int data) {
        StackNode newNode = new StackNode(data);
        newNode.next = top;
        top = newNode;
    }
    
    // Pop element - O(1)
    public int pop() {
        if (isEmpty()) {
            System.out.println("Stack Underflow");
            return -1;
        }
        int data = top.data;
        top = top.next;
        return data;
    }
    
    // Peek element - O(1)
    public int peek() {
        if (isEmpty()) {
            System.out.println("Stack is empty");
            return -1;
        }
        return top.data;
    }
    
    // Check if empty
    public boolean isEmpty() {
        return top == null;
    }
}
```

### Using Java's Built-in Stack

```java
import java.util.Stack;

public class StackExample {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();
        
        // Push elements
        stack.push(10);
        stack.push(20);
        stack.push(30);
        
        // Peek top element
        System.out.println("Top: " + stack.peek());  // 30
        
        // Pop element
        System.out.println("Popped: " + stack.pop());  // 30
        
        // Check if empty
        System.out.println("Is empty: " + stack.isEmpty());  // false
        
        // Get size
        System.out.println("Size: " + stack.size());  // 2
        
        // Search element (returns position from top, 1-indexed)
        System.out.println("Position of 10: " + stack.search(10));  // 2
    }
}
```

---

## Queue

### What is a Queue?
A Queue is a linear data structure that follows **FIFO** (First In, First Out) principle. The first element added is the first one to be removed.

**Real-world Examples:** Line at ticket counter, Print queue, CPU scheduling

**Key Operations:**
- `enqueue()/offer()`: Add element to rear - O(1)
- `dequeue()/poll()`: Remove element from front - O(1)
- `peek()/front()`: Get front element - O(1)
- `isEmpty()`: Check if empty - O(1)

### Queue Implementation Using Array (Circular Queue)

```java
public class CircularQueue {
    private int[] arr;
    private int front;
    private int rear;
    private int size;
    private int capacity;
    
    // Constructor
    public CircularQueue(int capacity) {
        this.capacity = capacity;
        arr = new int[capacity];
        front = 0;
        rear = -1;
        size = 0;
    }
    
    // Enqueue element - O(1)
    public void enqueue(int data) {
        if (isFull()) {
            System.out.println("Queue is full");
            return;
        }
        rear = (rear + 1) % capacity;  // Circular increment
        arr[rear] = data;
        size++;
    }
    
    // Dequeue element - O(1)
    public int dequeue() {
        if (isEmpty()) {
            System.out.println("Queue is empty");
            return -1;
        }
        int data = arr[front];
        front = (front + 1) % capacity;  // Circular increment
        size--;
        return data;
    }
    
    // Peek front element - O(1)
    public int peek() {
        if (isEmpty()) {
            System.out.println("Queue is empty");
            return -1;
        }
        return arr[front];
    }
    
    // Check if empty
    public boolean isEmpty() {
        return size == 0;
    }
    
    // Check if full
    public boolean isFull() {
        return size == capacity;
    }
    
    // Get size
    public int size() {
        return size;
    }
}
```

### Queue Implementation Using Linked List

```java
class QueueNode {
    int data;
    QueueNode next;
    
    public QueueNode(int data) {
        this.data = data;
        this.next = null;
    }
}

public class QueueUsingLinkedList {
    private QueueNode front;
    private QueueNode rear;
    private int size;
    
    public QueueUsingLinkedList() {
        front = rear = null;
        size = 0;
    }
    
    // Enqueue - O(1)
    public void enqueue(int data) {
        QueueNode newNode = new QueueNode(data);
        
        if (rear == null) {
            front = rear = newNode;
            size++;
            return;
        }
        
        rear.next = newNode;
        rear = newNode;
        size++;
    }
    
    // Dequeue - O(1)
    public int dequeue() {
        if (isEmpty()) {
            System.out.println("Queue is empty");
            return -1;
        }
        
        int data = front.data;
        front = front.next;
        
        if (front == null) {
            rear = null;
        }
        
        size--;
        return data;
    }
    
    // Peek - O(1)
    public int peek() {
        if (isEmpty()) {
            System.out.println("Queue is empty");
            return -1;
        }
        return front.data;
    }
    
    // Check if empty
    public boolean isEmpty() {
        return front == null;
    }
    
    // Get size
    public int size() {
        return size;
    }
}
```

### Using Java's Built-in Queue

```java
import java.util.Queue;
import java.util.LinkedList;

public class QueueExample {
    public static void main(String[] args) {
        Queue<Integer> queue = new LinkedList<>();
        
        // Enqueue elements
        queue.offer(10);
        queue.offer(20);
        queue.offer(30);
        
        // Peek front element
        System.out.println("Front: " + queue.peek());  // 10
        
        // Dequeue element
        System.out.println("Dequeued: " + queue.poll());  // 10
        
        // Check if empty
        System.out.println("Is empty: " + queue.isEmpty());  // false
        
        // Get size
        System.out.println("Size: " + queue.size());  // 2
    }
}
```

---

## Deque (Double-Ended Queue)

### What is a Deque?
A Deque allows insertion and deletion from both ends (front and rear).

```java
import java.util.Deque;
import java.util.ArrayDeque;

public class DequeExample {
    public static void main(String[] args) {
        Deque<Integer> deque = new ArrayDeque<>();
        
        // Add to front
        deque.addFirst(10);
        deque.addFirst(5);
        
        // Add to rear
        deque.addLast(20);
        deque.addLast(25);
        
        // Deque: 5 <-> 10 <-> 20 <-> 25
        
        // Remove from front
        System.out.println(deque.removeFirst());  // 5
        
        // Remove from rear
        System.out.println(deque.removeLast());   // 25
        
        // Peek operations
        System.out.println("First: " + deque.peekFirst());  // 10
        System.out.println("Last: " + deque.peekLast());    // 20
    }
}
```

---

## Priority Queue

### What is a Priority Queue?
Elements are dequeued based on priority (not FIFO). In Java, it's implemented using a Min Heap by default.

```java
import java.util.PriorityQueue;
import java.util.Collections;
import java.util.Comparator;

public class PriorityQueueExample {
    public static void main(String[] args) {
        // Min Heap (default)
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        minHeap.offer(30);
        minHeap.offer(10);
        minHeap.offer(20);
        System.out.println(minHeap.poll());  // 10 (smallest)
        
        // Max Heap (reverse order)
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        maxHeap.offer(30);
        maxHeap.offer(10);
        maxHeap.offer(20);
        System.out.println(maxHeap.poll());  // 30 (largest)
        
        // Custom comparator
        PriorityQueue<String> pq = new PriorityQueue<>(
            (a, b) -> a.length() - b.length()  // Sort by length
        );
        pq.offer("apple");
        pq.offer("pie");
        pq.offer("banana");
        System.out.println(pq.poll());  // "pie" (shortest)
    }
}
```

---

## Monotonic Stack Queue

### Monotonic Stack
A stack where elements are always in increasing or decreasing order.

```java
// Monotonic increasing stack
public int[] nextGreaterElement(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    Stack<Integer> stack = new Stack<>();  // Store indices
    
    // Traverse from right to left
    for (int i = n - 1; i >= 0; i--) {
        // Pop smaller elements
        while (!stack.isEmpty() && nums[stack.peek()] <= nums[i]) {
            stack.pop();
        }
        
        // Next greater element
        result[i] = stack.isEmpty() ? -1 : nums[stack.peek()];
        
        // Push current index
        stack.push(i);
    }
    
    return result;
}
```

---

## Important Problems

### 1. Valid Parentheses

```java
// Check if parentheses are valid - O(n) time, O(n) space
public boolean isValid(String s) {
    Stack<Character> stack = new Stack<>();
    
    for (char c : s.toCharArray()) {
        // Push opening brackets
        if (c == '(' || c == '{' || c == '[') {
            stack.push(c);
        } 
        // Check closing brackets
        else {
            if (stack.isEmpty()) return false;
            
            char top = stack.pop();
            if (c == ')' && top != '(') return false;
            if (c == '}' && top != '{') return false;
            if (c == ']' && top != '[') return false;
        }
    }
    
    return stack.isEmpty();  // All brackets matched
}
```

### 2. Implement Queue Using Stacks

```java
public class QueueUsingStacks {
    private Stack<Integer> stack1;  // For enqueue
    private Stack<Integer> stack2;  // For dequeue
    
    public QueueUsingStacks() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }
    
    // Enqueue - O(1)
    public void enqueue(int x) {
        stack1.push(x);
    }
    
    // Dequeue - Amortized O(1)
    public int dequeue() {
        if (stack2.isEmpty()) {
            // Transfer elements from stack1 to stack2
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        
        if (stack2.isEmpty()) {
            return -1;  // Queue is empty
        }
        
        return stack2.pop();
    }
    
    // Peek - Amortized O(1)
    public int peek() {
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        
        if (stack2.isEmpty()) {
            return -1;
        }
        
        return stack2.peek();
    }
}
```

### 3. Implement Stack Using Queues

```java
public class StackUsingQueues {
    private Queue<Integer> queue;
    
    public StackUsingQueues() {
        queue = new LinkedList<>();
    }
    
    // Push - O(n)
    public void push(int x) {
        queue.offer(x);
        
        // Rotate queue to make new element at front
        int size = queue.size();
        for (int i = 0; i < size - 1; i++) {
            queue.offer(queue.poll());
        }
    }
    
    // Pop - O(1)
    public int pop() {
        if (queue.isEmpty()) return -1;
        return queue.poll();
    }
    
    // Top - O(1)
    public int top() {
        if (queue.isEmpty()) return -1;
        return queue.peek();
    }
}
```

### 4. Min Stack

```java
// Stack that supports getMin() in O(1) time
public class MinStack {
    private Stack<Integer> stack;
    private Stack<Integer> minStack;  // Track minimum
    
    public MinStack() {
        stack = new Stack<>();
        minStack = new Stack<>();
    }
    
    // Push - O(1)
    public void push(int val) {
        stack.push(val);
        
        // Update min stack
        if (minStack.isEmpty() || val <= minStack.peek()) {
            minStack.push(val);
        }
    }
    
    // Pop - O(1)
    public void pop() {
        if (stack.isEmpty()) return;
        
        int val = stack.pop();
        
        // Update min stack
        if (val == minStack.peek()) {
            minStack.pop();
        }
    }
    
    // Top - O(1)
    public int top() {
        return stack.peek();
    }
    
    // Get minimum - O(1)
    public int getMin() {
        return minStack.peek();
    }
}
```

### 5. Largest Rectangle in Histogram

```java
// Find largest rectangle in histogram - O(n) time
public int largestRectangleArea(int[] heights) {
    Stack<Integer> stack = new Stack<>();  // Store indices
    int maxArea = 0;
    int n = heights.length;
    
    for (int i = 0; i <= n; i++) {
        int currentHeight = (i == n) ? 0 : heights[i];
        
        // Pop while current is smaller
        while (!stack.isEmpty() && currentHeight < heights[stack.peek()]) {
            int height = heights[stack.pop()];
            int width = stack.isEmpty() ? i : i - stack.peek() - 1;
            maxArea = Math.max(maxArea, height * width);
        }
        
        stack.push(i);
    }
    
    return maxArea;
}
```

### 6. Sliding Window Maximum

```java
// Find maximum in each sliding window - O(n) time
public int[] maxSlidingWindow(int[] nums, int k) {
    if (nums.length == 0) return new int[]{};
    
    int n = nums.length;
    int[] result = new int[n - k + 1];
    Deque<Integer> deque = new ArrayDeque<>();  // Store indices
    
    for (int i = 0; i < n; i++) {
        // Remove elements outside window
        while (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
            deque.pollFirst();
        }
        
        // Remove smaller elements
        while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
            deque.pollLast();
        }
        
        deque.offerLast(i);
        
        // Add to result
        if (i >= k - 1) {
            result[i - k + 1] = nums[deque.peekFirst()];
        }
    }
    
    return result;
}
```

### 7. Evaluate Reverse Polish Notation

```java
// Evaluate RPN expression - O(n) time
public int evalRPN(String[] tokens) {
    Stack<Integer> stack = new Stack<>();
    
    for (String token : tokens) {
        if (token.equals("+")) {
            int b = stack.pop();
            int a = stack.pop();
            stack.push(a + b);
        } else if (token.equals("-")) {
            int b = stack.pop();
            int a = stack.pop();
            stack.push(a - b);
        } else if (token.equals("*")) {
            int b = stack.pop();
            int a = stack.pop();
            stack.push(a * b);
        } else if (token.equals("/")) {
            int b = stack.pop();
            int a = stack.pop();
            stack.push(a / b);
        } else {
            stack.push(Integer.parseInt(token));
        }
    }
    
    return stack.pop();
}
```

### 8. Daily Temperatures

```java
// Find next warmer temperature - O(n) time
public int[] dailyTemperatures(int[] temperatures) {
    int n = temperatures.length;
    int[] result = new int[n];
    Stack<Integer> stack = new Stack<>();  // Store indices
    
    for (int i = 0; i < n; i++) {
        // Pop while current temp is higher
        while (!stack.isEmpty() && 
               temperatures[i] > temperatures[stack.peek()]) {
            int prevIndex = stack.pop();
            result[prevIndex] = i - prevIndex;
        }
        
        stack.push(i);
    }
    
    return result;
}
```

### 9. Basic Calculator

```java
// Evaluate expression with +, -, (, ) - O(n) time
public int calculate(String s) {
    Stack<Integer> stack = new Stack<>();
    int result = 0;
    int sign = 1;
    int num = 0;
    
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        
        if (Character.isDigit(c)) {
            num = num * 10 + (c - '0');
        } else if (c == '+') {
            result += sign * num;
            num = 0;
            sign = 1;
        } else if (c == '-') {
            result += sign * num;
            num = 0;
            sign = -1;
        } else if (c == '(') {
            // Push current result and sign
            stack.push(result);
            stack.push(sign);
            result = 0;
            sign = 1;
        } else if (c == ')') {
            result += sign * num;
            num = 0;
            result *= stack.pop();  // Pop sign
            result += stack.pop();  // Pop previous result
        }
    }
    
    result += sign * num;
    return result;
}
```

### 10. LRU Cache

```java
// Least Recently Used Cache - O(1) for get and put
class LRUCache {
    class Node {
        int key, value;
        Node prev, next;
        
        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }
    
    private Map<Integer, Node> map;
    private Node head, tail;
    private int capacity;
    
    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<>();
        
        // Dummy head and tail
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
    }
    
    public int get(int key) {
        if (!map.containsKey(key)) {
            return -1;
        }
        
        Node node = map.get(key);
        remove(node);
        addToHead(node);
        return node.value;
    }
    
    public void put(int key, int value) {
        if (map.containsKey(key)) {
            Node node = map.get(key);
            node.value = value;
            remove(node);
            addToHead(node);
        } else {
            if (map.size() == capacity) {
                Node toRemove = tail.prev;
                remove(toRemove);
                map.remove(toRemove.key);
            }
            
            Node newNode = new Node(key, value);
            addToHead(newNode);
            map.put(key, newNode);
        }
    }
    
    private void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }
    
    private void addToHead(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }
}
```

---

## Key Takeaways

1. **Stack**: LIFO, use for nested structures, backtracking
2. **Queue**: FIFO, use for BFS, level order traversal
3. **Deque**: Flexible, can act as both stack and queue
4. **Monotonic Stack**: Powerful for next greater/smaller problems
5. **Two Stacks/Queues**: Can implement one using the other

**Interview Tip**: Always consider stack/queue for problems involving ordering, matching, or processing in specific order!

