## 1. Given a string consisting of lower-case and upper-case alphabets
##  Converts: (1) lower -> uppercase
##            (2) upper -> lowercase 
S = "Hello", O/P: hELLO

   
## Approaches
If captial letter, do +32
If Small letter, do -32

Pseudocode:
for(i = 0; i < n; i++){
    if(s[i] >= 'a' && s[i] <= 'z'){
        s[i] = char(s[i] - 32)
    }else{
        s[i] = char(s[i] + 32)
    }
}
return new String(arr)

## 2. Check for substring if its's palindrome or not
S = "a, b, m, a, d, a, m, t, a, m"  startIndex = 2, endIndex = 6 is a palindrome

   
## Approaches

Pseudocode:
boolean isPalindrome(String str, int s, int e){
    while(s < e){
        if(str[s] != str[e]){
            return false
        }
        else{
            s++
            e--
        }
    }
    return true
}
T.C: O(N)
S.C: O(1)

## 3. Given a string s. Find the length of the longest palindrome substring in s.
S = "a n m a d a m m"  , ans = 5

   
### Brute force
If I can go to all substring, check if it is a palindrome , consider it.
Pseudocode:
ans = 0
for(i = 0; i < n; i++){
    for(j = i; j < n; j++){
        if(isPalindrom(s,i,j))  // above we have its implementation
        ans = max(ans,j-i+1)
    }
}
T.C: O(N^3)
S.C: O(1) 

### Optimise
Pseudocode:
ans = 0
for(i = 0; i < n; i++){
    // odd length -> sinngle center
    s = i, e = i
    while(s >= 0 && e < n){
        if(str[s] != str[e])
            break
         else{
            s--
            e++
         }   
    }
    ans = max(ans, e-s-1)
}

for(i = 0; i < n-1; i++){
    // even length
    s = i, e = i+1
    while(s >= 0 && e < n){
        if(str[s] != str[e])
            break
         else{
            s--
            e++
         }   
    }
    ans = max(ans, e-s-1)
}

return ans
T.C: O(N^2)
S.C: O(1) 
# Interview Question

## 3. Given a binary array[]. we can atmost replace a singlle zero with 1. Find the maximum consecutive 1's we can get in the array[] after the replacement.
         0 1 2 3 4 5 6 7  
arr[] = [1 1 0 1 1 1 1 0] after making 0 at 2nd index to 1 we will be getting maximum consecutive 1's that what we need to do.

### Optimize
Pseudocode:
1. If totalOnes == n return n
2. If totalOnes == 0 return 1
ans = 0
for(i = 0; i < n; i++){
    if(arr[i] == 0){
        int l = 0
        j = i-1
        while(j >= 0 && arr[j] == 1){
            l++
            j--
        }
        r = 0, j = i+1
        while(j < n && arr[j] == 1){
            r++
            j++
        }
        len = l+ r + 1
        ans = max(ans, len)
    }
}
return ans;
T.C: O(N) 

### Variant 
### Given a binary array[]. we can swap a single 0 with 1. Find the maximum consecutive 1's we can get in the array[] after almost 1 swap.
1. If totalOnes == n return n
2. If totalOnes == 0 return 1
ans = 0
for(i = 0; i < n; i++){
    if(arr[i] == 0){
        int l = 0
        j = i-1
        while(j >= 0 && arr[j] == 1){
            l++
            j--
        }
        r = 0, j = i+1
        while(j < n && arr[j] == 1){
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
T.C: O(N)
## 4. Given array[N].Find the majority element.(Majority element => elements which occurs more than N/2 times) You can assume that majority always exists. (Moore's Voting Algorithim)
ex: [2,3,3,4,4,4,4,4] => ans = 4
    [3,3,4,2,4,4,2,4] => here 4 is not majority element because its exactly 50% we need more that 50% i.e N/2
### Observation
At max, How many majority elements are possible?
### Brute force
Get the max freq & see if this guy is majority

### Optimise
If you remove 2 different elements majority element remains the same.
        [3 4 3 6 1 3 2 5 3 3 3]
ele      3 3 3 3 1 1 2 2 3 3 3
count    1 0 1 0 1 0 1 0 1 2 3

Pseudocode:
ele = arr[0] count = 1
for(i = 1; i < n; i++){
    if(ele == arr[i])
        count++
    else{
        if(count > 0)
            count--
        else{
            ele = arr[i]
            count = 1
        }    
    }    
}
// check if ele is majority
for(i = 0; i < n; i++){
    if(arr[i] == ele)
        count++
}
if(count > N/2)
    return ele
else
    return -1

## 5. Given an array [N][M]. Make all elements in a row and column zero if arr[i][j]

1   2   3   4           1   2   0   0
5   6   7   0      =>   0   0   0   0
9   2   0   4           0   0   0   0

1   2   3   4           1   2   3   0
5   6   7   0      =>   0   0   0   0
9   2   13  4           9   2   13  0

When we update last col in 2nd example it also made 0 in last row, and because we have 0 in last row do we again make the last row to 0 or not? that is the problem how to handle that? It means we are not able to make difference b/w original 0 and create 0.
Solution: When you convert, convert it to -1 so that you know that its a created one and then convert -1 only to 0 that it.

### Brute force
// Row wise
for(i = 0; i < n; i++){
    has_zero = false
    for(j = 0; j < m; j++){
        if(a[i]a[j] == 0)
            has_zero = true
    }
    if(has_zero){
        for(j = 0; j < m; j++){
            if(a[i]a[j] != 0)
                a[i]a[j] = -1
        }
    }
}
// Column wise
for(j = 0; j < m; j++){
    has_zero = false
    for(i = 0; i < n; i++){
        if(a[i]a[j] == 0)
            has_zero = true
    }
    if(has_zero){
        for(i = 0; i < n; i++){
            if(a[i]a[j] != 0)
                a[i]a[j] = -1
        }
    }
}
// Conversion code
for(i = 0; i < n; i++){
     for(j = 0; j < m; j++){
         if(a[i]a[j] == -1)
            a[i]a[j] = -1
     }
}
T.C: (n*m)
S.C: (1)




