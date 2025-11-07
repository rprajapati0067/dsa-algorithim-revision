# Basic Arrays Revision

## 1. Sum of Even-Indexed Elements in Range
Given an arr[N] and Q queries with start(s) and end(e) index. For every query print sum of all even indexed elements from s to e.

### Input
```
arr[] = [2, 3, 1, 6, 4, 5]
```

### Queries
```
s    e    sum
1    3    1
2    5    5   
0    4    7
3    3    0
```

## Approaches

### Brute Force
For every query you can iterate and add only the even index.

### Prefix Sum Approach
Create a prefix sum array by ignoring odd indexes (Consider odd index as 0).  
For example:
- Index 0 (even): Take 2
- Index 1 (odd): Consider as 0, so 0+2 = 2
- And so on...

Final prefix sum array: `psum[] = [2, 2, 3, 3, 7, 7]`

#### Pseudocode for Even Indices
```java
pf_even[n]
pf_even[0] = arr[0]
for(i = 1; i < n; i++) {
    if(i%2 == 0) {
        pf_even[i] = pf_even[i-1] + arr[i]
    } else {
        pf_even[i] = pf_even[i-1]
    }
}
```

#### Pseudocode for Odd Indices
```java
pf_odd[n]
pf_odd[0] = 0  // because 0 index is even so we take it as 0
for(i = 1; i < n; i++) {
    if(i%2 == 1) {
        pf_odd[i] = pf_odd[i-1] + arr[i]
    } else {
        pf_odd[i] = pf_odd[i-1]
    }
}
```

## 2. Count Special Indices
Given an arr[N], count the number of special indices in the array.  
**Special Index:** Index after removing which, sum of even indexed elements = sum of odd indexed elements.

### Example
```
Original: [4, 3, 2, 7, 6, -2]
Remove idx = 0 -> [3, 2, 7, 6, -2]
Check: odd and even indexed element sum
3+7-2 == 2+6 i.e 8 == 8 ✓
```
Similarly check for all indices. For this example, we have 2 special indices.

### Brute Force Approach
You can create a new array after eliminating each index i, then create the array and find the sum.  
**Time Complexity:** O(n²)

### Optimized Approach
Using prefix sum arrays for even and odd indices:
```
even idx before i -> pf_even[i-1]
odd idx before i  -> pf_odd[i-1]

even idx after i [i+1, n-1] -> pf_even[n-1]-pf_even[i]
odd idx after i             -> pf_odd[n-1] - pf_odd[i]

sum of even indexed elem after removing ith index:
    pf_even[i-1] + pf_odd[n-1] - pf_odd[i]
    
sum of odd indexed elem after removing ith index:
    pf_odd[i-1] + pf_even[n-1] - pf_even[i-1]
```

#### Pseudocode
```java
// 1. Calculate pf_even & pf_odd
ans = 0
for(i = 0; i < n; i++) {
    se = pf_even[i-1] + pf_odd[n-1] - pf_odd[i]
    so = pf_odd[i-1] + pf_even[n-1] - pf_even[i-1]

    if(se == so)
        ans++
}
return ans
```
**Time Complexity:** O(n)  
**Space Complexity:** O(n)


## 3. Count Pairs of 'a' and 'g'
Given a string s of lowercase chars, return the count of pairs(i, j) where i < j and s[i] is 'a' and s[j] is 'g'.

### Example
```
Input: st = [a, b, e, g, a, g]
Pairs: [0,3], [0,5], [4,5]
Answer: 3 pairs
```

### Brute Force Approach
#### Pseudocode
```java
count = 0
for(i = 0; i < n; i++) {
    for(j = i+1; j < n; j++) {
        if(s[i] == 'a' && s[j] == 'g') {
            count++
        }
    }
}
```

### Optimized Approach
**Key Observation:** A 'g' will form a pair with ALL 'a's on its left

#### Example Walkthrough
```
Index:    0 1 2 3 4 5 6 7
String:   b c a g g a a g
count_a:  0 0 1 1 1 2 3 3
ans:      0 0 0 1 2 2 2 5  -> 5 is final ans
```

#### Pseudocode
```java
count_a = 0, ans = 0
for(i = 0; i < n; i++) {
    if(s[i] == 'a') {
        count_a++
    } else if(s[i] == 'g') {
        ans += count_a
    }
}
return ans
```
**Time Complexity:** O(N)  
**Space Complexity:** O(1)

## 4. Print All Possible Subarrays
Given an array, print all possible subarrays.

### Example
```
Input: [5, 7, 3, 2]  
Output:
[5]
[5, 7]
[5, 7, 3]
[5, 7, 3, 2]
[7]
[7, 3]
[7, 3, 2]
[3]
[3, 2]
[2]
```

### Solution Approach
#### Pseudocode
```java
// Iterate over each subarray and print
for(start = 0; start < n; start++) {
    for(end = start; end < n; end++) {
        printSubarr(arr, start, end) // function to print subarray
    }
}
```
**Time Complexity:** O(N²)  
**Space Complexity:** O(1)
## 5. Smallest Subarray with Min and Max Elements
Given an array of N integers, return the length of smallest subarray which contains both max and min elements of the array.

### Problem Constraints
- 1 ≤ N ≤ 10⁶

### Example
```
Input: arr = [2, 2, 6, 4, 5, 1, 5, 2, 6, 4, 1]
max = 6 and min = 1
Output: 3 (subarray [6, 4, 1])
```

### Brute Force Approach
Check all subarrays  
**Time Complexity:** O(N³)

### Optimized Approach
#### Key Observations
1. There must be exactly one occurrence of min & max element -> `[min  min....max]`
2. Min and Max elements should be the end points of subarray
3. Two cases to consider:
   - Case 1: `[min.....max]`
   - Case 2: `[max.....min]`

#### Example Walkthrough
```
arr =           [2, 2, 6, 4, 5, 1, 5, 2, 6, 4, 1]
last_min = -1   -1,-1,-1,1,-1,5,5,5,5,5,10
last_max = -1   -1,-1,2,2,2,2,2,2,8,8,8
```
Length calculation: 10-8+1 = 3 (answer)

#### Pseudocode
```java
min_val, max_val
last_min = -1, last_max = -1
ans = INT_MAX
for(i = 0; i < n; i++) {
    if(arr[i] == min_val) {
        last_min = i
        if(last_max != -1) {
            ans = min(ans, i-last_max+1)
        }
    }
    if(arr[i] == max_val) {
        last_max = i
        if(last_min != -1) {
            ans = min(ans, i-last_min+1)
        }
    }
}
return ans
```
**Time Complexity:** O(N)  
**Space Complexity:** O(1)

## 6. Sum of All Subarray Sums
Given an array of N elements, find sum of all subarray sums.

### Example
```
Input: arr = [1, 2, 3]
Subarrays and their sums:
[1]     -> 1
[1,2]   -> 3
[1,2,3] -> 6
[2]     -> 2
[2,3]   -> 5
[3]     -> 3
Total: 20
```

### Approaches

#### 1. Brute Force Approach
```java
for (i = 0; i < n; i++) {
    for(j = i; j < n; j++) {
        sum = 0
        for(k = i; k < n; k++) {
            sum += arr[k]
        }
        ans += sum
    }
}
```
**Time Complexity:** O(n³)  
**Space Complexity:** O(1)

#### 2. Using Prefix Sum Array
```java
// Calculate prefix sum array first
ans = 0
for (i = 0; i < n; i++) {
    for(j = i; j < n; j++) {
        if(i == 0)
            sum = pf[j]
        else
            sum = pf[j] - pf[i-1]
        ans += sum        
    }
}
```
**Time Complexity:** O(N²)  
**Space Complexity:** O(N)

#### 3. Carry Forward Technique
Reduces space complexity while maintaining same time complexity
```java
ans = 0
for (i = 0; i < n; i++) {
    curr_sum = 0
    for(j = i; j < n; j++) {
        curr_sum += arr[j]
        ans += curr_sum        
    }
}
```
**Time Complexity:** O(N²)  
**Space Complexity:** O(1)

#### 4. Mathematical Approach
Using the observation that each element contributes to multiple subarrays
```java
ans = 0
for (i = 0; i < n; i++) {
    // Calculate how many times each element occurs in subarrays
    total_sub = (i+1)(n-i)
    ans += arr[i] * total_sub
}
print(ans)
```
**Time Complexity:** O(N)  
**Space Complexity:** O(1)

## 7. Maximum Subarray Sum of Length K (Sliding Window)
Given arr[N], print maximum sum of any subarray with length k.

### Example
```
Input: arr = [-3, 4, -2, 5, 3, -2, 8, 2, -1, 4], K = 5

Subarrays of length 5:
s   e   sum
0   4   7
1   5   8
2   6   12
3   7   16   <- Maximum
4   8   10
5   9   11

Answer: 16 (Maximum of all sums)
```

### Approaches

#### 1. Brute Force Approach
```java
i = 0, j = k-1
while(j < N) {
    sum = 0
    for (k = 0; k <= j; k++) {
        sum += arr[k]
    }
    ans = max(ans, sum)
    i++
    j++
}
```
**Time Complexity:** O(N²)  
**Space Complexity:** O(1)

#### 2. Using Prefix Sum
```java
// Calculate prefix sum array first
i = 0, j = k-1
while(j < N) {
    sum = 0
    if(i == 0)
        sum = pf[j]
    else
        sum = pf[j] - pf[i-1]

    ans = max(ans, sum)      
    i++
    j++
}
```
**Time Complexity:** O(N)  
**Space Complexity:** O(N)

#### 3. Sliding Window Approach
```java
// 1. Create initial window
sum = 0
for (i = 0; i < k; i++) {
    sum += arr[i]
}
ans = sum

// 2. Slide window and update sum
i = 1, j = k
while(j < N) {
    sum = sum + arr[j] - arr[i - 1]
    ans = max(ans, sum)
    i++
    j++
}
```
**Time Complexity:** O(N)  
**Space Complexity:** O(1)       

 

