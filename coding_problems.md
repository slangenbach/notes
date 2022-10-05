# Solutions to Coding Problems

## Arrays

### Two Number Sum

* Create an empty hash table
* Loop over the array
* For each number in the array, calculate the target to arrive at the target sum, i.e. target = target_sum - num
* Check wether the target is contained in the hash table
* If so, return the current number and the target - if not, add the current number to the hash_table
* If the loop finishes without returning, return an empty array
* This solution has time and space complexity of O(N), as we are only looping over the array once, and since the maximum size of the hash table is determined by the number of elements in the array