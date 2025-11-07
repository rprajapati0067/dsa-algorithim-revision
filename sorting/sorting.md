# Sorting Revision

## 1. Minimum Cost to Remove All Elements
Given N elements, at every step remove an array element. Cost to remove an element == sum of array of elements present in an array.  
Find minimum cost to remove all elements. **NOTE:** First add the cost of removal and then remove it.

### Example 1 (Cost = 17):
```
arr[] = 
[2, 1, 4]    7 -> 2+1+4 = 7
[2, 4]       6 -> 2+4 = 6
[4]          4 -> 4   = 4     
[]           17      = 17
```

### Example 2 (Cost = 11):
```
[2, 1, 4]    7 -> remove biggest num i.e 4
[2, 1]       3 -> again remove biggest num i.e 2
[1]          1     
[]           11 <- total i.e 11 is the ans (min)
```

### Approaches

#### Brute Force
At each step, remove biggest number

#### Optimized Solution
We sort it first and process:
```
Index:  0       1        2       3  <- these represent indices } a < b < c < d
        arr[0]  arr[1]   arr[2]  arr[3] -> arr[0] + arr[1] + arr[2] + arr[3]
        arr[0]  arr[1]   arr[2]         -> arr[0] + arr[1] + arr[2]             (arr[3] is removed)
        arr[0]  arr[1]                  -> arr[0] + arr[1]                      (arr[2] is removed)
        arr[0]                          -> arr[0]                               (arr[1] is removed)
        []                              -> 4 arr[0] + 3 arr[1] + 2 arr[2] + arr[3]
```
This implies that =>  arr[i] * (N-i)

##### Pseudocode
```java
Arrays.sort(arr)
ans = 0
for(i = 0; i < n; i++) {
    ans += (n-i) * arr[i]
}
return ans
```
**Time Complexity:** O(nlogn)  
**Space Complexity:** O(1)

## 2. Noble Integers [Distinct Data]
Given N array elements, calculate number of noble integers.  
An element ele in arr[] is said to be noble if [count of smaller elements = ele itself]

### Example
```
arr = [1, -5, 3, 5, -10, 4], ans = 3

i = 0, arr[0] = 1  -> elements smaller than 1: -5, -10 (count=2) -> not noble
i = 1, arr[1] = -5 -> ...
i = 2, arr[2] = 3  -> elements smaller than 3: 1, -5, -10 (count=3) -> noble
```

### Brute Force Approach
N² loop idea:
```java
for each i, count a[j] < a[i]
    if count == a[i]
        ans++
```

### Optimized Approach
```
        0   1   2   3 -> index
        -3  0   2   5
smaller 0   1   2   3
```
Do we see a relation between smaller elements and the index? i.e., the number of elements smaller than me is equal to my index.

#### Pseudocode
```java
Arrays.sort(arr)
ans = 0
for(i = 0; i < n; i++) {
    if(arr[i] == i)
        ans++
}
return ans
```
**Time Complexity:** O(nlogn)  
**Space Complexity:** O(1)

### Variant: Noble Integer [Data can repeat]
```
        0   1  2  3  4 -> index
arr = [-10, 1, 1, 3, 100]
small   0   1  1  3  4      ans is 3
```

#### Pseudocode
```java
Arrays.sort(arr)
count = 0, ans = 0
for(i = 1; i < n; i++) {
    if(arr[i] != arr[i-1])
        count = i
    if(count == arr[i])
        ans++   
}
```
**Time Complexity:** O(nlogn)  
**Space Complexity:** O(1)

## 3. Selection Sort
### Idea
Select the minimum element and send that element to correct position by swapping.

### Example
```
arr =  [3,  5,  1,  4,  2]
       [1,  5,  3,  4,  2]  // After first iteration
       [1,  2,  3,  4,  5]  // Final sorted array
```
Select smallest element and place at current position.

### Pseudocode
```java
for(i = 0; i < n; i++) {
    min_index = i
    for(j = i+1; j < n; j++) {
        if(a[j] < a[min_index]) {
            min_index = j
        }
    }
    swap(arr[i], arr[min_index])   
}
```
**Time Complexity:** O(N²)  
**Space Complexity:** O(1)

## 4. Insertion Sort
Similar to arranging playing cards in hand.

### Idea
Assume that element at i = 0 is sorted and start with i = 1.

### Example
```
Initial:  [8,  3,  5,  2,  7]
Step 1:   [3,  8,  5,  2,  7]
Step 2:   [3,  5,  8,  2,  7]
Step 3:   [3,  5,  2,  8,  7]
Step 4:   [3,  2,  5,  8,  7]
Step 5:   [2,  3,  5,  8,  7]
Final:    [2,  3,  5,  7,  8]
```

### Pseudocode
```java
for(i = 1; i < n; i++) {
    curr = arr[i]
    j = i - 1
    while(j >= 0 && arr[j] > curr) {
        arr[j+1] = arr[j]
        j--
    }
    arr[j+1] = curr
}
```
**Time Complexity:** O(N²)  
**Space Complexity:** O(1)
