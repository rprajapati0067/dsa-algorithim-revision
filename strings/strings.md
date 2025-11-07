## 1. Given a string consisting of lower-case and upper-case alphabets
Convert:
1. lower -> uppercase
2. upper -> lowercase 

**Input:** S = "Hello"  
**Output:** "hELLO"

### Approaches
- If capital letter, do +32
- If small letter, do -32

### Pseudocode
```java
for(i = 0; i < n; i++) {
    if(s[i] >= 'a' && s[i] <= 'z') {
        s[i] = char(s[i] - 32)
    } else {
        s[i] = char(s[i] + 32)
    }
}
return new String(arr)

## 2. Check for substring if it's palindrome or not
**Input:** S = "a, b, m, a, d, a, m, t, a, m"  
startIndex = 2, endIndex = 6 is a palindrome

### Approaches

### Pseudocode
```java
boolean isPalindrome(String str, int s, int e) {
    while(s < e) {
        if(str[s] != str[e]) {
            return false
        }
        else {
            s++
            e--
        }
    }
    return true
}
```
**Time Complexity:** O(N)  
**Space Complexity:** O(1)

## 3. Given a string s. Find the length of the longest palindrome substring in s.
**Input:** S = "a n m a d a m m"  
**Output:** ans = 5

### Brute force
If we can go to all substrings, check if it is a palindrome, consider it.

#### Pseudocode
```java
ans = 0
for(i = 0; i < n; i++) {
    for(j = i; j < n; j++) {
        if(isPalindrom(s,i,j))  // above we have its implementation
        ans = max(ans,j-i+1)
    }
}
```
**Time Complexity:** O(N^3)  
**Space Complexity:** O(1) 

### Optimized Approach
#### Pseudocode
```java
ans = 0
for(i = 0; i < n; i++) {
    // odd length -> single center
    s = i, e = i
    while(s >= 0 && e < n) {
        if(str[s] != str[e])
            break
        else {
            s--
            e++
        }   
    }
    ans = max(ans, e-s-1)
}

for(i = 0; i < n-1; i++) {
    // even length
    s = i, e = i+1
    while(s >= 0 && e < n) {
        if(str[s] != str[e])
            break
        else {
            s--
            e++
        }   
    }
    ans = max(ans, e-s-1)
}

return ans
```
**Time Complexity:** O(N^2)  
**Space Complexity:** O(1)
# Interview Questions

## 3. Maximum Consecutive Ones after Single Zero Replacement
Given a binary array[]. We can at most replace a single zero with 1. Find the maximum consecutive 1's we can get in the array[] after the replacement.

**Example:**
```
Index:  0 1 2 3 4 5 6 7  
arr[] = [1 1 0 1 1 1 1 0]
```
After making 0 at 2nd index to 1 we will be getting maximum consecutive 1's.

### Optimized Approach
#### Pseudocode
```java
// If totalOnes == n return n
// If totalOnes == 0 return 1
ans = 0
for(i = 0; i < n; i++) {
    if(arr[i] == 0) {
        int l = 0
        j = i-1
        while(j >= 0 && arr[j] == 1) {
            l++
            j--
        }
        r = 0, j = i+1
        while(j < n && arr[j] == 1) {
            r++
            j++
        }
        len = l + r + 1
        ans = max(ans, len)
    }
}
return ans;
```
**Time Complexity:** O(N)

### Variant 
### Maximum Consecutive Ones after Single Zero Swap
Given a binary array[]. We can swap a single 0 with 1. Find the maximum consecutive 1's we can get in the array[] after at most 1 swap.

#### Pseudocode
```java
// If totalOnes == n return n
// If totalOnes == 0 return 1
ans = 0
for(i = 0; i < n; i++) {
    if(arr[i] == 0) {
        int l = 0
        j = i-1
        while(j >= 0 && arr[j] == 1) {
            l++
            j--
        }
        r = 0, j = i+1
        while(j < n && arr[j] == 1) {
            r++
            j++
        }
        if(l+r == totalOnes)
            len = l+r
        else
           len = l+r+1     
        ans = max(ans, len)
    }
}
return ans;
```
**Time Complexity:** O(N)
## 4. Find the Majority Element using Moore's Voting Algorithm
Given array[N]. Find the majority element. (Majority element => elements which occurs more than N/2 times) You can assume that majority always exists.

**Examples:**
```
[2,3,3,4,4,4,4,4] => ans = 4
[3,3,4,2,4,4,2,4] => here 4 is not majority element because its exactly 50% 
                     we need more than 50% i.e N/2
```

### Observation
At max, how many majority elements are possible?

### Brute Force
Get the max frequency & see if this element is majority

### Optimized Approach (Moore's Voting Algorithm)
If you remove 2 different elements majority element remains the same.
```
Array:  [3 4 3 6 1 3 2 5 3 3 3]
ele:     3 3 3 3 1 1 2 2 3 3 3
count:   1 0 1 0 1 0 1 0 1 2 3
```

#### Pseudocode
```java
ele = arr[0] count = 1
for(i = 1; i < n; i++) {
    if(ele == arr[i])
        count++
    else {
        if(count > 0)
            count--
        else {
            ele = arr[i]
            count = 1
        }    
    }    
}
// check if ele is majority
for(i = 0; i < n; i++) {
    if(arr[i] == ele)
        count++
}
if(count > N/2)
    return ele
else
    return -1
```

## 5. Set Matrix Zeroes
Given an array [N][M]. Make all elements in a row and column zero if arr[i][j] is zero.

**Example 1:**
```
Input:
1   2   3   4           1   2   0   0
5   6   7   0     =>    0   0   0   0
9   2   0   4           0   0   0   0
```

**Example 2:**
```
Input:
1   2   3   4           1   2   3   0
5   6   7   0     =>    0   0   0   0
9   2   13  4           9   2   13  0
```

**Challenge:** When we update last col in 2nd example it also made 0 in last row, and because we have 0 in last row do we again make the last row to 0 or not? That's the problem - how to handle that? It means we are not able to make difference between original 0 and created 0.

**Solution:** When you convert, convert it to -1 so that you know that it's a created one and then convert -1 only to 0.

### Brute Force Approach
#### Pseudocode
```java
// Row wise
for(i = 0; i < n; i++) {
    has_zero = false
    for(j = 0; j < m; j++) {
        if(a[i][j] == 0)
            has_zero = true
    }
    if(has_zero) {
        for(j = 0; j < m; j++) {
            if(a[i][j] != 0)
                a[i][j] = -1
        }
    }
}
// Column wise
for(j = 0; j < m; j++) {
    has_zero = false
    for(i = 0; i < n; i++) {
        if(a[i][j] == 0)
            has_zero = true
    }
    if(has_zero) {
        for(i = 0; i < n; i++) {
            if(a[i][j] != 0)
                a[i][j] = -1
        }
    }
}
// Conversion code
for(i = 0; i < n; i++) {
    for(j = 0; j < m; j++) {
        if(a[i][j] == -1)
            a[i][j] = 0
    }
}
```
**Time Complexity:** O(n*m)  
**Space Complexity:** O(1)




