# Sorting Revesion
## 1. Given N elements, at every step remove an array element.Cost to remove an element == sum of array of elements present in an array
## Find minimum cost to remove all elements. NOTE: First add the cost of removal and then remove it.
   arr[] = 
   [2, 1, 4]    7 -> 2+1+4 = 7
   [2, 4]       6 -> 2+4 = 6
   [4]          4 -> 4   = 4     
   []           17       = 17

   [2, 1, 4]    7 -> remove biggest num i.e 4
   [2, 1]       3 -> again remove biggest num i.e 2
   [1]          1     
   []           11 <- total i.e 11 is the ans (min)

   
## Approaches

### Brute Force
At each step, remove biggest number

### Solution
1.  We sort it first and do this
    0,      1,       2,      3  <- these represent indexed } a < b < c < d
    arr[0]  arr[1]   arr[2]  arr[3] -> arr[0] + arr[1] + arr[2] + arr[3]
    arr[0]  arr[1]   arr[2]         -> arr[0] + arr[1] + arr[2]             (arr[3] is removed)
    arr[0]  arr[1]                  -> arr[0] + arr[1]                      (arr[2] is removed)
    arr[0]                          -> arr[0]                               (arr[1] is removed)
    []                              -> 4 arr[0] + 3 arr[1] + 2 arr[2] + arr[3]  this implies that =>  arr[i] * (N-i)                       

    Pseudocode:
    Arrays.sort(arr)
    ans = 0
    for(i = 0; i < n; i++){
        ans += (n-i) * arr[i]
    }
    return
    T.C: O(nlogn)
    S.C: O(1)

## Noble Integers [Distinct Data]
## 2. Given N array elements, calculate number of noble integer.
##    An element ele is arr[] is said to be noble if [count of smaller elements = ele itself]
    arr = [1, -5, 3, 5, -10, 4], ans = 3
    i = 0, arr[0] = 1, how many ele in array is smaller than 1 we have two i.e -5 and -10, so this is not a noble number
    i = 1, arr[1] = -5,
    i = 2, arr[2] = 3, ele smaller than 3 is 3 right i.e 1, -5 and -10 so this is noble

### Brute Force
N^2 loop idea
for each i, count a[j] < a[i]
    if count == a[i]
        ans++

### Optimise
        0   1   2   3 -> index
        -3  0   2   5
smaller 0   1   2   3

Do we see a relation b/w smaller and the indexed i.e the no. of people smaller than me is equal to my index
Pseudocode:
Arrays.sort(arr)
ans = 0
for(i = 0; i < n; i++){
    if(arr[i] == i)
        ans++
}
return ans
T.C: O(nlogn)
S.C: O(1)

### Variant (Noble Integer): [Data can repeat]
        0   1  2  3  4 -> index
arr = [-10, 1, 1, 3, 100]
small   0   1  1  3  4      ans is 3
Pseudocode:
Arrays.sort(arr)
count = 0, and = 0
for(i = 1; i < n; i++){
    if(arr[i] != arr[i-1])
        count = i
     if(count == arr[i])
        ans++   
}
T.C: O(nlogn)
S.C: O(1)

## 3. Selection Sort
### Idea: Select the minimum element and send that elements to correct position by swapping.
arr =  [3,  5,  1 , 4,  2]
        1,  5,  3,  4,  2
        1,  2,  3,  4,  5
Select smallest guy and get at current position
Pseudocode:
for(i = 0; i < n; i++){
    min_index = i
    for(j = i+1; j < n; j++){
        if(a[j] < a[min_index]){
            min_index = j
        }
    }
    swap(arr[i], arr[min_index])   
}
T.C: O(N^2)
S.C: O(1)

## 4. Insertion Sort (Arrangement of playing cards)
### Idea: assume that i = 0 elem is sorted and start with i = 1
arr =  [8,  3,  5 , 2,  7]
        3,  8,  5,  2,  7
        3,  5,  8,  2,  7
        3,  5,  2,  8,  7
        3,  2,  5,  8,  7
        2,  3,  5,  8,  7
        2,  3,  5,  7,  8
Pseudocode:
for(i = 1; i < n; i++){
    curr = arr[i]
    j = i -1
    while(j >= 0 && arr[j] > curr){
        arr[j+1] = arr[j]
        j--
    }
    arr[j+1] = curr
}
T.C: O(N^2)
S.C: O(1)