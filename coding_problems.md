# Solutions to Coding Problems

## General

* If an input is sorted or in a specific order this may mean that you can solve the problem in linear time
* When dealing with an _array_ problem, sort it and see if any patterns emerge
* Always ask if you are allowed to do in-place operations to given inputs
* When dealing with _strings_, be mindful of the time complexity when building up strings one by one

## Arrays

### Two Number Sum

* Create an empty hash table
* Loop over the array
* For each number in the array, calculate the target to arrive at the target sum, i.e. target = target_sum - num
* Check wether the target is contained in the hash table
* If so, return the current number and the target - if not, add the current number to the hash_table
* If the loop finishes without returning, return an empty array
* This solution has time and space complexity of O(n), as we are only looping over the array once, and since the maximum size of the hash table is determined by the number of elements in the array

### Validate Subsequence

* Declare a pointer for the sequence index and set it to 0
* Loop over the array
* If the value of sequence index pointer is equal to length of the sequence, stop - we found the entire sequence within the array
* If not, check if the current value of the array is equal to the value of sequence at the current sequence index
* If so, increment the sequence index
* Finally, check wether the sequence index is equal to the length of the sequence and return
* This solution has time complexity of O(n) and space complexity of O(1)

### Sorted Squared Array

#### Brute Force Solution 

* The brute force approach is simply squaring all elements and then sorting the array with an internal function
* Note that the brute force solution will work also with an _unsorted_ input array 
* The brute force solution has time complexity of O(n* log(n)) because of sorting with an internal function, and space complexity of O(n) as N squares of the input array will be inserted in a new output array

#### Optimal Solution

* Declare a new array of 0s with the same length as the input array - we need this pre-filled array to insert squared values at the correct position
* Declare a pointer for the smaller value index and set it to 0
* Declare a pointer for the larger value index and set it to length of the input array - 1
* Loop over the index of the input array in reversed order - using reversed(range(len(array)))
* Get the values of the input array at the smaller and larger value indexes
* Compare the absolute values with each other:
    - If smaller value > larger value, insert the square of smaller value at the current index
    - Increase the smaller value index by 1
    - Else, insert the square of the larger value at the current index
    - Decrease the larger value index by 1
* Return the new array containing the sorted squares of the input array
* The optimal solution has time and space complexity of O(n)

### Tournament Winner

* Declare an empty string tracking the best team
* Declare a hash table tracking teams and scores (use defaultdict(int) to handle missing keys)
* Loop over index and values of the competitions array by enumerating it
* Determine the winning team
* Record the scores for the winning team in the hash table
* Check if the score of the winning team is bigger than the score of the best team - if so, make the winning team the new best team
* Return the name of the best team
* The solution has time complexity of O(n) and space complexity of O(k) - where k is the number of teams in the competitions array

### Non-constructible Change

* The key to solving this problem is realizing that we can not make change if the value of a coin is greater than the value of the change we can currently make + 1
* Declare a variable tracking change and set it to 0
* Sort the input array
* Loop over the sorted array of coins:
    - For each coin check if its value is > current change + 1
    - If so, return current change + 1, as we are not able to make it
    - If not, increment current change by the coin's value
* Finally, return current change + 1
* The solution has time complexity of O(n*log(n)) (because of sorting the input array) and space complexity of O(1)

## Strings

### Palindrome check

* This problem can be solved in _several_ ways. 
* One option is to compare the input string to its reverse, another is to to loop over each char of the string from start and end simultaneously and compare them
* Declare two integers pointing to the start and end of the string
* Loop over the string while start pointer is smaller than end pointer
* Check if char at start pointer is not equal to char at end pointer
    - If so, return False
    - If not, increment start pointer by 1 and decrement end pointer by
* Finally, return True
* You may add a check condition which returns True if the string is of length 1

### Caesar Cipher Encryptor

* Solutions to this problem can be developed using built-in functions for unicode conversion and mapping letters in the alphabet to its positions (via a simple array or two hash maps)
* This solution has time complexity of O(n) and space complexity of O(n) - O(m) if the alphabet is really large only

#### Unicode conversion

* Define a shift function taking char and key as input
* Derive the key for conversion by converting char to its unicode value (via ord) and adding the value of key % 26 (26 being the length of the alphabet)
* Modulo by 26 is done to handle edge cases for very large keys
* Check if the new key is smaller or equal than 122 (value of "z" in unicode)
    - If so, return the char at the new value (via chr)
    - If not, return the char at 96 + new value % 122 (where 96 is unicode value of "a" and modulo by 122 is necessary to wrap around "z")
* Finally apply the shift function to each letter in string and join it to an empty string

#### Mapping letters to positions in the alphabet

* Create an array containing the letters of the alphabet (via string.ascii_lowercase)
* Declare a variable storing the length of the alphabet
* Declare an empty array to build the new string
* Loop over each letter in the string
* Derive the new key by getting the index of the current char in the alphabet (via index) + key % length of the alphabet (handling edge cases as mentioned previously)
* If the new key is smaller then the length of the alphabet, get the new letter by indexing the alphabet array at the new key position
* Else get the new letter by indexing the array at position key % alphabet length
* Add the new letter to the string array
* Finally join the new string array to an empty string
