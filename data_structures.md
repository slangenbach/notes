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