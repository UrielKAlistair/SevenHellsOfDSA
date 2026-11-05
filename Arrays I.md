# Screwing with Arrays 

## Part 1 : Pointers

### Remove Element
[Link](https://leetcode.com/problems/remove-element/)

Given an integer array nums and an integer val, remove all occurrences of val in nums [in-place](https://en.wikipedia.org/wiki/In-place_algorithm). The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

>[!tip]- Hint
>
> Snowball

> [!success]- Solution
> 
> The silly thing to do, would be go and erase every occurrence of val in nums using the normal remove function. This is actually accepted by leetcode, and might even finish in “0 ms”. The reason it’s silly is that such removal is O(n) per removal: it needs to go and shift all successive elements to fill the gap created by deletion.
> 
> This question is actually equivalent to the next one, where val is just set to be zero always. You can just pop the pile at the end after moving them all there, and the pops are a bunch of constant time operations after a single O(n) pass.

> [!example]- Code
> ```cpp
> class Solution {
> public:
>     int removeElement(vector<int>& nums, int val) {
>         int win_size=0;
>         for(int i=0;i<nums.size();i++){
>             if(nums[i]==val){
>                 win_size++;
>             }
>             else{
>                 swap(nums[i],nums[i-win_size]);
>             }
>         }
>
>         for(int i=0;i<win_size;i++){
>             nums.pop_back();
>         }
>         return nums.size();
>     }
> };
> ```

### Move Zeroes
[Link](https://leetcode.com/problems/move-zeroes/)

Given an integer array nums, move all the 0s to the end while keeping the relative order of the non-zero elements. Do it in-place, without making a copy of the array.

> [!success]- Solution
> 
> This is a rather clever solution to encounter for the first time. Simply snowball,  i.e gradually accumulate the zeroes at the end as you move through the list. 
> 
> [0,1,0,3,12]  [1,0,0,3,12]  [1,0,0,3,12]  [1,3,0,0,12]  [1,3,12,0,0]
>
> The “snowball” of zeroes just keeps growing bigger as it rolls down the list.
>
> Only the first zero of the window and the incoming element are swapped; if it's a zero, the window grows instead. At worst, this causes n swaps, so it's O(n)
> 
> To implement, keep track of the firstZero position and swap incoming non zero numbers with it. (Or keep track of the pile size instead, which is more elegant.)

> [!example]- Code
> ```python
> class Solution:
>     def moveZeroes(self, nums: List[int]) -> None:
>        pile_size = 0
>        for i in range(len(nums)):
>            if nums[i] != 0:
>                nums[i],nums[i-pile_size] = nums[i-pile_size], nums[i]
>            else:
>                pile_size+=1
> ```

### Two Sum
[Link](https://leetcode.com/problems/two-sum/)

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

> [!success]- Solution
> It’s a very simple application of a hashmap.

> [!example]- Code
> > [!example]- C++
> > ```cpp
> > class Solution {
> > public:
> >     vector<int> twoSum(vector<int>& nums, int target) {
> >         unordered_map<int, int> umap;
> >         vector<int> ans{-1,-1};
> >         for(int i=0;i<nums.size();i++){
> >             umap[nums.at(i)]=i;
> >         }
> >         for(int i=0;i<nums.size();i++){
> >             int temp = target-nums.at(i);
> >             if (!(umap.find(temp) == umap.end())) {
> >                 if(umap[temp]==i) continue;
> >                 ans[0]=i; ans[1]=umap[temp];
> >                 return ans;
> >             }}
> >         return ans;
> >     }
> > };
> > ```
>
> > [!example]- Python
> > ```python
> > class Solution:
> >     def twoSum(self, nums: list[int], target: int) -> list[int]:
> >         my_map = {}
> >         for i in range(len(nums)):
> >             if target-nums[i] in my_map:
> >                 return [my_map[target-nums[i]],i]
> >             else:
> >                 my_map[nums[i]] = i
> > ```

### Two Sum II
[Link](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

Given a sorted array of integers nums and an integer target, return the indices of the two numbers such that they add up to target.

> [!success]- Solution
>
> This is done easiest with “two pointers”, one on either end of the array. If the target is less than the sum of these two, increase the sum by incrementing the left pointer. If it’s greater, push the right pointer down. Eventually, we will reach the answer.

> [!example]- Code
>
> ```python
> class Solution:
>     def twoSum(self, nums: List[int], target: int) -> bool:
>         left,right = 0,len(nums)-1
>         while left < right:
>             curr_sum = nums[right]+nums[left]
>             if curr_sum==target:
>                 return True
>             elif curr_sum<target:
>                 left+=1
>             else:
>                 right-=1
>         return False
> ```

### Max Number of K-Sum Pairs
[Link](https://leetcode.com/problems/max-number-of-k-sum-pairs)

You are given an integer array nums and an integer k.

In one operation, you can pick two numbers from the array whose sum equals k and remove them from the array.

Return the maximum number of operations you can perform on the array.

> [!success]- Solution
>
> Sorting the array will cost O(nlogn), and then getting every pair possible is O(n)
> 
> Using the hashmap will be O(n), but will cost some minor extra memory. Doesn’t matter.

> [!example]- Code
> > [!example]- C++
> > ```cpp
> > class Solution {
> > public:
> >     int maxOperations(vector<int>& nums, int k) {
> >         unordered_map<int,int> umap;
> >         int ans=0;
> >         for(auto i : nums){
> >             if (umap[k-i]>0){
> >                 umap[k-i]--;
> >                 ans++;
> >             }
> >             else if (!(umap.find(i) == umap.end())){
> >                 umap[i]++;
> >             }
> >             else{
> >                 umap[i]=1;
> >             }
> >         }
> >         return ans;
> >     }
> > };
> > ```
>
> > [!example]- Python
> > ```python
> > class Solution:
> >     def maxOperations(self, nums: List[int], k: int) -> int:
> >         freq = {}
> >         answer=0
> >         for num in nums:
> >             if freq.get(k-num,0):
> >                 freq[k-num]-=1
> >                 answer+=1
> >             else:
> >                 freq[num] = freq.get(num,0)+1
> >         return answer
> > ```

### 3Sum
[Link](https://leetcode.com/problems/3sum/)

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i!=j, i!=k, and j!=k, and nums[i] + nums[j] + nums[k] == 0.

Note that the solution set must not contain duplicate triplets that differ only in order.

> [!success]- Solution
> A hash table can be made with the array. We can iterate through the array and get all possible pairs. The negative of their sum can be looked up in the hash table. That allows us to get all triplets.
>
> O(n) making hash, O(n<sup>2</sup>) to look up pairs.
>
> This needs a little more work to eliminate duplicates: When a,b find c, a,c will find b and b,c will find a. As a consequence, some careful tracking needs to be done to not pick up duplicates.
>
> Alternatively, we can sort the array in O(nlogn) and set the negative of each number as the target, and use two pointers to arrive at the pair corresponding to it. We will only look to the right of the target, since triplets containing the ones to the left would’ve already been found.
>
> Overall, that would cost O(n<sup>2</sup>). I still like my hash solution, though.

> [!example]- Code
> ```cpp
> class Solution {
> public:
>     vector<vector<int>> threeSum(vector<int>& nums) {
>         map<int,int> m;
>         vector<vector<int>> ans;
>
>         for(int i=0;i<nums.size();i++){
>             m[nums[i]]++;
>         }
>
>         // edge case: 2 zeroes
>         if(m.find(0) != m.end()){
>             if(m[0]==2){
>                 m[0]=1;
>             }
>         }
>
>         for(auto i = m.begin(); i != m.end(); i++){
>             auto j = i;
>             j++;
>
>             if(i->second>1){
>                 j--;
>             }
>
>             // edge case: 3 zeroes
>             if((i->first==0)&&(i->second>=3)){
>                 ans.push_back(vector<int> {0,0,0});
>                 j++;
>             }
>
>             vector<int> done {};
>
>             start:
>             while(j!=m.end()){
>
>                 // removing duplicates
>                 for(auto k : done){
>                     if(k==j->first){
>                         j++;
>                         goto start;
>                     }
>                 }
>
>                 if(m.find(-(i->first+j->first)) != m.end()){
>
>                     if(m[-(i->first+j->first)]==0){
>                         j++;
>                         continue;
>                     }
>
>                     if((-(i->first+j->first)==i->first)||(-(i->first+j->first)==j->first)){
>                         if(m[-(i->first+j->first)]<2){
>                             j++;
>                             continue;
>                         }
>                     }
>
>                     vector<int> temp {i->first,j->first,-(i->first+j->first)};
>                     done.push_back(-(i->first+j->first));
>                     ans.push_back(temp);
>                 }
>                 j++;
>             }
>
>             m[i->first]=0;
>         }
>         return ans;
>     }
> };
> ```


### Container With Most Water
[Link](https://leetcode.com/problems/container-with-most-water/)

Given an array height, where height[i] is the height of a vertical line at position i, pick two lines that together with the x-axis form a container holding the most water. Return that maximum amount of water.

> [!example]- Code
> ```python
> class Solution:
>     def max_area(self, heights: List[int]) -> int:
>         left = 0
>         right = len(heights)-1
>         _max_area = 0
>         while left<=right:
>             _max_area = max(_max_area, (right-left)*min(heights[left],heights[right]))
>             if heights[left]<heights[right]:
>                 left+=1
>             else:
>                 right-=1
>         return _max_area
> ```

### Trapping Rain Water
[Link](https://leetcode.com/problems/trapping-rain-water/)

Given n non-negative integers representing an elevation map where each bar has width 1, compute how much water it can trap after it rains.

> [!success]- Solution
> I initially made 2 lists, and solved in 3 loops. O(n).
> This can be done in one loop same O(n) but I dont like the commonly prescribed solution, it's too disgusting.
> 
> I made a list of tallest from right, and one for tallest from left. I then noticed that I can very easily make the left tallest an in place computation and elimate one list and finish this in 2 loops.

> [!example]- Code
> ```python
> class Solution:
>     def trap(self, height: list[int]) -> int:
>          left_tallest = 0
>          right_tallest = [0 for i in range(len(height))]
>          water_sum = 0
>
>          for i in range(len(height)-2,-1,-1):
>              right_tallest[i] = max(right_tallest[i+1], height[i+1])
>
>          for i in range(1,len(height)):
>             left_tallest = max(left_tallest, height[i-1])
>             water_sum += max(0,min(right_tallest[i],left_tallest) - height[i])
>
>          return water_sum
> ```

### Sort Colours
[Link](https://leetcode.com/problems/sort-colors/)

Given an array nums of n objects coloured red, white or blue (as 0, 1 and 2), sort it in-place so that objects of the same colour are adjacent, in the order red, white, blue. You may not use the library sort.

> [!success]- Solution
>  I sort of scammed this with a double Snowball. I ran the [[#Move Zeroes]] algorithm Twice.
>  This can be actually be done in one loop, with a two pointer approach, you just need to swap the zeroes to the front and the twos to the back. 
>  
>  The only catch is that you need to be careful about not iterating into the 2 pile, and carefully putting what you obtain from the top of the two pile to where it belongs.

> [!example]- Code: Double snowball
> ```python
> class Solution:
>    def sortColors(self, nums: list[int]) -> None:
>        """
>        Do not return anything, modify nums in-place instead.
>        """
>        for i in (1,2):
>            self.snowball(nums,i)
>        return nums
>    
>    def snowball(self, nums: List[int], val) -> List[int]:
>        pile_size = 0
>        for i in range(len(nums)):
>            if nums[i] != val:
>                nums[i],nums[i-pile_size] = nums[i-pile_size], nums[i]
>            else:
>                pile_size+=1
> ```

> [!example]- Code: Two pointer
> ```python
> class Solution:
>     def sortColors(self, nums: list[int]) -> None:
>         """
>         Do not return anything, modify nums in-place instead.
>         """
>         zero_pile = 0
>         two_pile = len(nums)-1
>         i=0
>         while i <= two_pile:
>             if nums[i] == 0: 
>                 nums[i], nums[zero_pile] = nums[zero_pile], nums[i]
>                 zero_pile+=1
>             if nums[i] == 2:
>                 nums[i], nums[two_pile] = nums[two_pile], nums[i]
>                 two_pile-=1  
>             else:
>                 i+=1
>         return nums
> ```


### Valid Triangle Number
[Link](https://leetcode.com/problems/valid-triangle-number/)

Given an integer array nums, return the number of triplets chosen from it that can be the side lengths of a triangle.

**My remarks (Oct 2026):** Very clever solution. Could not get it. Worth trying again even if you remember the idea, because the code requires you to not fumble a lot of pointers.

> [!example]- Solution 1
> ```python
> class Solution:
>     def triangleNumber(self, nums: list[int]) -> int:
>         nums.sort()
>         answer = 0 
>         for c in range(2,len(nums)):
>             a=0
>             for b in range(c-1,0,-1):    
>                 while a<b and nums[a]+nums[b]<=nums[c]:
>                     a+=1
>                 if a==b:
>                     break
>                 answer+=b-a
>                 
>         return answer
> ```

> [!example]- Solution 2
> ```python
> class Solution:
>     def triangleNumber(self, nums: list[int]) -> int:
>         nums.sort()
>         answer = 0 
>         for a in range(len(nums)-2):
>             c=a+2
>             for b in range(a+1,len(nums)-1):
>                 c = max(c, b+1)
>                 while c<len(nums) and nums[a]+nums[b]>nums[c]:
>                     c+=1
>                 answer+=c-b-1
>                 
>         return answer
> ```

