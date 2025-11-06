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
