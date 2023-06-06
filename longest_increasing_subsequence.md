## Problem
Given an integer array `nums`, return _the length of the longest **strictly increasing**_

_**subsequence**_.

*Example 1:*

```markdown
**Input:** nums = [10,9,2,5,3,7,101,18]
**Output:** 4
**Explanation:** The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

*Example 2:*
```markdown
**Input:** nums = [0,1,0,3,2,3]
**Output:** 4
```

*Example 3:*
```markdown
**Input:** nums = [7,7,7,7,7,7,7]
**Output:** 1
```

## Solutions

### Brute-Force
We need to find maximum increasing subsequence length. In the brute-force approach, we can model this problem as -

1. If the current element is greater than the previous element, then we can either pick it or dont pick it because we may get a smaller element somewhere ahead which is greater than previous and picking that would be optimal. So we try both options.
2. If the current element is smaller or equal to previous element, it can't be picked.

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums, int i = 0, int prev = INT_MIN) {
        if(i == size(nums)) return 0;
        return max(lengthOfLIS(nums, i + 1, prev), (nums[i] > prev) + lengthOfLIS(nums, i + 1, max(nums[i], prev)));
    }
};
```

A better and more understandable way of writing the same code as above -

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        return solve(nums, 0, INT_MIN);
    }
    int solve(vector<int>& nums, int i, int prev) {
        if(i >= size(nums)) return 0;                                // cant pick any more elements
        int take = 0, dontTake = solve(nums, i + 1, prev);           // try skipping the current element
        if(nums[i] > prev) take = 1 + solve(nums, i + 1, nums[i]);   // or pick it if it is greater than previous picked element
        return max(take, dontTake);                                  // return whichever choice gives max LIS
    }
};
```

_**Time Complexity :**_ **`O(2^N)`**, where `N` is the size of _`nums`_. At each index, we have choice to either take or not take the element and we explore both ways. So, we `2 * 2 * 2...N times = O(2^N)`  
_**Space Complexity :**_ **`O(N)`**, max recursive stack depth.

### DP with memoization
In the previous solution we can observe that there're some redundant calls to `solve` function.

For example:
Let we have `[2,3,5,6,4]`

Let us peek number *2* and then *3*

At the same time we do not peek *2* but peek *3*

And in that case two branches need to know `solve(nums, 2, 3)`

So we need to memorize results for parameters `(i, prev)`. The easiest way to do this is to use 2D matrix
```cpp
class Solution {
public:
    vector<vector<int>> dp;
    int lengthOfLIS(vector<int>& nums) {
        dp.resize(size(nums), vector<int>(1+size(nums), -1));   // dp[i][j] denotes max LIS starting from i when nums[j] is previous picked element
        return solve(nums, 0, -1);
    }
    int solve(vector<int>& nums, int i, int prev_i) {
        if(i >= size(nums)) return 0;
        if(dp[i][prev_i+1] != -1) return dp[i][prev_i+1];
        int take = 0, dontTake = solve(nums, i + 1, prev_i);
        if(prev_i == -1 || nums[i] > nums[prev_i]) take = 1 + solve(nums, i + 1, i); // try picking current element if no previous element is chosen or current > nums[prev_i]
        return dp[i][prev_i+1] = max(take, dontTake);
    }
};
```

_**Time Complexity :**_ **`O(N^2)`**  
_**Space Complexity :**_ **`O(N^2)`**

### DP with memoization and space optimization
We solved the problem memorizing function calls.
We can do better and further reduce the state stored using DP.
It's redundant to store states for all `i` having `prev` as its previous element index.
The length will always be greatest for the state `(prev, prev)` since no more elements before `prev` can be taken.
So we can just use a linear DP where `dp[i]` denotes the LIS starting at index `i`
```cpp
class Solution {
public:
    vector<int> dp;
    int lengthOfLIS(vector<int>& nums) {
        dp.resize(size(nums)+1, -1);
        return solve(nums, 0, -1);
    }
    int solve(vector<int>& nums, int i, int prev_i) {
        if(i >= size(nums)) return 0;
        if(dp[prev_i+1] != -1) return dp[prev_i+1];
        int take = 0, dontTake = solve(nums, i + 1, prev_i);
        if(prev_i == -1 || nums[i] > nums[prev_i])
            take = 1 + solve(nums, i + 1, i);
        return dp[prev_i+1] = max(take, dontTake);
    }
};
```
_**Time Complexity :**_ **`O(N2)`**  
_**Space Complexity :**_ **`O(N)`**

### Binary Search
In the brute-force approach, we were not sure if an element should be included or not to form the longest increasing subsequence and thus we explored both options. The problem lies in knowing if an element must be included in the sequence formed till now. Let's instead try an approach where we include element whenever possible to maximize the length and if it's not possible, then create a new subsequence and include it.

Consider an example - `[1,7,8,4,5,6,-1,9]`:

1. Let's pick first element - `1` and form the subseqeunce **`sub1=[1]`**.
2. `7` is greater than previous element so extend the sequence by picking it.   **`sub1=[1,7]`**.
3. Similarly, we pick `8` as well since it's greater than `7`.   **`sub1=[1,7,8]`**
4. Now we cant extend it further. We can't simply discard previous sequence and start with `4` nor can we discard `7,8` and place `4` instead of them because we don't know if future increasing subsequence will be of more length or not. So we keep both previous subsequence as well as try picking `4` by forming a new subsequence. It's better to form new subsequence and place `4` after `1` to maximize new sequence length. So we have **`sub1=[1,7,8]`** and **`sub2=[1,4]`**
5. Can we add `5` in any of the sequence? Yes we can add it to `sub2`. If it wasn't possible we would have tried the same approach as in 4th step and created another subsequence list.   **`sub1=[1,7,8], sub2=[1,4,5]`**
6. Similarly, add `6` to only possible list - `cur2`.   **`sub1=[1,7,8], sub2=[1,4,5,6]`**
7. Now, `-1` cant extend any of the existing subsequence. So we need to form another sequence. Notice we cant copy and use any elements from existing subsequences before `-1` either, since `-1` is lowest. **`sub1=[1,7,8], sub2=[1,4,5,6], sub3=[-1]`**
8. Now, `9` can be used to extend all of the list. At last, we get   **`sub1=[1,7,8,9], sub2=[1,4,5,6,9], sub3=[-1,9]`**

We finally pick the maximum length of all lists formed till now. This approach works and gets us the correct LIS but it seems like just another **inefficient approach because it's costly to maintain multiple lists and search through all of them when including a new element or making a new list**. Is there a way to speed up this process? Yes. We can just maintain a single list and mark multiple lists inside it. Again, an example will better explain this.

Consider the same example as above - `[1,7,8,4,5,6,-1,9]`:

1. Pick first element - `1` and form the subseqeunce **`sub=[1]`**.
    
2. `7` is greater than `1` so extend the existing subsequence by picking it.   **`sub=[1,7]`**.
    
3. Similarly, we pick `8` as well since it's greater than `7`.   **`sub=[1,7,8]`**
    
4. **Now comes the main part**. We can't extend any existing sequence with `4`. So we need to create a new subsequence following 4th step previous approach but this time we will create it inside `sub` itself by replacing smallest element larger than `4` (Similar to 4th step above where we formed a new sequence after picking smaller elements than `4` from existing sequence).
    
    ```csharp
        [1,    4,      8]
               ^sub2   ^sub1
    
    This replacement technique works because replaced elements dont matter to us
    We only used end elements of existing lists to check if they can be extended otherwise form newer lists
    And since we have replaced a bigger element with smaller one it wont affect the 
    step of creating new list after taking some part of existing list (see step 4 in above approach)
    ```
    
5. Now, we can't extend with `5` either. We follow the same approach as step 4.
    
    ```python
        [1,    4,    5]
    				 ^sub2
    	
    Think of it as extending sub2 in 5th step of above appraoch
    Also, we can see sub2 replaced sub1 meaning any subsequence formed with sub2 always
    has better chance of being LIS than sub1.
    ```
    
6. We get `6` now and we can extend the `sub` list by picking it.
    
    ```csharp
        [1,    4,    5,    6]
    				       ^sub2	
    ```
    
7. Cant extend with `-1`. So, Replace -
    
    ```csharp
        [-1,    4,    5,   6]
    	  ^sub3		       ^sub2	
    
    We have again formed a new list internally by replacing smallest element larger than -1 from exisiting list
    ```
    
8. We get `9` which is greater than the end of our list and thus can be used to extend the list
    
    ```
        [-1,    4,    5,    6,    9]
    	  ^sub3		              ^sub2	
    ```
    
      
    Finally the length of our maintained list will denote the LIS length = `5`. Do note that it wont give the LIS itself but just correct length of it.

The optimization which improves this approach over DP is applying **Binary search** when we can't extend the sequence and need to replace some element from maintained list - `sub`. The list always remains sorted and thus binary search gives us the correct index of element in list which will be replaced by current element under iteration.

Basically, we will compare end element of `sub` with element under iteration `cur`. If `cur` is bigger than it, we just extend our list. Otherwise, we will simply apply binary search to find the smallest element >= cur and replace it. Understanding the explanation till now was the hard part...the approach is very easy to code.

Without modifying input array:

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<int> tail; 
        for(int num: nums){
            if (tail.empty() || num > tail.back()){
                tail.push_back(num);
            } else {
                int index = lower_bound(tail.begin(), tail.end(), num) - tail.begin();
                tail[index] = num;
            }
        }
        return tail.size();
    }
};

```

With modifying input array:

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& A) {
        int len = 0;
        for(auto cur : A) 
            if(len == 0 || A[len-1] < cur) A[len++] = cur;             // extend
            else *lower_bound(begin(A), begin(A) + len, cur) = cur;    // replace
        return len;
    }
};
```

_**Time Complexity :**_ **`O(NlogN)`**  
_**Space Complexity :**_ **`O(1)`**

---
## Links
[[leetcode]] - problem № 300
