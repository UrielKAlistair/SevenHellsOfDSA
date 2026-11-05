### Climbing Stairs
[Link](https://leetcode.com/problems/climbing-stairs/)

You are climbing a staircase. There are n steps.

With each operation, you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

>[!tip]- Hint
> Fibonacci
 
> [!success]- Solution
>
> You can reach stair n either by climbing one step from the (n-1)<sup>th</sup> step, or by climbing 2 steps from the (n-2)<sup>th</sup> step.
>
> Thus, P(n)=P(n-1)+P(n-2)
>
> Base case: P(1)=1, P(2)=2

### Domino Tiling of a 2 x n Board

![[Dynamic Programming - image1.png]]
Given an integer n, return the number of ways to tile a 2 x n board using dominos that can cover 2 tiles. Example is shown for a 2 x 3 board.

>[!tip]- Hint
>Fibonacci

> [!success]- Solution> 
>
> Consider the top left dot. We must place our first tile. There are two possibilities, placing it vertically, like in figure 1, or horizontally, like in figure 3.
>
> If we place it vertically, we have reduced the problem to one of size n-1.
>
> If we place it horizontally, the two dots below must mandatorily be covered by another horizontal tile, thus reducing the problem to one of size n-2.
>
> Thus, P(n) = P(n-1) + P(n-2)
>
> Base case:
>
> P(1) = 1, P(2) = 2

### Domino and Tromino Tiling
[Link](https://leetcode.com/problems/domino-and-tromino-tiling/)

You are given 2 types of tiles, as shown. You may rotate these, just as in the previous problem.

Given an integer n, return the number of ways to tile a 2 x n board. Since the answer may be very large, return it modulo 10<sup>9</sup> + 7.

![[Dynamic Programming - image2.jpeg]]

> [!success]- Solution
> 
> As in the previous problem, we begin by placing some tile to cover the top left dot. There are now 4 possibilities: The two considered previously, and then these two.
>
> ![[Dynamic Programming - image3.png]]
>
> By symmetry, both of these leave an identical remainder portion when we look at no of ways of tiling. Let’s call the problem of tiling a 2 x n board, with one extra tile jutting out on the side Q(n).
>
> Thus, P(n) = P(n-1) + P(n-2) + 2 Q(n-2)
>
> Now, look at Q(n).
>
> That one extra tile has now two choices. One: place a horizontal domino; Two: place the complementary tromino, like in both the figures used here.
>
> Q(n) = Q(n-1) + P(n-1)
>
> Now, a bit of algebra.
>
> Q(n-1) = Q(n-2) + P(n-2) and so on.
>
> Q(n) = P(n-1) + P(n-2) + P(n-3) + … P(1) + Q(1)
>
> Base Case: Q(1) = 1
>
> $\boxed{Q(n) = 1+\sum_{1}^{n-1} P\left(i\right)}$
>
> Thus, $P(n) = P(n-1) + P(n-2) + 2 * (1 +\sum_{1}^{n-3} P\left(i\right))$
>
> This in itself is a solution. Base case: P(1) = 1 P(2) = 2
>
> Doing the sum of P(i) for each n is wasteful, so you should maintain a running sum variable that allows you to reuse the sum, improving the complexity to O(n) from O(n<sup>2</sup>) of direct implementation.
>
> We’ve pretty much fully solved the problem, but there is a small elegance upgrade we can do to this.
>
> $P(n) = P(n-1) +  \boxed{P(n-2) + 2 * (1 +\sum_{1}^{n-3} P\left(i\right))}$
>
> $P(n-1) = P(n-2) + P(n-3) + 2 * (1 +\sum_{1}^{n-4} P\left(i\right))$
>
> Observe that P(n-1) + P(n-3) is the term inside the box in P(n).
>
> Thus, $\boxed{P(n) = 2 P(n-1) + P(n-3)}$

> [!example]- Code
> ```cpp
> class Solution {
> public:
>     int numTilings(int n) {
>         // P(n)= 2P(n-1) + P(n-3)
>         if (n==1) return 1;
>         if (n==2) return 2;
>         long int prev = 2;
>         long int two_back = 1;
>         long int three_back = 1;
>         long int ans;
>         for (int i=3; i<=n;i++){
>             ans = (2 * prev + three_back) % (long int)(1e9 + 7);
>             three_back = two_back;
>             two_back = prev;
>             prev = ans;
>         }
>         return ans;
>     }
> };
> ```
>

