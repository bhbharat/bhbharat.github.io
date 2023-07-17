---
title: DSA
tags: 
keywords: rouge, pygments, prettify, color coding,
last_updated: Sept 5, 2022
summary: ""
sidebar: mydoc_sidebar
permalink: dsa.html
folder: mydoc
---


[video course](https://learn.udacity.com/courses/ud513/lessons/a7bd2ec6-dec4-4f3a-8282-9d452d0cb693/concepts/8101e3a3-86f2-4207-8e96-6eddea1de62e) |
[Book](file:///C:/Users/if441f/2022_Projects/Learning/Introduction_to_Algorithms_Third_Edition_(2009).pdf) |
[Edit here](https://github.com/bhbharat/bhbharat.github.io/edit/gh-pages/pages/mydoc/course-dsa.md)


# Data Structures

## Linked list

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def insert(self, data):
        new_node = Node(data)
        if self.head is None:
            self.head = new_node
        else:
            current_node = self.head
            while current_node.next is not None:
                current_node = current_node.next
            current_node.next = new_node

    def print_list(self):
        current_node = self.head
        while current_node is not None:
            print(current_node.data)
            current_node = current_node.next
            
my_list = LinkedList()
my_list.insert(1)
my_list.insert(2)
my_list.insert(3)
my_list.print_list()
```
## Stack data structure
A stack is a linear data structure that follows the Last-In-First-Out (LIFO) principle. In a stack, elements are added and removed from the same end, called the "top" of the stack.
```python
class Stack:
    def __init__(self):
        self.items = []

    def push(self, item):
        self.items.append(item)

    def pop(self):
        return self.items.pop()

    def peek(self):
        return self.items[-1]

    def is_empty(self):
        return len(self.items) == 0
```
## Queue
```python
class Queue:
    def __init__(self):
        self.items = []
    
    def is_empty(self):
        return len(self.items) == 0
    
    def enqueue(self, item):
        self.items.insert(0, item)
    
    def dequeue(self):
        if self.is_empty():
            return None
        return self.items.pop()
    
    def size(self):
        return len(self.items)
        
q = Queue()
q.enqueue(10)
q.enqueue(20)
q.enqueue(30)
print(q.size())  # Output: 3
```

## Hash table
A hash table is a data structure that allows for fast insertion, deletion, and lookup operations. It works by using a hash function to map keys to indices in an array. In Python, you can implement a hash table using a dictionary.
```python
# create an empty hash table
hash_table = {}

# add key-value pairs to the hash table
hash_table['apple'] = 1
hash_table['banana'] = 2
hash_table['cherry'] = 3

# access the value associated with a key
print(hash_table['apple'])  # output: 1

# update the value associated with a key
hash_table['banana'] = 4

# delete a key-value pair from the hash table
del hash_table['cherry']

# check if a key exists in the hash table
print('apple' in hash_table)  # output: True
print('cherry' in hash_table)  # output: False

```
## Trees
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.children = []

    def add_child(self, node):
        self.children.append(node)
        
root = Node(1)
child1 = Node(2)
child2 = Node(3)
child3 = Node(4)
root.add_child(child1)
root.add_child(child2)
child2.add_child(child3)
```

## Binary search tree
A binary search tree is a binary tree data structure in which each node has at most two children, left and right. Additionally, each node's left subtree contains only nodes with keys lesser than the node's key, while the right subtree contains only nodes with keys greater than the node's key.
```python
class Node:
    def __init__(self, key):
        self.left = None
        self.right = None
        self.val = key

class BST:
    def __init__(self):
        self.root = None

    def insert(self, root, key):
        if not root:
            return Node(key)
        else:
            if root.val == key:
                return root
            elif root.val < key:
                root.right = self.insert(root.right, key)
            else:
                root.left = self.insert(root.left, key)
        return root

    def inorder(self, root):
        if root:
            self.inorder(root.left)
            print(root.val)
            self.inorder(root.right)

# Create a new BST object
bst = BST()

# Insert values into the tree
root = bst.insert(bst.root, 50)
bst.insert(root, 30)
bst.insert(root, 20)
bst.insert(root, 40)
bst.insert(root, 70)
bst.insert(root, 60)
bst.insert(root, 80)

# Traverse the tree in inorder fashion
bst.inorder(root)
```

## Graph
A graph is a non-linear data structure consisting of a set of vertices (nodes) and a set of edges connecting these vertices. Graphs are used to represent relationships between objects, such as social networks, maps, and computer networks.
```python
class Graph:
    def __init__(self):
        self.adj_list = {}

    def add_vertex(self, vertex):
        if vertex not in self.adj_list:
            self.adj_list[vertex] = []

    def add_edge(self, vertex1, vertex2):
        if vertex1 in self.adj_list and vertex2 in self.adj_list:
            self.adj_list[vertex1].append(vertex2)
            self.adj_list[vertex2].append(vertex1)

    def remove_edge(self, vertex1, vertex2):
        if vertex1 in self.adj_list and vertex2 in self.adj_list:
            self.adj_list[vertex1].remove(vertex2)
            self.adj_list[vertex2].remove(vertex1)

    def remove_vertex(self, vertex):
        if vertex in self.adj_list:
            del self.adj_list[vertex]
            for adj_list in self.adj_list.values():
                if vertex in adj_list:
                    adj_list.remove(vertex)

    def get_vertices(self):
        return list(self.adj_list.keys())

    def get_edges(self):
        edges = []
        for vertex in self.adj_list:
            for neighbour in self.adj_list[vertex]:
                if {neighbour, vertex} not in edges:
                    edges.append({vertex, neighbour})
        return edges
        
graph = Graph()
graph.add_vertex('A')
graph.add_vertex('B')
graph.add_vertex('C')
graph.add_edge('A', 'B')
graph.add_edge('B', 'C')
print(graph.get_vertices()) # Output: ['A', 'B', 'C']
print(graph.get_edges()) # Output: [{'A', 'B'}, {'B', 'C'}]

```

# Algorithms

There are several basic algorithms that you can start with to build a foundation in data structures and algorithms. Here are some examples:

1.  Linear Search: This is a simple algorithm to search for an element in an array. It sequentially checks each element of the array until the target element is found.
    
2.  Binary Search: This is a more efficient search algorithm that works on sorted arrays. It divides the array into halves and checks whether the target element is in the left or right half of the array. This process is repeated until the target element is found.
    
3.  Bubble Sort: This is a basic sorting algorithm that repeatedly swaps adjacent elements if they are in the wrong order. This sorting technique is inefficient for large arrays but is easy to understand and implement.
    
4.  Selection Sort: This is another simple sorting algorithm that repeatedly finds the minimum element from the unsorted part of the array and places it at the beginning of the sorted part.
    
5.  Insertion Sort: This is a sorting algorithm that builds the final sorted array one element at a time. It iterates through the array, compares each element with the already sorted elements, and inserts it at the correct position in the sorted part of the array.
    
6.  Merge Sort: This is a divide-and-conquer sorting algorithm that works by dividing the array into two halves, sorting each half separately, and merging them back together.
    
7.  Quick Sort: This is also a divide-and-conquer sorting algorithm that works by partitioning the array into two sub-arrays around a pivot element, sorting the sub-arrays recursively, and then combining them.

## Linear Search
The time complexity of Binary Search algorithm is O(n)
```python
def linear_search(arr, x):
    """
    This function performs a linear search on the given array and returns the index
    of the element if it is present, else returns -1.
    """
    n = len(arr)
    for i in range(n):
        if arr[i] == x:
            return i
    return -1
```

## Binary Search
The time complexity of Binary Search algorithm is O(log n)
```python
def binary_search(arr, target):
    left = 0
    right = len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1

```

## Bubble sort
The time complexity of Bubble Sort is O(n^2)
```python
def bubble_sort(arr):
    n = len(arr)
    
    # Traverse through all array elements
    for i in range(n):
        
        # Last i elements are already in place
        for j in range(0, n-i-1):
            
            # Swap if the element found is greater than the next element
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]

# Test the code
arr = [64, 34, 25, 12, 22, 11, 90]
bubble_sort(arr)
print("Sorted array is:", arr)

```

## Selection sort
The time complexity of Selection Sort is O(n^2)
```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_index = i
        for j in range(i+1, n):
            if arr[j] < arr[min_index]:
                min_index = j
        arr[i], arr[min_index] = arr[min_index], arr[i]
    return arr
```
## Insertion sort
The time complexity of the insertion sort algorithm is O(n^2)
```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and key < arr[j]:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
    return arr
```
## Merge sort
The time complexity is O(n log n).
```python
def merge_sort(arr):
    if len(arr) > 1:
        mid = len(arr) // 2
        left_half = arr[:mid]
        right_half = arr[mid:]
        
        merge_sort(left_half)
        merge_sort(right_half)
        
        i = j = k = 0
        
        while i < len(left_half) and j < len(right_half):
            if left_half[i] < right_half[j]:
                arr[k] = left_half[i]
                i += 1
            else:
                arr[k] = right_half[j]
                j += 1
            k += 1
        
        while i < len(left_half):
            arr[k] = left_half[i]
            i += 1
            k += 1
            
        while j < len(right_half):
            arr[k] = right_half[j]
            j += 1
            k += 1

```
## Quick sort
The time complexity of Quick Sort is O(nlogn) in the average and best cases and O(n^2)
```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    else:
        pivot = arr[0]
        left = [x for x in arr[1:] if x < pivot]
        right = [x for x in arr[1:] if x >= pivot]
        return quick_sort(left) + [pivot] + quick_sort(right)
```

