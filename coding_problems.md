# Solutions to Coding Problems

## General

* If an input is sorted or in a specific order this may mean that you can solve the problem in linear time

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

#### Brute force solution 

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