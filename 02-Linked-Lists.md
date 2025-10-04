# Linked Lists

## 📖 Table of Contents
- [Introduction](#introduction)
- [Types of Linked Lists](#types-of-linked-lists)
- [Singly Linked List](#singly-linked-list)
- [Doubly Linked List](#doubly-linked-list)
- [Circular Linked List](#circular-linked-list)
- [Common Operations](#common-operations)
- [Important Problems](#important-problems)

---

## Introduction

### What is a Linked List?
A Linked List is a linear data structure where elements (nodes) are not stored in contiguous memory locations. Each node contains data and a reference (pointer) to the next node.

**Advantages over Arrays:**
- Dynamic size (can grow/shrink)
- Easy insertion/deletion at any position (O(1) if position known)
- No memory wastage

**Disadvantages:**
- No random access (O(n) to access element)
- Extra memory for storing references
- Not cache-friendly

---

## Types of Linked Lists

1. **Singly Linked List**: Each node points to the next node
2. **Doubly Linked List**: Each node points to both next and previous nodes
3. **Circular Linked List**: Last node points back to the first node

---

## Singly Linked List

### Node Structure

```java
// Node class for Singly Linked List
class Node {
    int data;        // Data stored in node
    Node next;       // Reference to next node
    
    // Constructor
    public Node(int data) {
        this.data = data;
        this.next = null;
    }
}
```

### Singly Linked List Implementation

```java
public class SinglyLinkedList {
    private Node head;  // Head of the list
    
    // Constructor
    public SinglyLinkedList() {
        this.head = null;
    }
    
    // 1. Insert at beginning - O(1) time
    public void insertAtBeginning(int data) {
        Node newNode = new Node(data);
        newNode.next = head;  // Point new node to current head
        head = newNode;       // Update head
    }
    
    // 2. Insert at end - O(n) time
    public void insertAtEnd(int data) {
        Node newNode = new Node(data);
        
        // If list is empty
        if (head == null) {
            head = newNode;
            return;
        }
        
        // Traverse to last node
        Node current = head;
        while (current.next != null) {
            current = current.next;
        }
        
        // Link last node to new node
        current.next = newNode;
    }
    
    // 3. Insert at specific position - O(n) time
    public void insertAtPosition(int data, int position) {
        // If position is 0, insert at beginning
        if (position == 0) {
            insertAtBeginning(data);
            return;
        }
        
        Node newNode = new Node(data);
        Node current = head;
        
        // Traverse to position-1
        for (int i = 0; i < position - 1 && current != null; i++) {
            current = current.next;
        }
        
        // If position is invalid
        if (current == null) {
            System.out.println("Invalid position");
            return;
        }
        
        // Insert node
        newNode.next = current.next;
        current.next = newNode;
    }
    
    // 4. Delete from beginning - O(1) time
    public void deleteFromBeginning() {
        if (head == null) {
            System.out.println("List is empty");
            return;
        }
        head = head.next;  // Move head to next node
    }
    
    // 5. Delete from end - O(n) time
    public void deleteFromEnd() {
        if (head == null) {
            System.out.println("List is empty");
            return;
        }
        
        // If only one node
        if (head.next == null) {
            head = null;
            return;
        }
        
        // Traverse to second last node
        Node current = head;
        while (current.next.next != null) {
            current = current.next;
        }
        
        // Remove last node
        current.next = null;
    }
    
    // 6. Delete specific node - O(n) time
    public void deleteNode(int data) {
        if (head == null) return;
        
        // If head needs to be deleted
        if (head.data == data) {
            head = head.next;
            return;
        }
        
        // Find node to delete
        Node current = head;
        while (current.next != null && current.next.data != data) {
            current = current.next;
        }
        
        // If node not found
        if (current.next == null) {
            System.out.println("Node not found");
            return;
        }
        
        // Delete node
        current.next = current.next.next;
    }
    
    // 7. Search for element - O(n) time
    public boolean search(int data) {
        Node current = head;
        while (current != null) {
            if (current.data == data) {
                return true;
            }
            current = current.next;
        }
        return false;
    }
    
    // 8. Get length of list - O(n) time
    public int getLength() {
        int count = 0;
        Node current = head;
        while (current != null) {
            count++;
            current = current.next;
        }
        return count;
    }
    
    // 9. Display list - O(n) time
    public void display() {
        if (head == null) {
            System.out.println("List is empty");
            return;
        }
        
        Node current = head;
        while (current != null) {
            System.out.print(current.data + " -> ");
            current = current.next;
        }
        System.out.println("null");
    }
    
    // 10. Reverse the list - O(n) time, O(1) space
    public void reverse() {
        Node prev = null;
        Node current = head;
        Node next = null;
        
        while (current != null) {
            next = current.next;    // Store next node
            current.next = prev;    // Reverse link
            prev = current;         // Move prev forward
            current = next;         // Move current forward
        }
        
        head = prev;  // Update head
    }
}
```

---

## Doubly Linked List

### Node Structure

```java
// Node class for Doubly Linked List
class DNode {
    int data;
    DNode prev;  // Reference to previous node
    DNode next;  // Reference to next node
    
    public DNode(int data) {
        this.data = data;
        this.prev = null;
        this.next = null;
    }
}
```

### Doubly Linked List Implementation

```java
public class DoublyLinkedList {
    private DNode head;  // Head of the list
    private DNode tail;  // Tail of the list (optional but useful)
    
    public DoublyLinkedList() {
        this.head = null;
        this.tail = null;
    }
    
    // 1. Insert at beginning - O(1) time
    public void insertAtBeginning(int data) {
        DNode newNode = new DNode(data);
        
        if (head == null) {
            head = tail = newNode;
            return;
        }
        
        newNode.next = head;
        head.prev = newNode;
        head = newNode;
    }
    
    // 2. Insert at end - O(1) time (with tail pointer)
    public void insertAtEnd(int data) {
        DNode newNode = new DNode(data);
        
        if (tail == null) {
            head = tail = newNode;
            return;
        }
        
        tail.next = newNode;
        newNode.prev = tail;
        tail = newNode;
    }
    
    // 3. Delete from beginning - O(1) time
    public void deleteFromBeginning() {
        if (head == null) {
            System.out.println("List is empty");
            return;
        }
        
        if (head == tail) {
            head = tail = null;
            return;
        }
        
        head = head.next;
        head.prev = null;
    }
    
    // 4. Delete from end - O(1) time (with tail pointer)
    public void deleteFromEnd() {
        if (tail == null) {
            System.out.println("List is empty");
            return;
        }
        
        if (head == tail) {
            head = tail = null;
            return;
        }
        
        tail = tail.prev;
        tail.next = null;
    }
    
    // 5. Display forward - O(n) time
    public void displayForward() {
        DNode current = head;
        while (current != null) {
            System.out.print(current.data + " <-> ");
            current = current.next;
        }
        System.out.println("null");
    }
    
    // 6. Display backward - O(n) time
    public void displayBackward() {
        DNode current = tail;
        while (current != null) {
            System.out.print(current.data + " <-> ");
            current = current.prev;
        }
        System.out.println("null");
    }
}
```

---

## Circular Linked List

### Implementation

```java
public class CircularLinkedList {
    private Node tail;  // Using tail instead of head for easier operations
    
    // Insert at beginning - O(1) time
    public void insertAtBeginning(int data) {
        Node newNode = new Node(data);
        
        if (tail == null) {
            tail = newNode;
            tail.next = tail;  // Point to itself
            return;
        }
        
        newNode.next = tail.next;  // Point to head
        tail.next = newNode;       // Update tail's next
    }
    
    // Insert at end - O(1) time
    public void insertAtEnd(int data) {
        Node newNode = new Node(data);
        
        if (tail == null) {
            tail = newNode;
            tail.next = tail;
            return;
        }
        
        newNode.next = tail.next;  // Point to head
        tail.next = newNode;       // Update tail's next
        tail = newNode;            // Update tail
    }
    
    // Display - O(n) time
    public void display() {
        if (tail == null) {
            System.out.println("List is empty");
            return;
        }
        
        Node current = tail.next;  // Start from head
        do {
            System.out.print(current.data + " -> ");
            current = current.next;
        } while (current != tail.next);
        System.out.println("(back to start)");
    }
}
```

---

## Common Operations

### 1. Find Middle Element

```java
// Find middle element using slow and fast pointers - O(n) time
public Node findMiddle(Node head) {
    if (head == null) return null;
    
    Node slow = head;
    Node fast = head;
    
    // Fast moves 2 steps, slow moves 1 step
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    return slow;  // Slow will be at middle
}
```

### 2. Detect Cycle (Floyd's Algorithm)

```java
// Detect if linked list has a cycle - O(n) time, O(1) space
public boolean hasCycle(Node head) {
    if (head == null) return false;
    
    Node slow = head;
    Node fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        
        if (slow == fast) {
            return true;  // Cycle detected
        }
    }
    
    return false;  // No cycle
}

// Find the start of the cycle
public Node detectCycleStart(Node head) {
    if (head == null) return null;
    
    Node slow = head;
    Node fast = head;
    
    // Detect cycle
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        
        if (slow == fast) {
            // Cycle found, find start
            slow = head;
            while (slow != fast) {
                slow = slow.next;
                fast = fast.next;
            }
            return slow;  // Start of cycle
        }
    }
    
    return null;  // No cycle
}
```

### 3. Reverse Linked List

```java
// Iterative approach - O(n) time, O(1) space
public Node reverseIterative(Node head) {
    Node prev = null;
    Node current = head;
    Node next = null;
    
    while (current != null) {
        next = current.next;     // Save next
        current.next = prev;     // Reverse link
        prev = current;          // Move prev
        current = next;          // Move current
    }
    
    return prev;  // New head
}

// Recursive approach - O(n) time, O(n) space (call stack)
public Node reverseRecursive(Node head) {
    // Base case
    if (head == null || head.next == null) {
        return head;
    }
    
    // Recursive call
    Node newHead = reverseRecursive(head.next);
    
    // Reverse the link
    head.next.next = head;
    head.next = null;
    
    return newHead;
}
```

### 4. Merge Two Sorted Lists

```java
// Merge two sorted linked lists - O(m+n) time
public Node mergeTwoLists(Node l1, Node l2) {
    // Dummy node to simplify logic
    Node dummy = new Node(0);
    Node current = dummy;
    
    // Compare and merge
    while (l1 != null && l2 != null) {
        if (l1.data <= l2.data) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }
    
    // Attach remaining nodes
    if (l1 != null) {
        current.next = l1;
    }
    if (l2 != null) {
        current.next = l2;
    }
    
    return dummy.next;
}
```

### 5. Remove Nth Node From End

```java
// Remove nth node from end - O(n) time, O(1) space
public Node removeNthFromEnd(Node head, int n) {
    Node dummy = new Node(0);
    dummy.next = head;
    
    Node first = dummy;
    Node second = dummy;
    
    // Move first n+1 steps ahead
    for (int i = 0; i <= n; i++) {
        first = first.next;
    }
    
    // Move both until first reaches end
    while (first != null) {
        first = first.next;
        second = second.next;
    }
    
    // Remove nth node
    second.next = second.next.next;
    
    return dummy.next;
}
```

### 6. Check if Palindrome

```java
// Check if linked list is palindrome - O(n) time, O(1) space
public boolean isPalindrome(Node head) {
    if (head == null || head.next == null) return true;
    
    // Find middle
    Node slow = head;
    Node fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Reverse second half
    Node secondHalf = reverseIterative(slow.next);
    
    // Compare first and second half
    Node firstHalf = head;
    while (secondHalf != null) {
        if (firstHalf.data != secondHalf.data) {
            return false;
        }
        firstHalf = firstHalf.next;
        secondHalf = secondHalf.next;
    }
    
    return true;
}
```

---

## Important Problems

### 1. Intersection of Two Linked Lists

```java
// Find intersection point of two linked lists - O(m+n) time
public Node getIntersectionNode(Node headA, Node headB) {
    if (headA == null || headB == null) return null;
    
    Node a = headA;
    Node b = headB;
    
    // Traverse both lists
    // When reaching end, switch to other list's head
    while (a != b) {
        a = (a == null) ? headB : a.next;
        b = (b == null) ? headA : b.next;
    }
    
    return a;  // Intersection point or null
}
```

### 2. Add Two Numbers (Linked Lists)

```java
// Add two numbers represented as linked lists - O(max(m,n)) time
public Node addTwoNumbers(Node l1, Node l2) {
    Node dummy = new Node(0);
    Node current = dummy;
    int carry = 0;
    
    while (l1 != null || l2 != null || carry != 0) {
        int sum = carry;
        
        if (l1 != null) {
            sum += l1.data;
            l1 = l1.next;
        }
        
        if (l2 != null) {
            sum += l2.data;
            l2 = l2.next;
        }
        
        carry = sum / 10;
        current.next = new Node(sum % 10);
        current = current.next;
    }
    
    return dummy.next;
}
```

### 3. Copy List with Random Pointer

```java
// Node with random pointer
class RandomNode {
    int data;
    RandomNode next;
    RandomNode random;
    
    public RandomNode(int data) {
        this.data = data;
    }
}

// Deep copy list with random pointer - O(n) time, O(n) space
public RandomNode copyRandomList(RandomNode head) {
    if (head == null) return null;
    
    // Map original node to copied node
    Map<RandomNode, RandomNode> map = new HashMap<>();
    
    // First pass: Create all nodes
    RandomNode current = head;
    while (current != null) {
        map.put(current, new RandomNode(current.data));
        current = current.next;
    }
    
    // Second pass: Set next and random pointers
    current = head;
    while (current != null) {
        map.get(current).next = map.get(current.next);
        map.get(current).random = map.get(current.random);
        current = current.next;
    }
    
    return map.get(head);
}
```

### 4. Flatten a Multilevel Doubly Linked List

```java
// Node with child pointer
class MultiNode {
    int data;
    MultiNode prev;
    MultiNode next;
    MultiNode child;
}

// Flatten multilevel list - O(n) time
public MultiNode flatten(MultiNode head) {
    if (head == null) return null;
    
    MultiNode current = head;
    
    while (current != null) {
        // If no child, continue
        if (current.child == null) {
            current = current.next;
            continue;
        }
        
        // Find tail of child list
        MultiNode child = current.child;
        while (child.next != null) {
            child = child.next;
        }
        
        // Connect child list
        child.next = current.next;
        if (current.next != null) {
            current.next.prev = child;
        }
        
        current.next = current.child;
        current.child.prev = current;
        current.child = null;
    }
    
    return head;
}
```

### 5. Sort Linked List (Merge Sort)

```java
// Sort linked list using merge sort - O(n log n) time
public Node sortList(Node head) {
    // Base case
    if (head == null || head.next == null) {
        return head;
    }
    
    // Find middle
    Node mid = findMiddle(head);
    Node left = head;
    Node right = mid.next;
    mid.next = null;  // Split list
    
    // Recursively sort both halves
    left = sortList(left);
    right = sortList(right);
    
    // Merge sorted halves
    return mergeTwoLists(left, right);
}

private Node findMiddle(Node head) {
    Node slow = head;
    Node fast = head.next;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    return slow;
}
```

### 6. Reorder List (L0→Ln→L1→Ln-1→L2→Ln-2→...)

```java
// Reorder list - O(n) time, O(1) space
public void reorderList(Node head) {
    if (head == null || head.next == null) return;
    
    // Step 1: Find middle
    Node slow = head;
    Node fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Step 2: Reverse second half
    Node secondHalf = reverseIterative(slow.next);
    slow.next = null;
    
    // Step 3: Merge two halves
    Node firstHalf = head;
    while (secondHalf != null) {
        Node temp1 = firstHalf.next;
        Node temp2 = secondHalf.next;
        
        firstHalf.next = secondHalf;
        secondHalf.next = temp1;
        
        firstHalf = temp1;
        secondHalf = temp2;
    }
}
```

### 7. Rotate List by K

```java
// Rotate list to right by k places - O(n) time
public Node rotateRight(Node head, int k) {
    if (head == null || head.next == null || k == 0) return head;
    
    // Find length and last node
    Node last = head;
    int length = 1;
    while (last.next != null) {
        last = last.next;
        length++;
    }
    
    // Make it circular
    last.next = head;
    
    // Find new tail (length - k % length - 1)
    k = k % length;
    int stepsToNewTail = length - k;
    Node newTail = head;
    for (int i = 1; i < stepsToNewTail; i++) {
        newTail = newTail.next;
    }
    
    // Break circle and set new head
    Node newHead = newTail.next;
    newTail.next = null;
    
    return newHead;
}
```

---

## Key Takeaways

1. **Use Dummy Node**: Simplifies edge cases in many problems
2. **Two Pointers**: Fast & slow for middle, cycle detection
3. **Recursion**: Natural fit for linked list problems
4. **Reverse**: Master iterative and recursive reversal
5. **Space Complexity**: Can often solve with O(1) extra space

**Practice Tip**: Draw diagrams while solving linked list problems!

