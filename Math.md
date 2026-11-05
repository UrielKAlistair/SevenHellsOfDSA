## Bitwise Operations

### Single Number
[Link](https://leetcode.com/problems/single-number/)

Given a non-empty array of integers nums, every element appears twice except for one. Find that single one. Implement a solution with a linear runtime complexity and use only constant extra space.

> [!success]- Solution
> Key Observation: a^a=0

> [!example]- Code
> ```cpp
> class Solution {
> public:
>     int singleNumber(vector<int>& nums) {
>         int ans=0;
>         for(auto i: nums){
>             ans^=i;
>         }
>         return ans;
>     }
> };
> ```

### Number of 1 Bits
[Link](https://leetcode.com/problems/number-of-1-bits/)

Write a function that takes the binary representation of an unsigned integer and returns the number of '1' bits it has (also known as the [Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)).

> [!success]- Solution
> x&1 is the least significant bit of x. setting x = x>>1 allows one to loop through bits.

> [!example]- Code
> ```cpp
> class Solution {
> public:
>     int hammingWeight(uint32_t n) {
>         int ans=0;
>         while(n){ans+=(n&1);n=n>>1;}
>         return ans;
>     }
> };
> ```

### Reverse Bits
[Link](https://leetcode.com/problems/reverse-bits/)

Reverse bits of a given 32 bits unsigned integer. (Easy, especially if you saw the ans to 2)

> [!example]- Code
> ```cpp
> class Solution {
> public:
>     uint32_t reverseBits(uint32_t n) {
>         int ans=0;
>         for(int i=0;i<32;i++){
>             ans = (ans<<1) + (n&1);
>             n=n>>1;
>         }
>         return ans;
>     }
> };
> ```

### Bitwise AND of Numbers Range
[Link](https://leetcode.com/problems/bitwise-and-of-numbers-range)

Given two integers left and right that represent the range [left, right], return the bitwise AND of all numbers in this range, inclusive.

> [!success]- Solution
> All digits to the right of the leftmost mismatch bit are all zero in the answer. The other piece of the answer is just the common left half.

> [!example]- Code
> ```cpp
> class Solution {
> public:
>     int rangeBitwiseAnd(int left, int right) {
>         int temp = left ^ right;
>         int suffix_len=0;
>         while(temp){
>             temp>>=1;
>             suffix_len+=1;
>         }
>         return (left>>suffix_len)<<suffix_len;
>             
>     }
> };
> ```


### Find Optimum Operation
[Link](https://practice.geeksforgeeks.org/problems/find-optimum-operation4504/1)

Given a number N. Find the minimum number of operations required to reach N starting from 0. You have 2 operations available:

- Double the number

- Add one to the number

> [!success]- Solution
> This is basically just constructing a binary represtation of N. it costs 1 unit to create each bit, it costs 1 unit to flip the zeroes to ones. 

> [!example]- Code
> ```cpp
> class Solution {
>   public:
>     int minOperation(int n) {
>         int c=0;
>         while(n){
>             c+=(n&1)+1;
>             n>>=1;
>         }
>         return c-1;
>     }
> };
> ```

### Shortest Subarray With OR at Least K II
[Link](https://leetcode.com/problems/shortest-subarray-with-or-at-least-k-ii/)

Given an array nums of non-negative integers and an integer k, return the length of the shortest non-empty subarray whose bitwise OR is at least k, or -1 if there is none.

>[!hint]- Hint
>Think of solving the same problem, but for sum of the elements adding to k, assuming all positive elements in the list. Then you can follow the same strategy for this, but you'll need some clever bit tracking to subtract a bitwise or.

> [!example]- Code
> ```python
> import math
>
> class Solution:
>     def minimumSubarrayLength(self, nums: List[int], k: int) -> int:
>         win_or = 0
>         left,right = 0,0
>         win_or_bits = [0 for i in range(32)]
>         best = math.inf
>
>         while right < len(nums):
>             new = nums[right]
>             win_or |= new
>
>             bit=0
>             while new:
>                 win_or_bits[bit]+=new&1
>                 new>>=1
>                 bit+=1
>
>             while left<=right and win_or >= k:
>                 best = min(best,right-left+1)
>
>                 old = nums[left]
>                 bit=0
>                 while old:
>                     win_or_bits[bit]-=old&1
>                     if win_or_bits[bit] == 0:
>                         win_or &= ~(1<<bit)
>                     old>>=1
>                     bit+=1
>
>                 left+=1
>
>             right+=1
>
>         return -1 if best == math.inf else best
> ```
