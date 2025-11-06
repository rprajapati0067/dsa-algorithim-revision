# Basics Arrays Revesion
## 1. Given an arr[N] and Q queries with start(s) and end(e) index. For every query print sum of all even indexed elements from s to e.
   arr[] = [2,3,1,6,4,5]
   Queries:
   s    e   sum
   1    3   1
   2    5   5   
   0    4   7
   3    3   0
## Approaches

### Brute Force
For every query you can iterate and add only the even index.

### Prefix Sum
1.  Create a prefix sum array by ignoring odd indexes.(Consider odd index as 0)
    so 2 will take because even index
    now 3 which is odd index i.e 1 so what we will do we assume odd index element as 0 so 0+2 = 2
    that we can place in index 1 as below similar way we need to create psum array
    like this psum[] = [2,2,3,3,7,7]

    Pseudocode: Even
    pf_even[n]
    pg_even[0] = arr[0]
    for(i = 1; i < n; i++){
        if(i%2 == 0) {
            pf_even[i] = pf_even[i-1] + arr[i]
        }else{
            pf_even[i] = pf_even[i-1]
        }
    }

    Pseudocode: Odd
    pf_odd[n]
    pg_odd[0] = 0 // because 0 index is even so we taken it as 0
    for(i = 1; i < n; i++){
        if(i%2 == 1) {
            pf_odd[i] = pf_odd[i-1] + arr[i]
        }else{
            pf_odd[i] = pf_odd[i-1]
        }
    }

## 2. Given an arr[N] count the number of special indices in the array.
    Special Index: Index after removing which,
    Sum of even indexed elements = sum of odd indexed elements.

remove idx = 0 -> [4, 3, 2, 7, 6, -2] -> [3, 2, 7, 6, -2] -> now check are the odd and even indexed element sum = 0 3+7-2 == 2+6 i.e 8 == 8 ok
similar way we need to check by removing every element and check even and odd sum, for this particular questions we have 2 special indexes.
so answer is 2. 

### Brute force
You can create the new array after elemenating each index i
Creating the array and finding the sum. TC O(n2)

## Optimise
even idx before i -> pf_even[i-1]
odd idx before i -> pf_odd[i-1]

even idx after i [i+1, n-1]
pf_even[n-1]-pf_even[i]

odd idx after i
pf_odd[n-1] - pf_odd[i]

sum of even indexed elem after removing ith indexed elem -> pf_even[i-1] + pf_odd[n-1] - pf_odd[i]
sum of odd indexed elem after removing ith indexed elem -> pf_odd[i-1] + pf_even[n-1] - pf_even[i-1]

Pseudocode:
1. Calculate pf_even & pf_odd
2. ans = 0
    for(i = 0; i < n; i++){
        se = pf_even[i-1] + pf_odd[n-1] - pf_odd[i]
        so = pf_odd[i-1] + pf_even[n-1] - pf_even[i-1]

        if(se == so)
        ans++
    }
    return ans
    T.C: O(n)
    S.C: O(n)


## 3. Given a string s of lowercase chars, return the count of pairs(i, j) that i < j and s[i] is 'a' and s[j] is 'g'
    st -> [a, b, e, g, a, g]
    ans is [0,3], [0,5] and [4,5] so we have 3 pairs i.e 3 is the answer

### Brute force
    Pseudocode:
    count = 0
    for(i = 0; i < n; i++){
        for(j = i+1; j < n; j++){
            if(s[i] == 'a' && s[j] == 'g'){
                count++
            }
        }
    }
### Optimise
    Observation: A 'g' will form a pair with ALL 'a' on its left
                0 1 2 3 4 5 6 7
                b c a g g a a g

    count_a = 0 0 0 1 1 1 2 3 3
    ans = 0     0 0 0 1 2 2 2 5 -> 5 is final ans
    Pseudocode:
    count_a = 0, ans = 0
    for(i = 0; i < n; i++){
        if(s[i] == 'a'){
            count_a++
        }else if(s[i] == 'g'){
            ans += count_a
        }
    }
    return ans
    T.C: O(N)
    S.C: O(1)

## 4. Print all the possible sub-arrays of the given array
    [5, 7, 3, 2]  
    O/P:[5]
        [5, 7]
        [5, 7, 3]
        [5, 7, 3, 2]
        [7]
        [7, 3]
        [7, 3, 2]
        [3]
        [3, 2]
        [2]
 ### Brute force:
    Iterate over each sub array and print
    for(start = 0; start < n; start++){
        for(end = start; end < n; end++){
            printSubarr(arr, start, end) // a function and method to print
        }
    }
## 5. Given an array of N integers, return the length of smallest subarray which contains both max and min elements of the array.
    Constraint: 1 <= N <= 10^6, arr = [2, 2, 6 ,4, 5, 1, 5, 2, 6, 4, 1], max = 6 and min = 1, ans => [6, 4, 1] i.e 3 is the answer

### Brute force:
Check all subarrays -> T.C: O(N^3)

### Observation
1. There must be exactly one occurance of min & max element -> [min  min....max]
2. Min and Max elem should be the end point of subarray.
3. Case 1: [min.....max]
   Case 2: [max.....min]

arr =           [2, 2, 6 ,4, 5, 1, 5, 2, 6, 4, 1]
last_min = -1   -1,-1,-1,1,-1 5,5,5,5,5,10
last_max = -1   -1,-1, 2,2,2,2,2,2,8,8,8
8 to 10 how to calculate length 10-8 +1 = 3, so ans = 3
Pseudocode:
min_val, max_vall
last_min = -1, last_max = -1
ans = INT_MAX
for(i = 0; i < n; i++){
    if(arr[i] == min_val){
        last_min = i
        if(last_max != -1){
            ans = min(ans, i-last_max+1)
        }
    }
    if(arr[i] == max_val){
        last_max = i
        if(last_min != -1){
            ans = min(ans, i-last_min+1)
        }
    }
}
return ans

## 6. Given an array of N, find sum of all subarrays sums
arr = [1, 2, 3]
[1]     -> 1
[1,2]   -> 3
[1,2,3] -> 6
[2]     -> 2
[2,3]   -> 5
[3]     -> 3
    total: 20
### Brute force:    
    Pseudocode: 
    for (i = 0; i < n; i++){
        for(j = i; j < n; j++){
            sum = 0
            for(k = i; k < n; k++){
                sum += arr[k]
            }
            ans += sum
        }
    }
T.C: O(n^3)

### Use Prefix sum array
    Approach 1:
    1. calculate pf array
    ans =0
    for (i = 0; i < n; i++){
        for(j = i; j < n; j++){
            if(i == 0)
                sum = pf[j]
            else
                sum = pf[j] - pf[i-1]

            ans += sum        
        }
    }
    T.C: O(N^2)
    S.C: O(N)

    Approach 2:
    Second approach is to take carry forward technique where we will be able to reduce the S.C to O(1) keeping the same T.C: O(N^2)
    ans = 0
    for (i = 0; i < n; i++){
        curr_sum = 0
        for(j = i; j < n; j++){
            curr_sum += arr[j]
            ans += curr_sum        
        }
    }
    Approach 3:
    start = i+1
    end = n-i
    total_sub = (i+1)(n-i)
    ans = 0
    for (i = 0; i < n; i++){
        total_sub = (i+1)(n-i). // how many times a elem occors
        ans += arr[i] * total_sub
    }
    print(ans)
    T.C: O(N)
    S.C: O(1)

## 7. Given arr[N], Print maximum subarray sum of subarray with length k. (Sliding Window)
    arr = [-3,4,-2,5,3,-2,8,2,-1,4], K = 5
    s   e   sum
    0   4   7
    1   5   8
    2   6   12
    3   7   16
    4   8   10
    5   9   11
        ans: 16 (Max of all sum)

### Brute force:    
    Pseudocode:
    Approach 1:
    i = 0, j = k-1
    while(j < N>){
        sum = 0
        for (k = 0; k <= j; k++){
            sum += arr[k]
        }
        ans = max(ans, sum)
        i++
        j++
    }
    T.C: O(N^2)

    Approach 2:
    1. Calculate psum
    i = 0, j = k-1
    while(j < N>){
        sum = 0
        if(i == 0)
            sum = pf[j]
         else
            sum = pf[j] - pf[i-1]

        ans = max(ans, sum)      
        i++
        j++
    }
    T.C: O(N)
    Approach 3: Sliding Window
    1. Create the window -> calculate sum of first K elements
        sum = 0
        for (i = 0; i < kj; i++){
            sum += arr[k]
        }
        ans = sum
    2. Consider the remaining subarrays of length K with sliding window
        i = 1, j = k
        while(j < N){
            sum = sum + arr[j] - arr[i - 1]
            ans = max(ans, sum)
            i++
            j++
        }
    T.C: O(N)
    S.C: O(1)       

 

