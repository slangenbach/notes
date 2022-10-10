# Data Structures

## General

* What data structures best allows me to solve the problem (with the best space-time complexity)

## Complexity Analysis

* Time complexity measures how fast an algorithm runs
* Space complexity measures how much memory an algorithm takes up

## Memory

* Any piece of data can be transformed into binary numbers
* Data is stored in blocks of bytes (8 bits) within a bounded array of memory slots
* Memory slots can be accessed via memory addresses
* If data to be stored takes up more than 1 byte, it is stored in several adjacent blocks of memory 
* Integers are usually fixed-width, meaning that despite their value they take up the same memory
* Strings can be mapped to numbers via ASCII code
* One can also store references (pointers) to other memory slots within a slot

## Big O Notation

* Big O describes the time and space complexity of algorithms in terms of elementary operations
* In the context of coding interviews, Big O refers to performance in the _worst-case-scenario_
* Common complexities include:
    - Constant: O(1)
    - Logarithmic: O(log(n))
    - Linear: O(n)
    - Log-linear: O(n * log(n))
    - Quadratic: O(n**2)
    - Cubic: O(n**3)
    - Exponential: O(2**n)
    - Factorial: O(n!)
* Search for _complexity graph_ to get a visualization of the aforementioned complexities

## Logarithm

* log(n) = y iff (if and only if) b**y = n
* In the context of coding interviews we use the binary logarithm and assume b is equal to 2
* When the input (n) to an algorithm with time complexity of O(log(n)) doubles, the number of operations to complete the algorithm (y) only increases by 1
* Conversely, time complexity of O(log(n)) is pretty damn good

## Arrays

* Depending on the programming language, arrays may be static or dynamic 
* _Static_ arrays allocate a fixed amount of memory to store values upon initialization, while _dynamic_ arrays preemptively allocatee double the amount of memory to store values
* _Appending_ values to _static_ arrays therefore necessitates copying the entire array and allocations new memory for it (linear time operation). Using _dynamic_ arrays, appending values is a constant time operation until the allocated memory is filled up. Then the array is copied and double the memory is once again allocated to store (new) values.
* Arrays time and space complexities for standard operations are:
    - Access at given index: O(1)
    - Update at given index: O(1)
    - Insert at beginning: O(1)
    - Insert in the middle: O(n)
    - Insert at the end: O(1) for dynamic arrays (amortized), O(n) for static arrays
    - Removing at beginning: O(n)
    - Removing in the middle: O(n)
    - Removing at the end: O(1)
    - Copying: O(n)

## Linked Lists

* Linked lists exist as _singly_ and _doubly_ linked lists - _circular_ linked lists (not having a clear head or tail) also exist but are implemented as singly or doubly versions
* Singly linked lists consist of nodes, where each node contains some value and a pointer to the next node in the list
* The first node is called _head_, the last node _tail_
* The pointer of the last node points to null
* A singly linked lists usually only exposes the _head_ node to users
* _Singly_ linked list time and space complexities are:
    - Access head: O(1)
    - Access tail: O(n)
    - Access any other node: O(n)
    - Insert/remove head: O(1)
    - Insert/remove tail: O(n)
    - Traverse: O(n)
* Doubly linked lists also keep track of the _previous_ node by storing a pointer to it with each node
* Consequently the prev pointer of the head node points to null
* A doubly linked lists exposes both the _head_ and _tail_ node to users
* _Doubly_ linked list time and space complexities are:
    - Access head: O(1)
    - Access tail: O(1)
    - Access any other node: O(n)
    - Insert/remove head: O(1)
    - Insert/remove tail: O(1)
    - Insert/remove any other node: O(n)
    - Traverse: O(n)