# Introduction

In this explore card, we're going to go over the basics of **DP**, provide you with a framework for solving DP problems, learn about common patterns, and walk through many examples.

No prior knowledge of DP is required for this card, but you will need a solid understanding of recursion, a basic understanding of what greedy algorithms are, and general knowledge such as big O and hash tables. If you aren't familiar with recursion yet, check out the [recursion explore card](https://leetcode.com/explore/learn/card/recursion-i/) first.

# Intro to Dynamic Programming

## What is Dynamic Programming?


**Dynamic Programming** (DP) is a programming paradigm that can systematically and efficiently explore all possible solutions to a problem. As such, it is capable of solving a wide variety of problems that often have the following characteristics:

1. The problem can be broken down into "overlapping subproblems" - smaller versions of the original problem that are re-used multiple times.
2. The problem has an "optimal substructure" - an optimal solution can be formed from optimal solutions to the overlapping subproblems of the original problem.

As a beginner, these theoretical definitions may be hard to wrap your head around. Don't worry though - at the end of this chapter, we'll talk about how to practically spot when DP is applicable. For now, let's look a little deeper at both characteristics.

The [Fibonacci sequence](https://en.wikipedia.org/wiki/Fibonacci_number) is a classic example used to explain DP. For those who are unfamiliar with the Fibonacci sequence, it is a sequence of numbers that starts with 0, 1, and each subsequent number is obtained by adding the previous two numbers together.

If you wanted to find the $n^{th}$ Fibonacci number $F(n)$, you can break it down into smaller **subproblems** - find $F(n - 1)$ and $F(n - 2)$ instead. Then, adding the solutions to these subproblems together gives the answer to the original question, $F(n - 1) + F(n - 2) = F(n)$, which means the problem has **optimal substructure**, since a solution $F(n)$ to the original problem can be formed from the solutions to the subproblems. These subproblems are also **overlapping** - for example, we would need $F(4)$ to calculate both $F(5)$ and $F(6)$.

These attributes may seem familiar to you. Greedy problems have optimal substructure, but not overlapping subproblems. Divide and conquer algorithms break a problem into subproblems, but these subproblems are not **overlapping** (which is why DP and divide and conquer are commonly mistaken for one another).

Dynamic programming is a powerful tool because it can break a complex problem into manageable subproblems, avoid unnecessary recalculation of overlapping subproblems, and use the results of those subproblems to solve the initial complex problem. DP not only aids us in solving complex problems, but it also greatly improves the time complexity compared to brute force solutions. For example, the brute force solution for calculating the Fibonacci sequence has exponential time complexity, while the dynamic programming solution will have linear time complexity. Throughout this explore card, you will gain a better understanding of what makes DP so powerful. In the next section, we'll discuss the two main methods of implementing a DP algorithm.



##  Top-down and Bottom-up


There are two ways to implement a DP algorithm:

1. Bottom-up, also known as tabulation.
2. Top-down, also known as memoization.

Let's take a quick look at each method.



### Bottom-up (Tabulation)

Bottom-up is implemented with iteration and starts at the base cases. Let's use the Fibonacci sequence as an example again. The base cases for the Fibonacci sequence are $F(0)=0$ and $F(1)=1$. With bottom-up, we would use these base cases to calculate $F(2)$, and then use that result to calculate F(3)*F*(3), and so on all the way up to $F(n)$.

```
// Pseudocode example for bottom-up

F = array of length (n + 1)
F[0] = 0
F[1] = 1
for i from 2 to n:
    F[i] = F[i - 1] + F[i - 2]
```



### Top-down (Memoization)

Top-down is implemented with recursion and made efficient with memoization. If we wanted to find the $n^{th}$ Fibonacci number $F(n)$, we try to compute this by finding $F(n - 1)$ and $F(n - 2)$. This defines a recursive pattern that will continue on until we reach the base cases $F(0) = F(1) = 1$. The problem with just implementing it recursively is that there is a ton of unnecessary repeated computation. Take a look at the recursion tree if we were to find $F(5)$:



![img](Dynamic%20Programming.assets/C1A2_1.png)





Notice that we need to calculate $F(2)$ three times. This might not seem like a big deal, but if we were to calculate $F(6)$, this **entire image** would be only one child of the root. Imagine if we wanted to find $F(100)$ - the amount of computation is exponential and will quickly explode. The solution to this is to **memoize** results.

> **memoizing** a result means to store the result of a function call, usually in a hashmap or an array, so that when the same function call is made again, we can simply return the **memoized** result instead of recalculating the result.

After we calculate $F(2)$, let's store it somewhere (typically in a hashmap), so in the future, whenever we need to find $F(2)$, we can just refer to the value we already calculated instead of having to go through the entire tree again. Below is an example of what the recursion tree for finding $F(6)$ looks like with and without memoization:



![img](Dynamic%20Programming.assets/54033823-880e-44a1-9cdd-0dc8ac8ca060.png)

![3788f77e-2257-4021-a124-df70bf1c9de1](Dynamic%20Programming.assets/3788f77e-2257-4021-a124-df70bf1c9de1.png)

```
// Pseudocode example for top-down

memo = hashmap
Function F(integer i):
    if i is 0 or 1: 
        return i
    if i doesn't exist in memo:
        memo[i] = F(i - 1) + F(i - 2)
    return memo[i]
```



### Which is better?

Any DP algorithm can be implemented with either method, and there are reasons for choosing either over the other. However, each method has one main advantage that stands out:

- A bottom-up implementation's runtime is usually faster, as iteration does not have the overhead that recursion does.
- A top-down implementation is usually much easier to write. This is because with recursion, the ordering of subproblems does not matter, whereas with tabulation, we need to go through a logical ordering of solving subproblems.

> We'll be talking more about these two options throughout the card. For now, all you need to know is that top-down uses recursion, and bottom-up uses iteration.



##  When to Use DP



When it comes to solving an algorithm problem, especially in a high-pressure scenario such as an interview, half the battle is figuring out how to even approach the problem. In the first section, we defined what makes a problem a good candidate for dynamic programming. Recall:

1. The problem can be broken down into "overlapping subproblems" - smaller versions of the original problem that are re-used multiple times
2. The problem has an "optimal substructure" - an optimal solution can be formed from optimal solutions to the overlapping subproblems of the original problem

Unfortunately, it is hard to identify when a problem fits into these definitions. Instead, let's discuss some common characteristics of DP problems that are easy to identify.

**The first characteristic** that is common in DP problems is that the problem will ask for the optimum value (maximum or minimum) of something, or the number of ways there are to do something. For example:

- What is the minimum cost of doing...
- What is the maximum profit from...
- How many ways are there to do...
- What is the longest possible...
- Is it possible to reach a certain point...

> **Note:** Not all DP problems follow this format, and not all problems that follow these formats should be solved using DP. However, these formats are very common for DP problems and are generally a hint that you should consider using dynamic programming.

When it comes to identifying if a problem should be solved with DP, this first characteristic is not sufficient. Sometimes, a problem in this format (asking for the max/min/longest etc.) is meant to be solved with a greedy algorithm. The next characteristic will help us determine whether a problem should be solved using a greedy algorithm or dynamic programming.

**The second characteristic** that is common in DP problems is that future "decisions" depend on earlier decisions. Deciding to do something at one step may affect the ability to do something in a later step. This characteristic is what makes a greedy algorithm invalid for a DP problem - we need to factor in results from previous decisions. Admittedly, this characteristic is not as well defined as the first one, and the best way to identify it is to go through some examples.

[House Robber](https://leetcode.com/problems/house-robber/) is an excellent example of a dynamic programming problem. The problem description is:

> You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.
>
> Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

In this problem, each decision will affect what options are available to the robber in the future. For example, with the test case $nums = [2, 7, 9, 3, 1]$, the optimal solution is to rob the houses with $2$, $9$, and $1$ money. However, if we were to iterate from left to right in a greedy manner, our first decision would be whether to rob the first or second house. 7 is way more money than 2, so if we were greedy, we would choose to rob house 7. However, this prevents us from robbing the house with 9 money. As you can see, our decision between robbing the first or second house affects which options are available for future decisions.

[Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) is another example of a classic dynamic programming problem. In this problem, we need to determine the length of the longest (first characteristic) subsequence that is strictly increasing. For example, if we had the input $nums = [1, 2, 6, 3, 5]$, the answer would be 4, from the subsequence $[1, 2, 3, 5]$. Again, the important decision comes when we arrive at the 6 - do we take it or not take it? If we decide to take it, then we get to increase our current length by 1, but it affects the future - we can no longer take the 3 or 5. Of course, with such a small example, it's easy to see why we shouldn't take it - but how are we supposed to design an algorithm that can always make the correct decision with huge inputs? Imagine if nums contained 10,00010,000 numbers instead.

> When you're solving a problem on your own and trying to decide if the second characteristic is applicable, assume it isn't, then try to think of a counterexample that proves a greedy algorithm won't work. If you can think of an example where earlier decisions affect future decisions, then DP is applicable.

To summarize: if a problem is asking for the maximum/minimum/longest/shortest of something, the number of ways to do something, or if it is possible to reach a certain point, it is probably greedy or DP. With time and practice, it will become easier to identify which is the better approach for a given problem. Although, in general, if the problem has constraints that cause decisions to affect other decisions, such as using one element prevents the usage of other elements, then we should consider using dynamic programming to solve the problem. **These two characteristics can be used to identify if a problem should be solved with DP.**

> Note: these characteristics should only be used as guidelines - while they are extremely common in DP problems, at the end of the day DP is a very broad topic.



##  Chapter 1 Quiz

Multiple Choice Question

(Select all that apply.) Dynamic programming is used for problems with:

Optimal substructure

Non-overlapping subproblems

Overlapping subproblems

> Problems that have optimal substructure and overlapping subproblems are good problems to be solved with dynamic programming.



------



Single Choice Question

Without memoization, a recursive solution for Fibonacci Number would have the time complexity:

O(n)

O(log n)

O(2^n)

O(n^2)

> Each call to F(n) makes 2 additional calls, to F(n - 1) and F(n - 2). Those 2 calls will then generate 4 calls, which will generate 8, etc.



------



Single Choice Question

Usually, top-down algorithms are faster than bottom-up ones.

True

False

> Usually, a bottom-up algorithm will be faster than the equivalent top-down algorithm as there is some overhead with recursive function calls. However, sometimes, a top-down dynamic programming approach will be the better option, such as when only a fraction of the subproblems need to be solved.



------



Single Choice Question

A common characteristic of problems that can be solved with dynamic programming is that they ask for the maximum or minimum of something.

True

False

> While this is true, not all problems with this characteristic should be solved with dynamic programming.



------



Single Choice Question

_ approaches can be parallelized while ___ approaches cannot.

(Dynamic programming), (divide and conquer)

(Divide and conquer), (dynamic programming)

Neither can be parallelized.

> Strictly speaking, both can be parallelized, however the steps required to parallelize dynamic programming approaches are quite complex. So generally speaking, divide and conquer approaches can be parallelized while dynamic programming approaches cannot be (easily) parallelized. This is because the subproblems in divide an conquer approaches are independent of one another (they do not overlap) while in dynamic programming, the subproblems do overlap.



------



Single Choice Question

_ dynamic programming solutions make recursive calls according to the recurrence relation while ___ dynamic programming solutions strategically iterate over each state.

(Top-down), (bottom-up)

(Bottom-up), (top-down)

> Top-down DP solutions are generally considered more intuitive because we can simply make recursive calls according to the recurrence relation whenever the current state is not a base case. On the other hand, bottom-up DP solutions require us to iterate over the subproblems in a specific order so that all of the results necessary to solve the subproblem for the current state have already been obtained before arriving at the current state.



------



Single Choice Question

Which of the following must be done when converting a top-down dynamic programming solution into a bottom-up dynamic programming solution?

Change plus to minus in the recurrence relation.

Create new base cases.

Find the correct order in which to iterate over the states.

> When converting a top-down solution to a bottom-up solution, we can still use the same base case(s) and the same recurrence relation. However, bottom-up dynamic programming solutions iterate over all of the states (starting at the base case) such that all the results necessary to solve each subproblem for the current state have already been obtained before arriving at the current state. So, to convert a top-down solution into a bottom-up solution, we must first find a logical order in which to iterate over the states.




# Strategic Approach to DP

##  Framework for DP Problems



Now that we understand the basics of DP and how to spot when DP is applicable to a problem, we've reached the most important part: actually solving the problem. In this section, we're going to talk about a framework for solving DP problems. This framework is applicable to nearly every DP problem and provides a clear step-by-step approach to developing DP algorithms.

> For this article's explanation, we're going to use the problem [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) as an example, with a top-down (recursive) implementation. Take a moment to read the problem description and understand what the problem is asking.

Before we start, we need to first define a term: **state**. In a DP problem, a **state** is a set of variables that can sufficiently describe a scenario. These variables are called **state variables**, and we only care about relevant ones. For example, to describe every scenario in Climbing Stairs, there is only 1 relevant state variable, the current step we are on. We can denote this with an integer $i$. If $i = 6$, that means that we are describing the state of being on the 6th step. Every unique value of $i$ represents a unique **state**.

> You might be wondering what "relevant" means here. Picture this problem in real life: you are on a set of stairs, and you want to know how many ways there are to climb to say, the 10th step. We're definitely interested in what step you're currently standing on. However, we aren't interested in what color your socks are. You could certainly include sock color as a state variable. Standing on the 8th step wearing green socks is a different state than standing on the 8th step wearing red socks. However, changing the color of your socks will not change the number of ways to reach the 10th step from your current position. Thus the color of your socks is an **irrelevant** variable. In terms of figuring out how many ways there are to climb the set of stairs, the only **relevant** variable is what stair you are currently on.



### The Framework

To solve a DP problem, we need to combine 3 things:

1. **A function or data structure that will compute/contain the answer to the problem for every given state**.

   For Climbing Stairs, let's say we have an function $dp$ where $dp(i)$ returns the number of ways to climb to the $i^{th}$ step. Solving the original problem would be as easy as $return$ $dp(n)$.

   How did we decide on the design of the function? The problem is asking "How many distinct ways can you climb to the top?", so we decide that the function will represent how many distinct ways you can climb to a certain step - literally the original problem, but generalized for a given state.

   > Typically, top-down is implemented with a recursive function and hash map, whereas bottom-up is implemented with nested for loops and an array. When designing this function or array, we also need to decide on state variables to pass as arguments. This problem is very simple, so all we need to describe a state is to know what step we are currently on $i$. We'll see later that other problems have more complex states.

2. **A recurrence relation to transition between states.**

   A recurrence relation is an equation that relates different states with each other. Let's say that we needed to find how many ways we can climb to the 30th stair. Well, the problem states that we are allowed to take either 1 or 2 steps at a time. Logically, that means to climb to the 30th stair, we arrived from either the 28th or 29th stair. Therefore, the number of ways we can climb to the 30th stair is equal to the number of ways we can climb to the 28th stair plus the number of ways we can climb to the 29th stair.

   The problem is, we don't know how many ways there are to climb to the 28th or 29th stair. However, we can use the logic from above to define a recurrence relation. In this case, $dp(i) = dp(i - 1) + dp(i - 2)$. As you can see, information about some states gives us information about other states.

   > Upon careful inspection, we can see that this problem is actually the Fibonacci sequence in disguise! This is a very simple recurrence relation - typically, finding the recurrence relation is the most difficult part of solving a DP problem. We'll see later how some recurrence relations are much more complicated, and talk through how to derive them.

3. **Base cases, so that our recurrence relation doesn't go on infinitely.**

   The equation $dp(i) = dp(i - 1) + dp(i - 2)$ on its own will continue forever to negative infinity. We need base cases so that the function will eventually return an actual number.

   Finding the base cases is often the easiest part of solving a DP problem, and just involves a little bit of logical thinking. When coming up with the base case(s) ask yourself: What state(s) can I find the answer to without using dynamic programming? In this example, we can reason that there is only 1 way to climb to the first stair (1 step once), and there are 2 ways to climb to the second stair (1 step twice and 2 steps once). Therefore, our base cases are $dp(1) = 1$ and $dp(2) = 2$.

   > We said above that we don't know how many ways there are to climb to the 28th and 29th stairs. However, using these base cases and the recurrence relation from step 2, we can figure out how many ways there are to climb to the 3rd stair. With that information, we can find out how many ways there are to climb to the 4th stair, and so on. Eventually, we will know how many ways there are to climb to the 28th and 29th stairs.



### Example Implementations

Here is a basic top-down implementation using the 3 components from the framework:

~~~
class Solution {
    // A function that represents the answer to the problem for a given state
    private int dp(int i) {
        if (i <= 2) return i; // Base cases
        return dp(i - 1) + dp(i - 2); // Recurrence relation
    }
    
    public int climbStairs(int n) {
        return dp(n);
    }
}
~~~

Do you notice something missing from the code? We haven't memoized anything! The code above has a time complexity of $O(2^n)$ because every call to $dp$ creates 2 more calls to $dp$. If we wanted to find how many ways there are to climb to the 250th step, the number of operations we would have to do is approximately equal to the number of atoms in the universe.

In fact, without the memoization, this isn't actually dynamic programming - it's just basic recursion. Only after we optimize our solution by adding memoization to avoid repeated computations can it be called DP. As explained in chapter 1, memoization means caching results from function calls and then referring to those results in the future instead of recalculating them. This is usually done with a hashmap or an array.

~~~
class Solution {
    private HashMap<Integer, Integer> memo = new HashMap<>();
    
    private int dp(int i) {
        if (i <= 2) return i;
        // Instead of just returning dp(i - 1) + dp(i - 2), calculate it once and then
        // store it inside a hashmap to refer to in the future
        if (!memo.containsKey(i)) {
            memo.put(i, dp(i - 1) + dp(i - 2));
        }
        
        return memo.get(i);
    }
    
    public int climbStairs(int n) {
        return dp(n);
    }
}
~~~

With memoization, our time complexity drops to $O(n)$ - astronomically better, literally.

> You may notice that a hashmap is overkill for caching here, and an array can be used instead. This is true, but using a hashmap isn't necessarily bad practice as some DP problems will require one, and they're hassle-free to use as you don't need to worry about sizing an array correctly. Furthermore, when using top-down DP, some problems do not require us to solve every single subproblem, in which case an array may use more memory than a hashmap.

We just talked a whole lot about top-down, but what about bottom-up? Everything is pretty much the same, except we will start from our base cases and iterate up to our final answer. As stated before, bottom-up implementations usually use an array, so we will use an array $dp$ where $dp[i]$ represents the number of ways to climb to the $i^{th}$ step.

~~~
class Solution {
    public int climbStairs(int n) {
        if (n == 1) return 1;
        
        // An array that represents the answer to the problem for a given state
        int[] dp = new int[n + 1]; 
        dp[1] = 1; // Base cases
        dp[2] = 2; // Base cases
        for (int i = 3; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2]; // Recurrence relation
        }
        
        return dp[n];
    }
}
~~~

> Notice that the implementation still follows the framework exactly - the framework holds for both top-down and bottom-up implementations.



### To Summarize

With DP problems, we can use logical thinking to find the answer to the original problem for certain inputs, in this case we reason that there is 1 way to climb to the first stair and 2 ways to climb to the second stair. We can then use a recurrence relation to find the answer to the original problem for any state, in this case for any stair number. Finding the recurrence relation involves thinking about how moving from one state to another changes the answer to the problem.

This is the essence of dynamic programming. Here's a quick animation for Climbing Stairs:

![img](Dynamic%20Programming.assets/fc0b149c-a1f1-45b1-b6ea-2133cf2c775e.png)
![img](Dynamic%20Programming.assets/08bacf49-7380-47b6-b291-5f337fed6b0e.png)
![img](Dynamic%20Programming.assets/fa2cd69e-b1b5-4b21-a908-f7395cdfeb31.png)
![img](Dynamic%20Programming.assets/669a64b7-62f6-4280-8dcb-8a03a5bd380c.png)
![img](Dynamic%20Programming.assets/3246ba36-b1c6-4463-b17d-c1a01f68af4a.png)
![img](Dynamic%20Programming.assets/e4aecec9-284b-4c7a-a699-a6984fa6d39b.png)



### Next Up

For the rest of the explore card, we're going to use the framework to solve multiple examples, while explaining the thought process behind how to apply the framework at each step. It may be useful to refer back to the section of this article titled "The framework" as you move along the card. For now, take a deep breath - this was a lot to take in, but soon you will be equipped to start solving DP problems on your own.



##  Example 198. House Robber



> This is the first of 6 articles where we will use a framework to work through example DP problems. The framework provides a blueprint to solve DP problems, but when you are just starting to learn DP, deriving some of the logic yourself may be difficult. The objective of these articles is to talk through how to use the framework to work through each problem, and our goal is that, by the end of this, you will be able to independently tackle most DP problems using this framework.

In this article, we will be looking at the [House Robber](https://leetcode.com/problems/house-robber/) problem. In an earlier section of this explore card, we talked about how House Robber fits the characteristics of a DP problem. It's asking for the maximum of something, and our current decisions will affect which options are available for our future decisions. Let's see how we can use the framework to develop an algorithm for this problem.

1. A **function or array** that answers the problem for a given state


First, we need to decide on state variables. As a reminder, state variables should be fully capable of describing a scenario. Imagine if you had this scenario in real life - you're a robber and you have a lineup of houses. If you are at one of the houses, the only variable you would need to describe your situation is an integer - the index of the house you are currently at. Therefore, the only state variable is an integer, say $i$, that indicates the index of a house.

> If the problem had an added constraint such as "*you are only allowed to rob up to k houses*", then $k$ would be another necessary state variable. This is because being at, say house 4 with 3 robberies left is different than being at house 4 with 5 robberies left.
>
> You may be wondering - why don't we include a state variable that is a boolean indicating if we robbed the previous house or not? We certainly could include this state variable, but we can develop our recurrence relation in a way that makes it unnecessary. Building an intuition for this is difficult at first, but it becomes easier with practice.

The problem is asking for "the maximum amount of money you can rob". Therefore, we would use either a function $dp(i)$ that returns the maximum amount of money you can rob up to and including house $i$, or an array $dp$ where $dp[i]$ represents the maximum amount of money you can rob up to and including house $i$.

This means that after all the subproblems have been solved, $dp[i]$ and $dp(i)$ both return the answer to the original problem for the subarray of $nums$ that spans 00 to $i$ inclusive. To solve the original problem, we will just need to return $dp[nums.length - 1]$ or $dp(nums.length - 1)$, depending if we do bottom-up or top-down.

2. A **recurrence relation** to transition between states

> For this part, let's assume we are using a top-down (recursive function) approach. Note that the top-down approach is closer to our natural way of thinking and it is generally easier to think of the recurrence relation if we start with a top-down approach.

Next, we need to find a recurrence relation, which is typically the hardest part of the problem. For any recurrence relation, a good place to start is to think about a general state (in this case, let's say we're at the house at index $i$, and use information from the problem description to think about how other states relate to the current one.

If we are at some house, logically, we have 2 options: we can choose to rob this house, or we can choose to not rob this house.

1. If we decide not to rob the house, then we don't gain any money. Whatever money we had from the previous house is how much money we will have at this house - which is $dp(i - 1)$.
2. If we decide to rob the house, then we gain $nums[i]$ money. However, this is only possible if we did not rob the previous house. This means the money we had when arriving at this house is the money we had from the previous house without robbing it, which would be however much money we had 2 houses ago, $dp(i - 2)$. After robbing the current house, we will have $dp(i - 2) + nums[i]$ money.

From these two options, we always want to pick the one that gives us maximum profits. Putting it together, we have our recurrence relation: $dp(i) = max(dp(i - 1), dp(i - 2) + nums[i]$.

3. **Base cases**

The last thing we need is base cases so that our recurrence relation knows when to stop. The base cases are often found from clues in the problem description or found using logical thinking. In this problem, if there is only one house, then the most money we can make is by robbing the house (the alternative is to not rob the house). If there are only two houses, then the most money we can make is by robbing the house with more money (since we have to choose between them). Therefore, our base cases are:

   1. $dp(0) = nums[0]$
   2. $dp(1) = max( nums[0], nums[1]$)



### Top-down Implementation

Now that we have established all 3 parts of the framework, let's put it together for the final result. Remember: we need to memoize the function!

~~~
class Solution {
    private HashMap<Integer, Integer> memo = new HashMap<Integer, Integer>();
    private int[] nums;
    
    private int dp(int i) {
        // Base cases
        if (i == 0) return nums[0];
        if (i == 1) return Math.max(nums[0], nums[1]);
        if (!memo.containsKey(i)) {
            memo.put(i, Math.max(dp(i - 1), dp(i - 2) + nums[i])); // Recurrence relation
        }
        return memo.get(i);
    }
    
    public int rob(int[] nums) {
        this.nums = nums;
        return dp(nums.length - 1);
    }
}
~~~

### Bottom-up Implementation

Here's the bottom-up approach: everything is the same, except that we use an array instead of a hash map and we iterate using a for-loop instead of using recursion.

~~~
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) return nums[0];
        
        int[] dp = new int[nums.length];
        
        // Base cases
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        
        for (int i = 2; i < nums.length; i++) {
            dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]); // Recurrence relation
        }
        
        return dp[nums.length - 1];
    }
}
~~~

For both implementations, the time and space complexity is $O(n)$. We'll talk about time and space complexity of DP algorithms in depth at the end of this chapter. Here's an animation that shows the algorithm in action:

![img](Dynamic%20Programming.assets/d6a42e7a-bcf6-4fe4-806f-8c632aaa0fc0.png)
![img](Dynamic%20Programming.assets/b6e8934d-766e-447e-a087-11e1579e16f2.png)
![img](Dynamic%20Programming.assets/99c69e27-ff5d-4396-957a-b8a4ac0a8ff6.png)
![img](Dynamic%20Programming.assets/247caec3-5db9-4838-a0e6-bc3fd61f3461.png)

### Up Next

Now that you've seen the framework in action, try solving these problems (located on the next 2 pages) on your own. If you get stuck, come back here for hints:

746. Min Cost Climbing Stairs

Click here to show hint regarding state variables and $dp$
Let $dp(i)$ be the minimum cost necessary to reach step $i$.

Click here to show hint regarding the recurrence relation
We can arrive at step $i$ from either step $i - 1$ or step $i - 2$. Choose whichever one is cheaper.

Click here to show hint regarding base cases
Since we can start from either step 0 or step 1, the cost to reach these steps is 0.

1137. N-th Tribonacci Number

Click here to show hint regarding state variables and $dp$
Let $dp(i)$ represent the $i^{th}$ tribonacci number.

Click here to show hint regarding the recurrence relation
Use the equation given in the problem description.

Click here to show hint regarding base cases
Use the base cases given in the problem description.

740. Delete and Earn

Click here to show hint regarding preprocessing steps
Sort $nums$ and count how many times each number occurs in $nums$.

Click here to show hint regarding state variables and $dp$
Let $dp(i)$ be the maximum number of points you can earn between $i$ and the end of the sorted $nums$ array.

Click here to show hint regarding the recurrence relation
When we are at index $i$ we have 2 options:

Take all numbers that match $nums[i]$ and skip all $nums[i] + 1$.
Do not take $nums[i]$ and move to the first occurrence of $nums[i] + 1$.
Choose whichever option yields the most points.

Bonus Hint: When is the first option guaranteed to be better than the second option?

Click here to show hint regarding base cases
If we have reached the end of the $nums$ array $(i = nums.length)$ then return $0$ because we cannot gain any more points.



##  Coding Question - House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

 

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 2:**

```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

 

**Constraints:**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 400`



## Coding Question - Min Cost Climbing Stairs

You are given an integer array `cost` where `cost[i]` is the cost of `ith` step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index `0`, or the step with index `1`.

Return *the minimum cost to reach the top of the floor*.

 

**Example 1:**

```
Input: cost = [10,15,20]
Output: 15
Explanation: You will start at index 1.
- Pay 15 and climb two steps to reach the top.
The total cost is 15.
```

**Example 2:**

```
Input: cost = [1,100,1,1,1,100,1,1,100,1]
Output: 6
Explanation: You will start at index 0.
- Pay 1 and climb two steps to reach index 2.
- Pay 1 and climb two steps to reach index 4.
- Pay 1 and climb two steps to reach index 6.
- Pay 1 and climb one step to reach index 7.
- Pay 1 and climb two steps to reach index 9.
- Pay 1 and climb one step to reach the top.
The total cost is 6.
```

 

**Constraints:**

- `2 <= cost.length <= 1000`
- `0 <= cost[i] <= 999`



## Coding Question - N-th Tribonacci Number

The Tribonacci sequence Tn is defined as follows: 

T0 = 0, T1 = 1, T2 = 1, and Tn+3 = Tn + Tn+1 + Tn+2 for n >= 0.

Given `n`, return the value of Tn.

 

**Example 1:**

```
Input: n = 4
Output: 4
Explanation:
T_3 = 0 + 1 + 1 = 2
T_4 = 1 + 1 + 2 = 4
```

**Example 2:**

```
Input: n = 25
Output: 1389537
```

 

**Constraints:**

- `0 <= n <= 37`
- The answer is guaranteed to fit within a 32-bit integer, ie. `answer <= 2^31 - 1`.



**Hide Hint #1**

Make an array F of length 38, and set F[0] = 0, F[1] = F[2] = 1.

**Hide Hint #2**

Now write a loop where you set F[n+3] = F[n] + F[n+1] + F[n+2], and return F[n].



## Coding Question - Delete and Earn

You are given an integer array `nums`. You want to maximize the number of points you get by performing the following operation any number of times:

- Pick any `nums[i]` and delete it to earn `nums[i]` points. Afterwards, you must delete **every** element equal to `nums[i] - 1` and **every** element equal to `nums[i] + 1`.

Return *the **maximum number of points** you can earn by applying the above operation some number of times*.

 

**Example 1:**

```
Input: nums = [3,4,2]
Output: 6
Explanation: You can perform the following operations:
- Delete 4 to earn 4 points. Consequently, 3 is also deleted. nums = [2].
- Delete 2 to earn 2 points. nums = [].
You earn a total of 6 points.
```

**Example 2:**

```
Input: nums = [2,2,3,3,3,4]
Output: 9
Explanation: You can perform the following operations:
- Delete a 3 to earn 3 points. All 2's and 4's are also deleted. nums = [3,3].
- Delete a 3 again to earn 3 points. nums = [3].
- Delete a 3 once more to earn 3 points. nums = [].
You earn a total of 9 points.
```

 

**Constraints:**

- `1 <= nums.length <= 2 * 104`
- `1 <= nums[i] <= 104`



**Hide Hint #1 **

If you take a number, you might as well take them all. Keep track of what the value is of the subset of the input with maximum M when you either take or don't take M.



##  Multidimensional DP



The dimensions of a DP algorithm refer to the number of state variables used to define each state. So far in this explore card, all the algorithms we have looked at required only one state variable - therefore they are **one-dimensional**. In this section, we're going to talk about problems that require multiple dimensions.

Typically, the more dimensions a DP problem has, the more difficult it is to solve. Two-dimensional problems are common, and sometimes a problem might even require [five dimensions](https://leetcode.com/problems/maximize-grid-happiness/). The good news is, the framework works regardless of the number of dimensions.

The following are common things to look out for in DP problems that require a state variable:

- An index along some input. This is usually used if an input is given as an array or string. This has been the sole state variable for all the problems that we've looked at so far, and it has represented the answer to the problem if the input was considered only up to that index - for example, if the input is $nums = [0, 1, 2, 3, 4, 5, 6]$, then $dp(4)$ would represent the answer to the problem for the input $nums = [0, 1, 2, 3, 4]$.
- A second index along some input. Sometimes, you need two index state variables, say $i$ and $j$. In some questions, these variables represent the answer to the original problem if you considered the input starting at index $i$ and ending at index $j$. Using the same example above, $dp(1, 3)$ would solve the problem for the input $nums = [1, 2, 3]$, if the original input was $[0, 1, 2, 3, 4, 5, 6]$.
- Explicit numerical constraints given in the problem. For example, "you are only allowed to complete $k$ transactions", or "you are allowed to break up to $k$ obstacles", etc.
- Variables that describe statuses in a given state. For example "true if currently holding a key, false if not", "currently holding $k$ packages" etc.
- Some sort of data like a tuple or bitmask used to indicate things being "visited" or "used". For example, "$bitmask$ is a mask where the $i^{th}$ bit indicates if the $i^{th}$ city has been visited". Note that mutable data structures like arrays cannot be used - typically, only immutable data structures like numbers and strings can be hashed, and therefore memoized.

Multi-dimensional problems make us think harder about deciding what our function or array will represent, as well as what the recurrence relation should look like. In the next article, we'll walk through another example using the framework with a 2D DP problem.




## Top-down to Bottom-up



As we've said in the previous chapter, **usually** a top-down algorithm is easier to implement than the equivalent bottom-up algorithm. With that being said, it is useful to know how to take a completed top-down algorithm and convert it to bottom-up. There's a number of reasons for this: first, in an interview, if you solve a problem with top-down, you may be asked to rewrite your solution in an iterative manner (using bottom-up) instead. Second, as we mentioned before, bottom-up **usually** is more efficient than top-down in terms of runtime.

**Steps to convert top-down into bottom-up**

1. Start with a completed top-down implementation.
2. Initialize an array $dp$ that is sized according to your state variables. For example, let's say the input to the problem was an array $nums$ and an integer $k$ that represents the maximum number of actions allowed. Your array $dp$ would be 2D with one dimension of length $nums.length$ and the other of length $k$. The values should be initialized as some default value opposite of what the problem is asking for. For example, if the problem is asking for the maximum of something, set the values to negative infinity. If it is asking for the minimum of something, set the values to infinity.
3. Set your base cases, same as the ones you are using in your top-down function. Recall in House Robber, $dp(0) = nums[0]$ and $dp(1) = max(nums[0], nums[1])$. In bottom-up, $dp[0] = nums[0]$ and $dp[1] = max(nums[0], nums[1])$.
4. Write a for-loop(s) that iterate over your state variables. If you have multiple state variables, you will need nested for-loops. These loops should **start iterating from the base cases**.
5. Now, each iteration of the inner-most loop represents a given state, and is equivalent to a function call to the same state in top-down. Copy the logic from your function into the for-loop and change the function calls to accessing your array. All $dp(...)$ changes into $dp[...]$.
6. We're done! $dp$ is now an array populated with the answer to the original problem for all possible states. Return the answer to the original problem, by changing $return dp(...)$ to $return dp[...]$.



Let's try a quick example using the House Robber code from before. Here's a completed top-down solution:
~~~
class Solution {
    private HashMap<Integer, Integer> memo = new HashMap<Integer, Integer>();
    private int[] nums;
    
    private int dp(int i) {
        // Base cases
        if (i == 0) return nums[0];
        if (i == 1) return Math.max(nums[0], nums[1]);
        if (!memo.containsKey(i)) {
            memo.put(i, Math.max(dp(i - 1), dp(i - 2) + nums[i])); // Recurrence relation
        }
        return memo.get(i);
    }
    
    public int rob(int[] nums) {
        this.nums = nums;
        return dp(nums.length - 1);
    }
}
~~~
First, we initialize an array $dp$ sized according to our state variables. Our only state variable is $i$ which can take $n$ values.
~~~
class Solution {
    public int rob(int[] nums) {
        int[] dp = new int[nums.length];
    }
}
~~~
Second, we should set our base cases. $dp[0] = nums[0]$ and $dp[1] = max(nums[0], nums[1])$. To avoid index out of bounds, we should also just return $nums[0]$ if theres only one house.

~~~
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) {
            return nums[0];
        }

        int[] dp = new int[nums.length];
        
        // Base cases
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
    }
}
~~~

Next, write a for-loop to iterate over the state variables, starting from the base cases.
~~~
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) return nums[0];
        
        int[] dp = new int[nums.length];
        
        // Base cases
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        
        for (int i = 2; i < nums.length; i++) {
            
        }
    }
}
~~~

Lastly, copy the recurrence relation over from the top-down solution and put it in the for-loop. Return $dp[n - 1]$.

~~~
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) return nums[0];
        
        int[] dp = new int[nums.length];
        
        // Base cases
        dp[0] = nums[0];
        dp[1] = Math.max(nums[0], nums[1]);
        
        for (int i = 2; i < nums.length; i++) {
            dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]); // Recurrence relation
        }
        
        return dp[nums.length - 1];
    }
}
~~~

## Example 1770. Maximum Score from Performing Multiplication Operations

> For this problem, we will again start by looking at a top-down approach.

In this article, we're going to be looking at the problem [Maximum Score from Performing Multiplication Operations](https://leetcode.com/problems/maximum-score-from-performing-multiplication-operations/). We can tell this is a DP problem because it is asking for a maximum score, and every time we choose to use a number from $nums$, it affects all future possibilities. Let's solve this problem with the framework:

1. A **function or array** that answers the problem for a given state

Since we're doing top-down, we need to decide on two things for our function $dp$. What state variables we need to pass to it, and what it will return. We are given two input arrays: $nums$ and $multipliers$. The problem says we need to do $m$ operations, and on the $i^{th}$ operation, we gain score equal to $multipliers[i]$ times a number from either the left or right end of $nums$, which we remove after the operation. That means we need to know 3 things for each operation:

1. How many operations have we done so far; this tells us what number from $multipliers$ we will be using?
2. The index of the leftmost number remaining in $nums$.
3. The index of the rightmost number remaining in $nums$.

We can use one state variable, $i$, to indicate how many operations we have done so far, which means $multipliers[i]$ is the current multiplier to be used. For the leftmost number remaining in $nums$, we can use another state variable, $left$, that indicates how many left operations we have done so far. If we have done, say 3 left operations, if we were to do another left operation we would use $nums[3]$ (because nums is 0-indexed). We can say the same thing for the rightmost remaining number - let's use a state variable $right$ that indicates how many right operations we have done so far.

It may seem like we need all 3 of these state variables, but we can formulate an equation for one of them using the other two. If we know how many elements we have picked from the leftside, $left$, and we know how many elements we have picked in total, $i$, then we know that we must have picked $i - left$ elements from the rightside. The original length of $nums$ is $n$, which means the index of the rightmost element is $right = n - 1 - (i - left)$. Therefore, we only need 2 state variables: $i$ and $left$, and we can calculate $right$ inside the function.

Now that we have our state variables, what should our function return? The problem is asking for the maximum score from some number of operations, so let's have our function $dp(i, left)$ return the maximum possible score if we have already done $i$ total operations and used $left$ numbers from the left side. To answer the original problem, we should return $dp(0, 0)$.

![img](Dynamic%20Programming.assets/9ccd6275-00d1-43f6-9cb8-29dc24416e61.png)
![img](Dynamic%20Programming.assets/cc90866a-82a8-4d49-8eff-bb2f3fd1d11e.png)
![img](Dynamic%20Programming.assets/ec5ed092-2548-46b8-9401-28c216de464e.png)

2. A **recurrence relation** to transition between states

At each state, we have to perform an operation. As stated in the problem description, we need to decide whether to take from the left end ($nums[left]$) or the right end ($nums[right]$) of the current $nums$. Then we need to multiply the number we choose by $multipliers[i]$, add this value to our score, and finally remove the number we chose from $nums$. For implementation purposes, "removing" a number from $nums$ means incrementing our state variables $i$ and $left$ so that they point to the next two left and right numbers.

Let $mult=multipliers[i]$ and $right = nums.length - 1 - (i - left)$. The only decision we have to make is whether to take from the left or right of $nums$.

- If we choose left, we gain $mult⋅nums[left]$ points from this operation. Then, the next operation will occur at $(i + 1, left + 1)$. $i$ gets incremented at every operation because it represents how many operations we have done, and $left$ gets incremented because it represents how many left operations we have done. Therefore, our total score is $mult⋅nums[left] + dp(i + 1, left + 1)$.
- If we choose right, we gain $mult⋅nums[right]$ points from this operation. Then, the next operation will occur at $(i + 1, left)$. Therefore, our total score is $mult⋅nums[right] + dp(i + 1, left)$.

Since we want to maximize our score, we should choose the side that gives more points. This gives us our recurrence relation:

$dp(i, left)=max(mult⋅nums[left]+dp(i + 1, left + 1), mult⋅nums[right]+dp(i + 1, left))$

Where $mult⋅nums[left]+dp(i + 1, left + 1)$ represents the points we gain by taking from the left end of $nums$ plus the maximum points we can get from the remaining $nums$ array and $mult⋅nums[right]+dp(i + 1, left)$ represents the points we gain by taking from the right end of $nums$ plus the maximum points we can get from the remaining $nums$ array.

3. **Base cases**

The problem statement says that we need to perform $m$ operations. When $i$ equals $m$, that means we have no operations left. Therefore, we should return $0$.



### Top-down Implementation

Let's put the 3 parts of the framework together for a solution to the problem.

Protip: for Python, the [functools](https://docs.python.org/3/library/functools.html) module provides super handy tools that automatically memoize a function for us. We're going to use the `@lru_cache` decorator in the Python implementation.

> If you find yourself needing to memoize a function in an interview and you're using Python, check with your interviewer if using modules like functools is OK.

This particular problem happens to have very tight time limits. For Java, instead of using a hashmap for the memoization, we will use a 2D array. For Python, we're going to limit our cache size to $2000$.


~~~
class Solution {
    private int[][] memo;
    private int[] nums, multipliers;
    private int n, m;
    
    private int dp(int i, int left) {
        if (i == m) {
            return 0; // Base case
        }

        int mult = multipliers[i];
        int right = n - 1 - (i - left);
            
        if (memo[i][left] == 0) {
            // Recurrence relation
            memo[i][left] = Math.max(mult * nums[left] + dp(i + 1, left + 1), 
                                     mult * nums[right] + dp(i + 1, left));
        }

        return memo[i][left];
    }
    
    public int maximumScore(int[] nums, int[] multipliers) {
        n = nums.length;
        m = multipliers.length;
        this.nums = nums;
        this.multipliers = multipliers;
        this.memo = new int[m][m];
        return dp(0, 0);
    }
}
~~~

### Bottom-up Implementation

In the bottom-up implementation, the array works the same way as the function from top-down. $dp[i][left]$ represents the max score possible if $i$ operations have been performed and $left$ left operations have been performed.

Earlier in the explore card, we learned that while bottom-up is typically faster than top-down, it is often harder to implement. This is because the order in which we iterate needs to be precise. You'll see in the implementations below that we use the same math to calculate $right$, and the same recurrence relation but we need to iterate backwards starting from $m$ (because the base case happens when $i$ equals $m$). We also need to initialize $dp$ with one extra row so that we don't go out of bounds in the first iteration of the outer loop.

~~~
class Solution {
    public int maximumScore(int[] nums, int[] multipliers) {
        int n = nums.length;
        int m = multipliers.length;
        int[][] dp = new int[m + 1][m + 1];
        
        for (int i = m - 1; i >= 0; i--) {
            for (int left = i; left >= 0; left--) {
                int mult = multipliers[i];
                int right = n - 1 - (i - left);
                dp[i][left] = Math.max(mult * nums[left] + dp[i + 1][left + 1], 
                                       mult * nums[right] + dp[i + 1][left]);
            }
        }
        
        return dp[0][0];
    }
}
~~~

The time and space complexity of both implementations is $O(m^2)$ where $m$ is the length of $multipliers$. We will talk about more in depth about time and space complexity at the end of this chapter.

### Up Next

Try the next two problems on your own. The first one is a very classical computer science problem and popular in interviews. If you get stuck, come back here for hints:

1143. Longest Common Subsequence

Click here to show hint regarding state variables and dp
Let $dp(i, j)$ represent the longest common subsequence between the string $text1$ up to index $i$ and $text2$ up to index $j$.

Click here to show hint regarding the recurrence relation
If $text1[i] == text2[j]$, then we should use this character, giving us $1 + dp(i - 1, j - 1)$. Otherwise, we can either move one character back from $text1$, or 1 character back from $text2$. Try both.

Click here to show hint regarding base cases
221. Maximal Square

Click here to show hint regarding state variables and dp
Let $dp[row][col]$ represent the largest possible square whose bottom right corner is on $matrix[row][col]$.

Click here to show hint regarding the recurrence relation
Any square with a $0$ cannot have a square on it, and should be ignored. Otherwise, let's say you had a 3x3 square. Look at the bottom right corner of this 3x3 square. What do the squares above, to the left, and to the up-left of this square have in common? All 3 of those squares are the bottom-right square of a square that is (at least) 2x2.

Click here to show hint regarding base cases
Just make sure to stay in bounds.



## Coding Question - Maximum Score from Performing Multiplication Operations

You are given two integer arrays `nums` and `multipliers` of size `n` and `m` respectively, where `n >= m`. The arrays are **1-indexed**.

You begin with a score of `0`. You want to perform **exactly** `m` operations. On the `ith` operation **(1-indexed)**, you will:

- Choose one integer `x` from **either the start or the end** of the array `nums`.
- Add `multipliers[i] * x` to your score.
- Remove `x` from the array `nums`.

Return *the **maximum** score after performing* `m` *operations.*

 

**Example 1:**

```
Input: nums = [1,2,3], multipliers = [3,2,1]
Output: 14
Explanation: An optimal solution is as follows:
- Choose from the end, [1,2,3], adding 3 * 3 = 9 to the score.
- Choose from the end, [1,2], adding 2 * 2 = 4 to the score.
- Choose from the end, [1], adding 1 * 1 = 1 to the score.
The total score is 9 + 4 + 1 = 14.
```

**Example 2:**

```
Input: nums = [-5,-3,-3,-2,7,1], multipliers = [-10,-5,3,4,6]
Output: 102
Explanation: An optimal solution is as follows:
- Choose from the start, [-5,-3,-3,-2,7,1], adding -5 * -10 = 50 to the score.
- Choose from the start, [-3,-3,-2,7,1], adding -3 * -5 = 15 to the score.
- Choose from the start, [-3,-2,7,1], adding -3 * 3 = -9 to the score.
- Choose from the end, [-2,7,1], adding 1 * 4 = 4 to the score.
- Choose from the end, [-2,7], adding 7 * 6 = 42 to the score. 
The total score is 50 + 15 - 9 + 4 + 42 = 102.
```

 

**Constraints:**

- `n == nums.length`
- `m == multipliers.length`
- `1 <= m <= 103`
- `m <= n <= 105```
- `-1000 <= nums[i], multipliers[i] <= 1000`



**Hide Hint #1 **

At first glance, the solution seems to be greedy, but if you try to greedily take the largest value from the beginning or the end, this will not be optimal.

**Hide Hint #2**

You should try all scenarios but this will be costy.

**Hide Hint #3 **

Memoizing the pre-visited states while trying all the possible scenarios will reduce the complexity, and hence dp is a perfect choice here.



## Coding Question - Longest Common Subsequence

Given two strings `text1` and `text2`, return *the length of their longest **common subsequence**.* If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

 

**Example 1:**

```
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```

**Example 2:**

```
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
```

**Example 3:**

```
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

 

**Constraints:**

- `1 <= text1.length, text2.length <= 1000`
- `text1` and `text2` consist of only lowercase English characters.



**Hide Hint #1**

Try dynamic programming. $DP[i][j]$ represents the longest common subsequence of $text1[0 ... i]$ & $text2[0 ... j]$.

**Hide Hint #2**

$DP[i][j] = DP[i - 1][j - 1] + 1$ , if $text1[i] == text2[j] DP[i][j] = max(DP[i - 1][j], DP[i][j - 1])$ , otherwise



## Coding Question - Maximal Square

Given an `m x n` binary `matrix` filled with `0`'s and `1`'s, *find the largest square containing only* `1`'s *and return its area*.

 

**Example 1:**

![img](Dynamic%20Programming.assets/max1grid.jpg)

```
Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
Output: 4
```

**Example 2:**

![img](Dynamic%20Programming.assets/max2grid.jpg)

```
Input: matrix = [["0","1"],["1","0"]]
Output: 1
```

**Example 3:**

```
Input: matrix = [["0"]]
Output: 0
```

 

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 300`
- `matrix[i][j]` is `'0'` or `'1'`.



##  Time and Space Complexity

Finding the time and space complexity of a dynamic programming algorithm may sound like a daunting task. However, this task is usually not as difficult as it sounds. Furthermore, justifying the time and space complexity in an explanation is relatively simple as well. One of the main points with DP is that we never repeat calculations, whether by tabulation or memoization, we only compute a state once. Because of this, the time complexity of a DP algorithm is directly tied to the number of possible states.

If computing each state requires $F$ time, and there are $n$ possible states, then the time complexity of a DP algorithm is $O(n⋅F)$. With all the problems we have looked at so far, computing a state has just been using a recurrence relation equation, which is $O(1)$. Therefore, the time complexity has just been equal to the number of states. To find the number of states, look at each of your state variables, compute the number of values each one can represent, and then multiply all these numbers together.

Let's say we had 3 state variables: $i$, $k$, and $holding$ for some made up problem. $i$ is an integer used to keep track of an index for an input array $nums$, $k$ is an integer given in the input which represents the maximum actions we can do, and $holding$ is a boolean variable. What will the time complexity be for a DP algorithm that solves this problem? Let $n = nums.length$ and $K$ be the maximum actions possible given in the input. $i$ can be from $0$ to $nums.length$, $k$ can be from $0$ to $K$, and $holding$ }can be true or false. Therefore, there are $n⋅K⋅2$ states. If computing each state is $O(1)$, then the time complexity will be $O(n⋅K⋅2) = O(n⋅K)$.

Whenever we compute a state, we also store it so that we can refer to it in the future. In bottom-up, we tabulate the results, and in top-down, states are memoized. Since we store states, the space complexity is equal to the number of states. That means that in problems where calculating a state is $O(1)$, the time and space complexity are the same. In many DP problems, there are optimizations that can improve both complexities - we'll talk about this later.



##  Chapter 2 quiz

------

Multiple Choice Question

The three main components of the framework are:

The choice between bottom-up and top-down

The base cases

The recurrence relation

The number of dimensions

The time complexity

A data structure and a way of visiting each DP state

> Commit these to memory! Understanding the components of the framework allows us to approach dynamic programming problems in a step by step way.



------



Multiple Choice Question

Which of the following require state variables?

Remaining number of moves allowed

Original length of the input

Current index along the input

Number of keys currently being held

The original number of moves allowed

> Constants should never be state variables.



------



Single Choice Question

Typically, implementing a top-down algorithm is easier than the equivalent bottom-up algorithm.

True

False

> Often it feels more intuitive to write the top-down implementation of a dynamic programming approach first. This is partly because in the top-down implementation, when the current state is not a base case, we can simply look at the recurrence relation to make the appropriate recursive calls. These recursive calls decide the order in which we visit each state. However, when writing the bottom-up implementation, we must carefully choose how we iterate over the state variables. We must ensure that we never visit a state before we have the results for all subproblems used to solve the current state. Fortunately, there is a way to systematically convert a top-down algorithm into its bottom-up version. So we can always write the more intuitive approach first (top-down) and then convert it to bottom-up if desired.



------



Single Choice Question

Generally, the time and space complexity of dynamic programming algorithms is directly related to:

Whether the algorithm is top-down or bottom-up

The number of possible states

Whether an array or hashtable is used for caching results

> In many cases, the space complexity is the number of states, and the time complexity is the number of states multiplied by the cost of calculating a state.




# Common Patterns in DP

##  Iteration in the recurrence relation



In all the problems we have looked at so far, the recurrence relation is a static equation - it never changes. Recall [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/). The recurrence relation was:

$dp(i)=min(dp(i - 1) + cost[i - 1], dp(i - 2) + cost[i - 2])$

because we are only allowed to climb 1 or 2 steps at a time. What if the question was rephrased so that we could take up to $k$ steps at a time? The recurrence relation would become dynamic - it would be:

$dp(i)=min(dp(j) + cost[j])$ for all $(i - k)≤j<i$

We would need iteration in our recurrence relation.

This is a common pattern in DP problems, and in this chapter, we're going to take a look at some problems using the framework where this pattern is applicable. While iteration usually increases the difficulty of a DP problem, particularly with bottom-up implementations, the idea isn't too complicated. Instead of choosing from a static number of options, we usually add a for-loop to iterate through a dynamic number of options and choose the best one.



##  Example 1335. Minimum Difficulty of a Job Schedule

> We'll start with a top-down approach.

In this article, we'll be using the framework to solve [Minimum Difficulty of a Job Schedule](https://leetcode.com/problems/minimum-difficulty-of-a-job-schedule/). We can tell this is a problem where Dynamic Programming can be used because we are asked for the minimum of something, and deciding how many jobs to do on a given day affects the jobs we can do on all future days. Let's start solving:

1. A **function** that answers the problem for a given state

Let's first decide on state variables. What decisions are there to make, and what information do we need to make these decisions? Reading the problem description carefully, there are $d$ total days, and on each day we need to complete some number of jobs. By the end of the $d$ days, we must have finished all jobs (in the given order). Therefore, we can see that on each day, we need to decide how many jobs to take.

- Let's use one state variable $i$, where $i$ is the index of the first job that will be done on the current day.
- Let's use another state variable $day$, where $day$ indicates what day it currently is.

The problem is asking for the minimum difficulty, so let's have a function $dp(i, day)$ that returns the minimum difficulty of a job schedule which starts on the $i^{th}$ job and day, $day$. To solve the original problem, we will just return $dp(0, 1)$, since we start on the first day with no jobs done yet.

![img](Dynamic%20Programming.assets/C3A2_1_cropped.png)

![img](Dynamic%20Programming.assets/C3A2_2_cropped.png)

2. A **recurrence relation** to transition between states

At each state, we are on day $day$ and need to do job $i$. Then, we can choose to do a few more jobs. How many more jobs are we allowed to do? The problem says that we need to do at least one job per day. This means we **must** leave at least $d - day$ jobs so that all the future days have at least one job that can be scheduled on that day. If $n$ is the total number of jobs, $jobDifficulty.length$, that means from any given state $(i, day)$, we are allowed to do the jobs from index $i$ up to but not including index $n - (d - day)$.

We should try all the options for a given day - try doing only one job, then two jobs, etc. until we can't do any more jobs. The best option is the one that results in the easiest job schedule.

The difficulty of a given day is the most difficult job that we did that day. Since the jobs have to be done in order, if we are trying all the jobs we are allowed to do on that day (iterating through them), then we can use a variable $hardest$ to keep track of the difficulty of the hardest job done today. If we choose to do jobs up to the $j^{th}$ job (inclusive), where $i≤j<n - (d - day)$ (as derived above), then that means on the next day, we start with the $(j + 1)^{th}$ job. Therefore, our total difficulty is $hardest + dp(j + 1, day + 1)$. This gives us our scariest recurrence relation so far:

$dp(i, day) = min(hardest + dp(j + 1, day + 1))$ for all $i≤j<n - (d - day)$, where $hardest = max(jobDifficulty[k])$ for all $i≤k≤j$.

The codified recurrence relation is a scary one to look at for sure. However, it is easier to understand when we break it down bit by bit. On each day, we try all the options - do only one job, then two jobs, etc. until we can't do any more (since we need to leave some jobs for future days). $hardest$ is the hardest job we do on the current day, which means it is also the difficulty of the current day. We add $hardest$ to the next state which is the next day, starting with the next job. After trying all the jobs we are allowed to do, choose the best result.

![img](Dynamic%20Programming.assets/adce0788-577a-4e83-b03e-970fb32f8260.png)

![img](Dynamic%20Programming.assets/7ee9666d-524e-4b36-bec4-478dcef0bd50.png)

![img](Dynamic%20Programming.assets/b7f1a73e-abbb-4761-b05f-55b58e94e414.png)



3. **Base cases**

Despite the recurrence relation being complicated, the base cases are much simpler. We need to finish all jobs in $d$ days. Therefore, if it is the last day ($day == d$), we need to finish up all the remaining jobs on this day, and the total difficulty will just be the largest number in $jobDifficulty$ on or after index $i$.

$if day == d$ then return the maximum job difficulty between job $i$ and the end of the array (inclusive).

We can precompute an array $hardestJobRemaining$ where $hardestJobRemaining[i]$ represents the difficulty of the hardest job on or after day $i$, so that we this base case is handled in constant time.

Additionally, if there are more days than jobs ($n < d$), then it is impossible to do at least one job each day, and per the problem description, we should return $-1$. We can check for this case at the very start.



### Top-down Implementation

Let's combine these 3 parts for a top-down implementation. Again, we will use [functools](https://docs.python.org/3/library/functools.html) in Python, and a 2D array in Java for memoization. In the Python implementation, we are passing $None$ to `lru_cache` which means the cache size is not limited. We are doing this because the number of states that will be re-used in this problem is large, and we don't want to evict a state early and have to re-calculate it.

~~~
class Solution {
    private int n, d;
    private int[][] memo;
    private int[] jobDifficulty;
    private int[] hardestJobRemaining;
    
    private int dp(int i, int day) {
        // Base case, it's the last day so we need to finish all the jobs
        if (day == d) {
            return hardestJobRemaining[i];
        }
        
        if (memo[i][day] == -1) {
            int best = Integer.MAX_VALUE;
            int hardest = 0;
            // Iterate through the options and choose the best
            for (int j = i; j < n - (d - day); j++) {
                hardest = Math.max(hardest, jobDifficulty[j]);
                // Recurrence relation
                best = Math.min(best, hardest + dp(j + 1, day + 1));
            }
            memo[i][day] = best;
        }
        
        return memo[i][day];
    }
    
    public int minDifficulty(int[] jobDifficulty, int d) {
        n = jobDifficulty.length;
        // If we cannot schedule at least one job per day, 
        // it is impossible to create a schedule
        if (n < d) {
            return -1;
        }
        
        hardestJobRemaining = new int[n];
        int hardestJob = 0;
        for (int i = n - 1; i >= 0; i--) {
            hardestJob = Math.max(hardestJob, jobDifficulty[i]);
            hardestJobRemaining[i] = hardestJob;
        }
        
        // Initialize memo array with value of -1.
        memo = new int[n][d + 1];
        for (int i = 0; i < n; i++) {
            Arrays.fill(memo[i], -1);
        }
        
        this.d = d;
        this.jobDifficulty = jobDifficulty;
        return dp(0, 1);
    }
}
~~~

### Bottom-up Implementation

With bottom-up, we now use a 2D array where $dp[i][day]$ represents the minimum difficulty of a job schedule that starts on day $day$ and job $i$. It depends on the problem, but the bottom-up code generally has a faster runtime than its top-down equivalent. However, as you can see from the code, it looks like it is more challenging to implement. We need to first tabulate the base case and then work backwards from them using nested for loops.

The for-loops should start iterating from the base cases, and there should be one for-loop for each state variable. Remember that one of our base cases is that on the final day, we need to complete all jobs. Therefore, our for-loop iterating over $day$ should iterate from the final day to the first day. Then, our next for-loop for $i$ should conform to the restraints of the problem - we need to do at least one job per day.

~~~
class Solution {
    public int minDifficulty(int[] jobDifficulty, int d) {
        int n = jobDifficulty.length;
        // If we cannot schedule at least one job per day, 
        // it is impossible to create a schedule
        if (n < d) {
            return -1;
        }
        
        int dp[][] = new int[n][d + 1];
        for (int[] row: dp) {
            Arrays.fill(row, Integer.MAX_VALUE);
        }
        
        // Set base cases
        dp[n - 1][d] = jobDifficulty[n - 1];

        // On the last day, we must schedule all remaining jobs, so dp[i][d]
        // is the maximum difficulty job remaining
        for (int i = n - 2; i >= 0; i--) {
            dp[i][d] = Math.max(dp[i + 1][d], jobDifficulty[i]);
        }
        
        for (int day = d - 1; day > 0; day--) {
            for (int i = day - 1; i < n - (d - day); i++) {
                int hardest = 0;
                // Iterate through the options and choose the best
                for (int j = i; j < n - (d - day); j++) {
                    hardest = Math.max(hardest, jobDifficulty[j]);
                    // Recurrence relation
                    dp[i][day] = Math.min(dp[i][day], hardest + dp[j + 1][day + 1]);
                }
            }
        }
        
        return dp[0][1];
    }
}
~~~

Here's an animation showing the algorithm in action:

![img](Dynamic%20Programming.assets/780c37ad-8f63-462f-ab6e-ebc39dd08885.jpeg)
![img](Dynamic%20Programming.assets/5f78ac98-c021-4e66-87c1-4498db8e6b0b.jpeg)
![img](Dynamic%20Programming.assets/4328dd9e-e0ee-4d22-ad8b-4c4ebb0ad2d6.jpeg)
![img](Dynamic%20Programming.assets/fb015a97-087a-45f7-a724-1a00f3b0eb7c.jpeg)
![img](Dynamic%20Programming.assets/bb8633f1-a4cc-46a2-8453-a2425aece689.jpeg)
![img](Dynamic%20Programming.assets/b13ec281-620f-418b-bb85-6d6e848d65e7.jpeg)
![img](Dynamic%20Programming.assets/6386e222-87b3-4929-9af5-d8144ad7947a.jpeg)
![img](Dynamic%20Programming.assets/78f22e40-bf61-4a06-9fc8-9dabbde449f1.jpeg)
![img](Dynamic%20Programming.assets/3e8108bb-7512-4388-8808-0260e4eded03.jpeg)
![img](Dynamic%20Programming.assets/25b38c05-0a9a-4586-b6ee-2d81dc022eea.jpeg)
![img](Dynamic%20Programming.assets/5ed9742e-7461-4d46-8fbc-fc310e9effa6.jpeg)
![img](Dynamic%20Programming.assets/75f67fd0-5d78-466d-845b-23e276707f4e.jpeg)
![img](Dynamic%20Programming.assets/c8c1c06e-a2ea-4c36-8faf-629dbe8f1c50.jpeg)
![img](Dynamic%20Programming.assets/f04be944-7904-4b55-a29a-a25214de5253.jpeg)
![img](Dynamic%20Programming.assets/54e343f1-59c3-4153-abb5-c9d17043af17.jpeg)
![img](Dynamic%20Programming.assets/9752c601-592a-4567-b145-43344d7d0437.jpeg)
![img](Dynamic%20Programming.assets/25bef1b0-1bdf-4810-a9f2-b90ae60ecaa9.jpeg)
![img](Dynamic%20Programming.assets/18dfb9af-86c3-449e-bf44-a4f0a503d7c6.jpeg)
![img](Dynamic%20Programming.assets/bc834535-4aa1-472f-9237-1f5578e1e495.jpeg)
![img](Dynamic%20Programming.assets/b63676c6-2541-4747-92ab-45264a0a91cb.jpeg)
![img](Dynamic%20Programming.assets/c44cfa23-cf94-4bd1-afe3-0b234261b50a.jpeg)
![img](Dynamic%20Programming.assets/e288192a-a5fc-47c4-bab4-0267e1be0f79.jpeg)
![img](Dynamic%20Programming.assets/32f17ca6-6b03-4f48-a14a-1bd342457276.jpeg)
![img](Dynamic%20Programming.assets/c83df3d2-3cdc-4162-aed7-742016845045.jpeg)
![img](Dynamic%20Programming.assets/a3a0dd53-e526-4bc9-b30d-e0a8b2844216.jpeg)
![img](Dynamic%20Programming.assets/ea12ef54-6852-4d3a-9a42-db135166b20d.jpeg)
![img](Dynamic%20Programming.assets/e75dbadd-5547-442e-9e8e-11e53ad2b5dc.jpeg)
![img](Dynamic%20Programming.assets/00df0fc0-393e-4927-ae5f-7b7f26db1d56.jpeg)
![img](Dynamic%20Programming.assets/e5cdd626-d60b-4ab3-a29a-4635546dce92.jpeg)
![img](Dynamic%20Programming.assets/acbbc707-cb50-4be1-b764-e1de01328f7b.jpeg)




The time and space complexity of these algorithms can be quite tricky, and as in this example, there are sometimes slight differences between the top-down and bottom-up complexities.

Let's start with the bottom-up space complexity, because it follows what we learned in the previous chapter about finding time and space complexity. For this problem, the number of states is $n⋅d$. This means the space complexity is $O(n⋅d)$ as our $dp$ table takes up that much space.

The top-down algorithm's space complexity is actually a bit better. In top-down, when we memoize results with a hashtable, the hashtable's size only grows when we visit a state and calculate the answer for it. Because of the restriction of needing to complete at least one task per day, we don't actually need to visit all $n⋅d$ states. For example, if there were $10$ jobs and $5$ days, then the state $(9, 2)$ (starting the final job on the second day) is not reachable, because the 3rd, 4th, and 5th days wouldn't have a job to complete. This is true for both implementations and is enforced by our for-loops, and as a result, we only actually visit $d⋅(n−d)$ states. This means the space complexity for top-down is $O(d⋅(n - d))$. This is one advantage that top-down can have over bottom-up. With the bottom-up implementation, we can't really avoid allocating space for $n⋅d$ states because we are using a 2D array.

The time complexity for both algorithms is more complicated. As we just found out, we only actually visit $d⋅(n−d)$ states. At each state, we go through a for-loop (with variable $j$) that iterates on average $\frac{n - d}{2}$ times. This means our time complexity for both algorithms is $O(d⋅(n - d)^2)$.

**To summarize:**

Time complexity (both algorithms): $O(d⋅(n - d)^2)$

Space complexity (top-down): $O((n - d)⋅d)$

Space complexity (bottom-up): $O(n⋅d)$

> While the theoretical space complexity is better with top-down, practically speaking, the 2D array is more space-efficient than a hashmap, and the difference in space complexities here doesn't justify saying that top-down will use less space than bottom-up.



### Up Next

The next problem is a classical one and popular in interviews. Try it out yourself, and come back here if you need some hints:

Click here to show hint regarding state variables and $dp$
Let $dp[i]$ represent the fewest number of coins needed to make up $i$ money.

Click here to show hint regarding the recurrence relation.
For each coin available, we can get to $i$ from $i - coin$. Make sure that $coin≤i$.

Click here to show hint regarding the base cases.
According to the constraints, the minimum value for a coin is 1. Therefore, it is impossible to have 0 money, therefore $dp[0] = 0$.



## Coding Question - Minimum Difficulty of a Job Schedule

You want to schedule a list of jobs in `d` days. Jobs are dependent (i.e To work on the `ith` job, you have to finish all the jobs `j` where `0 <= j < i`).

You have to finish **at least** one task every day. The difficulty of a job schedule is the sum of difficulties of each day of the `d` days. The difficulty of a day is the maximum difficulty of a job done on that day.

You are given an integer array `jobDifficulty` and an integer `d`. The difficulty of the `ith` job is `jobDifficulty[i]`.

Return *the minimum difficulty of a job schedule*. If you cannot find a schedule for the jobs return `-1`.

 

**Example 1:**
![img](Dynamic%20Programming.assets/untitled.png)

```
Input: jobDifficulty = [6,5,4,3,2,1], d = 2
Output: 7
Explanation: First day you can finish the first 5 jobs, total difficulty = 6.
Second day you can finish the last job, total difficulty = 1.
The difficulty of the schedule = 6 + 1 = 7 
```

**Example 2:**

```
Input: jobDifficulty = [9,9,9], d = 4
Output: -1
Explanation: If you finish a job per day you will still have a free day. you cannot find a schedule for the given jobs.
```

**Example 3:**

```
Input: jobDifficulty = [1,1,1], d = 3
Output: 3
Explanation: The schedule is one job per day. total difficulty will be 3.
```

 

**Constraints:**

- `1 <= jobDifficulty.length <= 300`
- `0 <= jobDifficulty[i] <= 1000`
- `1 <= d <= 10`



**Hide Hint #1** 

Use DP. Try to cut the array into d non-empty sub-arrays. Try all possible cuts for the array.

**Hide Hint #2** 

Use dp[i][j] where DP states are i the index of the last cut and j the number of remaining cuts. Complexity is O(n * n * d).



## Coding Question - Coin Change

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the fewest number of coins that you need to make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

 

**Example 1:**

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

**Example 2:**

```
Input: coins = [2], amount = 3
Output: -1
```

**Example 3:**

```
Input: coins = [1], amount = 0
Output: 0
```

 

**Constraints:**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`



##  Example 139. Word Break



In this article, we'll use the framework to solve [Word Break](https://leetcode.com/problems/word-break/). So far, in this card, this is the most unique and perhaps the most difficult problem to see that dynamic programming is a viable approach. This is because, unlike all of the previous problems, we will not be working with numbers at all. When a question asks, "is it possible to do..." it isn't necessarily a dead giveaway that it should be solved with DP. However, we can see that in this question, the order in which we choose words from $wordDict$ is important, and a greedy strategy will not work.

> Recall back in the first chapter, we said that a good way to check if a problem should be solved with DP or greedy is to first assume that it can be solved greedily, then try to think of a counterexample.

Let's say that we had $s = "abcdef"$ and $wordDict = [ "abcde", "ef", "abc", "a", "d"]$. A greedy algorithm (picking the longest substring available) will not be able to determine that picking "abcde" here is the wrong decision. Likewise, a greedy algorithm (picking the shortest substring available) will not be able to determine that picking "a" first is the wrong decision.

With that being said, let's develop a DP algorithm using our framework:

> For this problem, we'll look at bottom-up first.

1. An **array** that answers the problem for a given state

Despite this problem being unlike the ones we have seen so far, we should still stick to the ideas of the framework. In the article where we learned about multi-dimensional dynamic programming, we talked about how an index variable, usually denoted $i$ is typically used in DP problems where the input is an array or string. All the problems that we have looked at up to this point reflect this.

- With this in mind, let's use a state variable $i$, which keeps track of which index we are currently at in $s$.
- Do we need any other state variables? The other input is $wordDict$ - however, it says in the problem that we can reuse words from $wordDict$ as much as we want. Therefore, a state variable isn't necessary because $wordDict$ and what we can do with it never changes. If the problem was changed so that we can only use a word once, or say $k$ times, then we would need extra state variables to know what words we are allowed to use at each state.

In all the past problems, we had a function $dp$ return the answer to the original problem for some state. We should try to do the same thing here. The problem is asking, is it possible to create $s$ by combining words in $wordDict$. So, let's have an array $dp$ where $dp[i]$ represents if it is possible to build the string $s$ up to index $i$ from $wordDict$. To answer the original problem, we can return $dp[s.length - 1]$ after populating $dp$.

2. A **recurrence relation** to transition between states

At each index $i$, what criteria determines if $dp[i]$ is true? First, a word from $wordDict$ needs to be able to **end** at index $i$. In terms of code, this means that there is some $word$ from $wordDict$ that matches the substring of $s$ that starts at index $i - word.length + 1$ and ends at index $i$.

We can iterate through all states of $i$ from $0$ up to but not including $s.length$, and at each state, check all the words in $wordDict$ for this criteria. For each $word$ in $wordDict$, if $s$ from index $i - word.length + 1$ to $i$ is equal to $word$, that means $word$ **ends** at $i$. However, this is not the sole criteria.

Remember, we are forming $s$ by adding words together. That means, if a $word$ meets the first criteria and we want to use it in a solution, we would add it on top of another string. We need to make sure that the string before it is also formable. If $word$ meets the first criteria, it starts at index $i - word.length + 1$. The index before that is $i - word.length$, and the second criteria is that $s$ up to this index is also formable from $wordDict$. This gives us our recurrence relation:

```
dp(i) = true` if `s.substring(i - word.length + 1, i + 1) == word` and `dp[i - word.length] == true` for any `word` in `wordDict`, otherwise `false
```

![img](Dynamic%20Programming.assets/C3A3_1_cropped.png)
![img](Dynamic%20Programming.assets/C3A3_2_cropped.png)
![img](Dynamic%20Programming.assets/C3A3_3_cropped.png)

In summary, the criteria is:

- $word$ from $wordDict$ can **end** at the current index $i$.
- If that $word$ is to end at index $i$, then it starts at index $i - word.length + 1$. The index before that $i - word.length$ should also be formable from $wordDict$.


3. **Base cases**

The base case for this problem is another simple one. The first word used from $wordDict$ starts at index $0$, which means we would need to check $dp[-1]$ for the second criteria, which is out of bounds. To fix this, we say that the second criteria can also be satisfied by $i == word.length - 1$.



### Bottom-up Implementation

~~~
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        boolean[] dp = new boolean[s.length()];
        for (int i = 0; i < s.length(); i++) {
            for (String word : wordDict) {
                // Make sure to stay in bounds while checking criteria
                if (i >= word.length() - 1 && (i == word.length() - 1 || dp[i - word.length()])) {
                    if (s.substring(i - word.length() + 1, i + 1).equals(word)) {
                        dp[i] = true;   
                        break;
                    }
                }
            }
        }
        
        return dp[s.length() - 1];
    }
}
~~~

### Top-down Implementation

In the top-down approach, we can check for the base case by returning $true$ if $i < 0$. In Java, we will memoize by using a $-1$ to indicate that the state is unvisited, $0$ to indicate $false$, and $1$ to indicate $true$.

~~~
class Solution {
    private String s;
    private List<String> wordDict;
    private int[] memo;
    
    private boolean dp(int i) {
        if (i < 0) return true;
        
        if (memo[i] == -1) {
            for (String word: wordDict) {
                if (i >= word.length() - 1 && dp(i - word.length())) {
                    if (s.substring(i - word.length() + 1, i + 1).equals(word)) {
                        memo[i] = 1;
                        break;
                    }
                }
            }
        }
        
        if (memo[i] == -1) {
            memo[i] = 0;
        }
        
        return memo[i] == 1;
    }
    
    public boolean wordBreak(String s, List<String> wordDict) {
        this.s = s;
        this.wordDict = wordDict;
        this.memo = new int[s.length()];
        Arrays.fill(this.memo, -1);
        return dp(s.length() - 1);
    }
}
~~~

Let's say that $n = s.length$, $k = wordDict.length$, and $L$ is the average length of the words in $wordDict$. While the space complexity for this problem is the same as the number of states $n$, the time complexity is much worse. At each state $i$, we iterate through $wordDict$ and splice $s$ to a new string with average length $L$. This gives us a time complexity of $O(n⋅k⋅L)$.



### Up Next

Before we move on to the next pattern, try the next practice problem which involves iteration on your own. It is a very classical computer science problem and is popular in interviews. As always, come back here if you need hints:



300. Longest Increasing Subsequence

Click here to show hint regarding state variables and dp
Let $dp[i]$ represent the length of the longest increasing subsequence that ends at index i.

Click here to show hint regarding recurrence relation
Let's say you have an index $j$, where $j < i$. If $nums[i] > nums[j]$, that means we can add onto whatever subsequence ends at index $j$ using $nums[i]$.

Click here to show hint regarding base cases
By default, every number on its own is an increasing subsequence of length 1.



## Coding Question - Word Break

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

 

**Example 1:**

```
Input: s = "leetcode", wordDict = ["leet","code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

**Example 2:**

```
Input: s = "applepenapple", wordDict = ["apple","pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.
```

**Example 3:**

```
Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
Output: false
```

 

**Constraints:**

- `1 <= s.length <= 300`
- `1 <= wordDict.length <= 1000`
- `1 <= wordDict[i].length <= 20`
- `s` and `wordDict[i]` consist of only lowercase English letters.
- All the strings of `wordDict` are **unique**.



## Coding Question - Longest Increasing Subsequence

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

 

**Example 1:**

```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

**Example 2:**

```
Input: nums = [0,1,0,3,2,3]
Output: 4
```

**Example 3:**

```
Input: nums = [7,7,7,7,7,7,7]
Output: 1
```

 

**Constraints:**

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`

 

**Follow up:** Can you come up with an algorithm that runs in `O(n log(n))` time complexity?



##  State Transition by Inaction

This is a small pattern that occasionally shows up in DP problems. Here, "doing nothing" refers to two different states having the same value. We're calling it "doing nothing" because often the way we arrive at a new state with the same value as the previous state is by "doing nothing" (we'll look at some examples soon). Of course, a decision making process needs to coexist with this pattern, because if we just had all states having the same value, the problem wouldn't really make sense ($dp(i) = dp(i - 1)?$) It is just that if we are trying to maximize or minimize a score for example, sometimes the best option is to "do nothing", which leads to two states having the same value. The actual recurrence relation would look something like $dp(i, j) = max(dp(i - 1, j), ...)$.

Usually when we "do nothing", it is by moving to the next element in some input array (which we usually use $i$ as a state variable for). As mentioned above, **this will be part of a decision making process due to some restriction in the problem**. For example, think back to House Robber: we could choose to rob or not rob each house we were at. Sometimes, not robbing the house is the best decision (because we aren't allowed to rob adjacent houses), then $dp(i) = dp(i - 1)$.

In the next article, we'll use the framework to solve a problem with this pattern.



# Example 188. Best Time to Buy and Sell Stock IV



> For this one, we're back to starting with top-down.

In this article, we'll be using the framework to solve [Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/). This problem is rated as "hard" and may seem daunting at first, but with the framework, the logic behind solving this problem is very intuitive. We'll also make use of the pattern of "doing nothing". Like usual, let's use the framework to develop an algorithm:

1. A **function** that answers the problem for a given state

*What information do we need at each state/decision?*

We need to know what day it is (so we can look up the current price of the stock), and we need to know how many transactions we have left. These two are directly related to the input.

The note in the problem description says that we cannot engage in multiple transactions at the same time. This means that at any moment, we are either holding one unit of stock or not holding any stock. We should have a state variable that indicates if we are currently holding stock. This variable is fine as a boolean, but for caching purposes, let's use an integer alternating between $0$ and $1$ ($0$ means not holding, $1$ means holding).

To summarize, we have 3 state variables:

1. $i$, which represents we are on the $i^{th}$ day. The current price of the stock is $prices[i]$.
2. $transactionsRemaining$, which represents how many transactions we have left. This number goes down by 1 whenever we sell a stock.
3. $holding$, which is equal to $0$ if we are not holding a stock, and $1$ if we are holding a stock. If $holding$ is $0$, we have the option to buy a stock. Otherwise, we have the option to sell a stock.

The problem is asking for a maximum achievable profit. Therefore, let's have a function $dp$ where $dp(i, transactionsRemaining, holding)$ returns the maximum achievable profit starting from the $i^{th}$ day with $transactionsRemaining$ transactions remaining, and $holding$ indicating if we start with a stock or not. To answer the original problem, we would return $dp(0, k, 0)$, as we start on day $0$ with $k$ transactions remaining and not holding a stock.

2. A **recurrence relation** to transition between states

At each state, we need to make a decision that depends on what $holding$ is. Let's split it up and look at our options one at a time:

- If we are holding stock, we have two options. We can sell, or not sell. If we choose to sell, we gain $prices[i]$ money, and the next state will be $(i + 1, transactionsRemaining - 1, 0)$. This is because it is the next day ($i + 1$), we lose a transaction as we completed one by selling ($transactionsRemaining - 1$), and we are no longer holding a stock ($0$). In total, our profit is $prices[i] + dp(i + 1, transactionsRemaining - 1, 0)$. If we choose not to sell and **do nothing**, then we just move onto the next day with the same number of transactions, while still holding the stock. Our profit is $dp(i + 1, transactionsRemaining, holding)$.
- If we are not holding stock, we have two options. We can buy, or not buy. If we choose to buy, we lose $prices[i]$ money, and the next state will be $(i + 1, transactionsRemaining, 1)$. This is because it is the next day, we have the same number of transactions because transactions are only completed on selling, and we now hold a stock. In total, our profit is $-prices[i] + dp(i + 1, transactionsRemaining, 1)$. If we choose not to buy and **do nothing**, then we just move onto the next day with the same number of transactions, while still not having stock. Our profit is $dp(i + 1, transactionsRemaining, holding)$.

> Note that you could also set up the solution so that transactions are completed upon buying a stock instead.

Of course, we always want to make the best decision. We can see that in both scenarios, **doing nothing** is the same - $dp(i + 1, transactionsRemaining, holding)$. Therefore, we have a recurrence relation of:

```
dp(i, transactionsRemaining, holding) = max(doNothing, sellStock)` if `holding == 1` otherwise `max(doNothing, buyStock)
```

Where,
`doNothing = dp(i + 1, transactionsRemaining, holding)`,
`sellStock = prices[i] + dp(i + 1, transactionsRemaining - 1, 0)`, and
`buyStock = -prices[i] + dp(i + 1, transactionsRemaining, 1)`.

![img](Dynamic%20Programming.assets/C3A5_1.png)

3. **Base cases**

Both base cases are very simple for this problem. If we are out of transactions ($transactionsRemaining = 0$), then we should immediately return $0$ as we cannot make any more money. If the stock is no longer on the market ($i = prices.length$), then we should also return $0$, as we cannot make any more money.

### Top-down Implementation

~~~
class Solution {
    private int[] prices;
    private int[][][] memo;
    
    private int dp(int i, int transactionsRemaining, int holding) {
        // Base cases
        if (transactionsRemaining == 0 || i == prices.length) {
            return 0;
        }
        
        if (memo[i][transactionsRemaining][holding] == 0) {
            int doNothing = dp(i + 1, transactionsRemaining, holding);
            int doSomething;

            if (holding == 1) {
                // Sell Stock
                doSomething = prices[i] + dp(i + 1, transactionsRemaining - 1, 0);
            } else {
                // Buy Stock
                doSomething = -prices[i] + dp(i + 1, transactionsRemaining, 1);
            }
            
            // Recurrence relation. Choose the most profitable option.
            memo[i][transactionsRemaining][holding] = Math.max(doNothing, doSomething);
        }
        
        return memo[i][transactionsRemaining][holding];
    }
    
    public int maxProfit(int k, int[] prices) {
        this.prices = prices;
        this.memo = new int[prices.length][k + 1][2];
        return dp(0, k, 0);
    }
}
~~~

### Bottom-up Implementation

Again, the recurrence relation is the same with top-down, but we need to be careful about how we configure our for loops. The base cases are automatically handled because the $dp$ array is initialized with all values set to $0$. For iteration direction and order, remember with bottom-up we start at the base cases. Therefore we will start iterating from the end of the input and with only 1 transaction remaining.

~~~
class Solution {
    public int maxProfit(int k, int[] prices) {
        int n = prices.length;
        int dp[][][] = new int[n + 1][k + 1][2];
        
        for (int i = n - 1; i >= 0; i--) {
            for (int transactionsRemaining = 1; transactionsRemaining <= k; transactionsRemaining++) {
                for (int holding = 0; holding < 2; holding++) {
                    int doNothing = dp[i + 1][transactionsRemaining][holding];
                    int doSomething;
                    if (holding == 1) {
                        // Sell stock
                        doSomething = prices[i] + dp[i + 1][transactionsRemaining - 1][0];
                    } else {
                        // Buy stock
                        doSomething = -prices[i] + dp[i + 1][transactionsRemaining][1];
                    }
                    
                    // Recurrence relation
                    dp[i][transactionsRemaining][holding] = Math.max(doNothing, doSomething);
                }
            }
        }
        
        return dp[0][k][0];
    }
}
~~~

The time and space complexity of this problem for both implementations is the number of states since the recurrence relation is just a constant time formula. If $n = prices.length$, then this means the time and space complexity is $O(n⋅k⋅2) = O(n⋅k)$.



### Up Next

We'll conclude this chapter with a practice problem similar to the one we just went through here. Remember the pattern of "doing nothing" and try to solve it yourself. If you get stuck, here are some hints:

309. Best Time to Buy and Sell Stock with Cooldown

Click here to show hint regarding state variables and $dp$
Let $dp[i][status]$ represent the maximum profit achievable starting from the $i^{th}$ day and a certain $status$. There are 3 statuses: currently holding a stock, not holding a stock but free to buy one, and not holding a stock but on cooldown.

Click here to show hint regarding recurrence relation
If you're holding a stock, you can sell it. If you're not holding a stock and not on cooldown, you can buy a stock at the current price. Regardless of your status, you can always do nothing. Choose the most profitable option.

Click here to show hint regarding base cases
If we reach the last index of the input and are holding a stock, sell the stock. Otherwise, return 0 since we won't have any more chances to buy a stock.



## Coding Question - Best Time to Buy and Sell Stock IV

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `k`.

Find the maximum profit you can achieve. You may complete at most `k` transactions.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

 

**Example 1:**

```
Input: k = 2, prices = [2,4,1]
Output: 2
Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
```

**Example 2:**

```
Input: k = 2, prices = [3,2,6,5,0,3]
Output: 7
Explanation: Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
```

 

**Constraints:**

- `0 <= k <= 100`
- `0 <= prices.length <= 1000`
- `0 <= prices[i] <= 1000`



## Coding Question - Best Time to Buy and Sell Stock with Cooldown

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

- After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

 

**Example 1:**

```
Input: prices = [1,2,3,0,2]
Output: 3
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```

**Example 2:**

```
Input: prices = [1]
Output: 0
```

 

**Constraints:**

- `1 <= prices.length <= 5000`
- `0 <= prices[i] <= 1000`



# Common Patterns Continued

##  State Reduction

In an earlier chapter when we used the framework to solve [Maximum Score from Performing Multiplication Operations](https://leetcode.com/explore/learn/card/dynamic-programming/631/strategy-for-solving-dp-problems/4100/), we mentioned that we could use 2 state variables instead of 3 because we could derive the information the 3rd one would have given us from the other 2. By doing this, we greatly reduced the number of states (as we learned earlier, the number of states is the product of the number of values each state variable can take). In most cases, reducing the number of states will reduce the time and space complexity of the algorithm.

This is called **state reduction**, and it is applicable for many DP problems, including a few that we have already looked at. State reduction usually comes from a clever trick or observation. Sometimes, as is in the case of Maximum Score from Performing Multiplication Operations, state reduction can result in lower time and space complexity. Other times, only the space complexity will be improved while the time complexity remains the same.

State reduction can also be achieved in the recurrence relation. Recall when we looked at House Robber. Only one state variable was used, $i$, which indicates what house we are currently at. An alternative way to solve the problem would be adding an extra boolean state variable $prev$ that indicates if we robbed the previous house or not, and that would look something like this:

~~~python
class Solution:
    def rob(self, nums: List[int]) -> int:
        @cache
        def dp(i, prev):
            if i < 0:
                return 0
            ans = dp(i - 1, False)
            if not prev:
                ans = max(ans, dp(i - 1, True) + nums[i])
                
            return ans
        
        return dp(len(nums) - 1, False)
~~~

However, we mentioned in the House Robber article: "*We certainly could include this state variable, but we can develop our recurrence relation in a way that makes it unnecessary.*". By using a clever recurrence relation and base case, we avoided the need for the extra state variable which reduces the number of states by a factor of 2.

> Note: state reductions for space complexity usually only apply to bottom-up implementations, while improving time complexity by reducing the number of state variables applies to both implementations.

When it comes to reducing state variables, it's hard to give any general advice or blueprint. The best advice is to try and think if any of the state variables are related to each other, and if an equation can be created among them. If a problem does not require iteration, there is usually some form of state reduction possible.



Another common scenario where we can improve space complexity is when the recurrence relation is static (no iteration) along one dimension. Let's look back at where we started - [Fibonacci](https://leetcode.com/problems/fibonacci-number/). Recall that the $i^{th}$ Fibonacci number can be calculated with the recurrence relation:

F(i) = F(i - 1) + F(i - 2)*F*(*i*)=*F*(*i*−1)+*F*(*i*−2)

Because this recurrence relation is static, to calculate the $i^{th}$ Fibonacci number, we only ever care about the previous two numbers. That means if we are using a bottom-up approach to find the $n^{th}$ Fibonacci number and start from the base cases, we don't actually need to use an array and remember every single Fibonacci number.

Let's say we wanted F(100)*F*(100). Starting from the base cases, we need to calculate every Fibonacci number from F(2)*F*(2) to F(99)*F*(99), but at the time of the actual calculation for F(100)*F*(100), we only care about F(98)*F*(98) and F(99)*F*(99). The other 90+ Fibonacci numbers aren't needed, so storing all of them is a waste of space.

![img](Dynamic%20Programming.assets/701bf76e-5f2b-49d7-8484-c9c10bc3eaca.png)

![img](Dynamic%20Programming.assets/ccd5186a-c35f-41d0-bba2-067bdefd85a0.png)

![img](Dynamic%20Programming.assets/f83acfdd-c2f8-4804-a48b-8b7c98cd7aad.png)

![img](Dynamic%20Programming.assets/35f4cd79-ba75-4058-a737-a13111463a64.png)

Using only two variables instead, we can improve space complexity to $O(1)$ from $O(n)$ using an array. The time complexity remains the same.

~~~java
class Solution {
    public int fib(int n) {
        if (n <= 1) return n;
        int one_back = 1;
        int two_back = 0;
        
        for (int i = 2; i <= n; i++) {
            int temp = one_back;
            one_back += two_back;
            two_back = temp;
        }
        
        return one_back;
    }
}
~~~



> Whenever you notice that values calculated by a DP algorithm are only reused a few times and then never used again, try to see if you can save on space by replacing an array with some variables. A good first step for this is to look at the recurrence relation to see what previous states are used. For example, in Fibonacci, we only refer to the previous two states, so all results before $n - 2$ can be discarded.



**Up Next**

The next problem, Min Cost Climbing Stairs, was already given as a practice problem in an earlier chapter. This time, try to solve it in $O(n)$ time with $O(1)$ space.



## Coding Question - Min Cost Climbing Stairs

You are given an integer array `cost` where `cost[i]` is the cost of `ith` step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index `0`, or the step with index `1`.

Return *the minimum cost to reach the top of the floor*.

 

**Example 1:**

```
Input: cost = [10,15,20]
Output: 15
Explanation: You will start at index 1.
- Pay 15 and climb two steps to reach the top.
The total cost is 15.
```

**Example 2:**

```
Input: cost = [1,100,1,1,1,100,1,1,100,1]
Output: 6
Explanation: You will start at index 0.
- Pay 1 and climb two steps to reach index 2.
- Pay 1 and climb two steps to reach index 4.
- Pay 1 and climb two steps to reach index 6.
- Pay 1 and climb one step to reach index 7.
- Pay 1 and climb two steps to reach index 9.
- Pay 1 and climb one step to reach the top.
The total cost is 6.
```

 

**Constraints:**

- `2 <= cost.length <= 1000`
- `0 <= cost[i] <= 999`



**Hide Hint #1 **

Say f[i] is the final cost to climb to the top from step i. Then f[i] = cost[i] + min(f[i+1], f[i+2]).



##  Counting DP

Most of the problems we have looked at in earlier chapters ask for either the maximum, minimum, or longest of something. However, it is also very common for a DP problem to ask for the number of distinct ways to do something. In fact, one of the first examples we looked at did this - recall that [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) asked us to find the number of ways to climb to the top of the stairs.

> Another term used to describe this class of problems is "counting DP".

What are the differences with counting DP? With the maximum/minimum problems, the recurrence relation typically involves a $max()$ or $min()$ function. This is true for all types of problems we have looked at - iteration, multi-dimensional, etc. With counting DP, the recurrence relation typically just sums the results of multiple states together. For example, in Climbing Stairs, the recurrence relation was $dp(i) = dp(i - 1) + dp(i - 2)$. There is no $max()$ or $min()$, just addition.

Another difference is in the base cases. In most of the problems we have looked at, if the state goes out of bounds, the base case equals $0$. For example, in the Best Time to Buy and Sell Stock questions, when we ran out of transactions or ran out of days to trade, we returned $0$ because we can't make any more profit. In Longest Common Subsequence, when we run out of characters for either string, we return $0$ because the longest common subsequence of any string and an empty string is $0$. With counting DP, the base cases are often not set to $0$. This is because the recurrence relation usually only involves addition terms with other states, so if the base case was set to $0$ then you would only ever add $0$ to itself. Finding these base cases involves some logical thinking - for example, when we looked at Climbing Stairs - we reasoned that there is $1$ way to climb to the first step and $2$ ways to climb to the second step.

These next 3 problems are very good practice problems that ask for the number of distinct ways to do something. Try them on your own, but if you get stuck, come back here for hints:

276. Paint Fence

Click here to show hint regarding state variables and dp
Let $dp(i)$ represent the number of ways to paint $i$ posts.

Click here to show hint regarding recurrence relation
At each new fence post, we can either paint it the same color as the previous post, or a different color. If we choose a different color, then there are $k - 1$ colors to choose from. If we choose the same color, we need to make sure that the post at $i - 1$ and $i - 2$ are not the same color.

Click here to show hint regarding base cases
There are $k$ ways to paint one fence post. Since we are allowed to paint 2 consecutive fence posts the same color, there are $k⋅k$ ways to paint two fence posts.

518. Coin Change 2

Click here to show hint regarding state variables and dp
Let $dp[i]$ represent the number of ways to make $i$ money.

Click here to show hint regarding recurrence relation
For each coin, for each amount of money $i$, you can make $i$ money by adding the coin to $i - coin$ money. Make sure to stay in bounds.

Click here to show hint regarding base cases
Since our recurrence relation only involves addition with other states, we need $dp[0]$ to be a nonzero value, otherwise we will only ever be adding 0 to itself.

91. Decode Ways

Click here to show hint regarding state variables and dp
Let $dp(i)$ return the number of ways the string starting from index $i$ can be decoded. Return $dp(0)$.

Click here to show hint regarding recurrence relation
You can always treat the current number as a one digit number. In addition, if the current number and next number combined is between 10 and 26, you have an additional option.

Click here to show hint regarding base cases
If $s[i] ==  "0"$, then there is no way for this string to ever be decoded to anything. If $i$ goes out of bounds, we need to return a nonzero value.

## Coding Question - Paint Fence 

Locked

## Coding Question - Coin Change 2

ou are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the number of combinations that make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

 

**Example 1:**

```
Input: amount = 5, coins = [1,2,5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

**Example 2:**

```
Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.
```

**Example 3:**

```
Input: amount = 10, coins = [10]
Output: 1
```

 

**Constraints:**

- `1 <= coins.length <= 300`
- `1 <= coins[i] <= 5000`
- All the values of `coins` are **unique**.
- `0 <= amount <= 5000`



## Coding Question - Decode Ways

A message containing letters from `A-Z` can be **encoded** into numbers using the following mapping:

```
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```

To **decode** an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, `"11106"` can be mapped into:

- `"AAJF"` with the grouping `(1 1 10 6)`
- `"KJF"` with the grouping `(11 10 6)`

Note that the grouping `(1 11 06)` is invalid because `"06"` cannot be mapped into `'F'` since `"6"` is different from `"06"`.

Given a string `s` containing only digits, return *the **number** of ways to **decode** it*.

The test cases are generated so that the answer fits in a **32-bit** integer.

 

**Example 1:**

```
Input: s = "12"
Output: 2
Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).
```

**Example 2:**

```
Input: s = "226"
Output: 3
Explanation: "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

**Example 3:**

```
Input: s = "06"
Output: 0
Explanation: "06" cannot be mapped to "F" because of the leading zero ("6" is different from "06").
```

 

**Constraints:**

- `1 <= s.length <= 100`
- `s` contains only digits and may contain leading zero(s).



## Kadane's Algorithm

[Kadane's Algorithm](https://en.wikipedia.org/wiki/Maximum_subarray_problem#Kadane's_algorithm) is an algorithm that can find the [maximum sum subarray](https://leetcode.com/problems/maximum-subarray/) given an array of numbers in $O(n)$ time and $O(1)$ space. Its implementation is a very simple example of dynamic programming, and the efficiency of the algorithm allows it to be a powerful tool in some DP algorithms. If you haven't already solved Maximum Subarray, take a quick look at the problem before continuing with this article - Kadane's Algorithm specifically solves this problem.

Kadane's Algorithm involves iterating through the array using an integer variable $current$, and at each index $i$, determines if elements before index $i$ are "worth" keeping, or if they should be "discarded". The algorithm is only useful when the array can contain negative numbers. If $current$ becomes negative, it is reset, and we start considering a new subarray starting at the current index.

Pseudocode for the algorithm is below:

```
// Given an input array of numbers "nums",
1. best = negative infinity
2. current = 0
3. for num in nums:
    3.1. current = Max(current + num, num)
    3.2. best = Max(best, current)

4. return best
```

Line 3.1 of the pseudocode is where the magic happens. If $current$ has become less than 0 from including too many or too large negative numbers, the algorithm "throws it away" and resets.

![img](Dynamic%20Programming.assets/fbe7426b-754b-4063-8f1d-fbac683b692e.png)
![img](Dynamic%20Programming.assets/204ca5bf-ce85-4944-9a3c-e070e56cc47a.png)
![img](Dynamic%20Programming.assets/55be8be0-3ad6-4523-8808-1fc5ff34692c.png)
![img](Dynamic%20Programming.assets/5d44b4d3-b59c-4297-9bac-d5a4ae686c1e.png)
![img](Dynamic%20Programming.assets/b834a7b3-4bc8-40ff-b317-992df73f1d10.png)
![img](Dynamic%20Programming.assets/9b14ed43-dcdb-4c7f-a162-5e3e321bd376.png)
![img](Dynamic%20Programming.assets/3b75d322-290d-43cd-8bb9-fe3ca5ee5d66.png)
![img](Dynamic%20Programming.assets/a7907076-caaf-4a55-92e5-f98c7a0fc3dc.png)
![img](Dynamic%20Programming.assets/39d94e81-26a3-4e64-8439-de5ce15994dd.png)
![img](Dynamic%20Programming.assets/8178047e-13cc-48e1-b4d2-e075573d2c63.png)


While usage of Kadane's Algorithm is a niche, variations of Kadane's Algorithm can be used to develop extremely efficient DP algorithms. Try the next two practice problems with this in mind. No framework hints are provided here as implementations of Kadane's Algorithm do not typically follow the framework intuitively, although they are still *technically* dynamic programming (Kadane's Algorithm utilizes optimal sub-structures - it keeps the maximum subarray ending at the previous position in $current$).



##  Best Time to Buy and Sell Stock

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return *the maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return `0`.

 

**Example 1:**

```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
```

**Example 2:**

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
```

 

**Constraints:**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

## Maximum Sum Circular Subarray

Given a **circular integer array** `nums` of length `n`, return *the maximum possible sum of a non-empty **subarray** of* `nums`.

A **circular array** means the end of the array connects to the beginning of the array. Formally, the next element of `nums[i]` is `nums[(i + 1) % n]` and the previous element of `nums[i]` is `nums[(i - 1 + n) % n]`.

A **subarray** may only include each element of the fixed buffer `nums` at most once. Formally, for a subarray `nums[i], nums[i + 1], ..., nums[j]`, there does not exist `i <= k1`, `k2 <= j` with `k1 % n == k2 % n`.

 

**Example 1:**

```
Input: nums = [1,-2,3,-2]
Output: 3
Explanation: Subarray [3] has maximum sum 3.
```

**Example 2:**

```
Input: nums = [5,-3,5]
Output: 10
Explanation: Subarray [5,5] has maximum sum 5 + 5 = 10.
```

**Example 3:**

```
Input: nums = [-3,-2,-3]
Output: -2
Explanation: Subarray [-2] has maximum sum -2.
```

 

**Constraints:**

- `n == nums.length`
- `1 <= n <= 3 * 104`
- `-3 * 104 <= nums[i] <= 3 * 104`



**Hide Hint #1**

For those of you who are familiar with the **Kadane's algorithm**, think in terms of that. For the newbies, Kadane's algorithm is used to finding the maximum sum subarray from a given array. This problem is a twist on that idea and it is advisable to read up on that algorithm first before starting this problem. Unless you already have a great algorithm brewing up in your mind in which case, go right ahead!

**Hide Hint #2**

What is an alternate way of representing a circular array so that it appears to be a straight array? Essentially, there are two cases of this problem that we need to take care of. Let's look at the figure below to understand those two cases:

![img](Dynamic%20Programming.assets/circular_subarray_hint_1.png)

**Hide Hint #3**

The first case can be handled by the good old Kadane's algorithm. However, is there a smarter way of going about handling the second case as well?

## Chapter 4 Quiz

Single Choice Question

For counting DP, base cases should usually be set to 0.

True

False

> The recurrence relation usually only includes addition terms between states, so if the base cases were set to 0, then all states would be stuck at 0.



------



Single Choice Question

We can reduce space complexity when:

We are using top-down and not bottom-up

The recurrence relation only involves a fixed number of states

Only when we have more than 1 state variable

> Usually, reductions involve only temporarily caching results to subproblems. Once we know that the result of a subproblem will never be used again, we can discard it. This is a common optimization in bottom-up implementations. Because we iterate over all of the states in an orderly manner, we can determine which cached results will not be used again based on the recurrence relation and the current state.



------



Single Choice Question

On its own, Kadane's Algorithm finds:

Maximum subarray sum

Average number in subarray

Largest number in subarray

> Kadane's Algorithm can be extended to create very efficient algorithms.



# DP for Paths in a Matrix

## Pathing Problems

The last pattern we'll be looking at is pathing problems on a matrix. These problems have matrices as part of the input and give rules for "moving" through the matrix in the problem description. Typically, DP will be applicable when the allowed movement is constrained in a way that prevents moving "backwards", for example if we are only allowed to move down and right.



![img](Dynamic%20Programming.assets/1293_next_steps.png)





If we are allowed to move in all 4 directions, then it might be a graph/BFS problem instead. This pattern is sometimes combined with other patterns we have looked at, such as counting DP.

In terms of difficulty, these problems are usually less difficult than the average DP problem as the recurrence relation is usually directly related to the rules of traversal. Most of these problems are also very similar or are variations of each other, and because of this, knowing a general approach to these problems can go a long way.

Let's walk through one last example with the framework, and then finish this card with a few good practice problems.

## Example 62. Unique Paths

> The bottom-up approach for pathing problems is often more intuitive than bottom-up for other types of dynamic programming problems - so this is a good chance for us to practice starting with a bottom-up approach.

In this article, we'll use the framework to solve [Unique Paths](https://leetcode.com/problems/unique-paths/). This problem asks us to find the number of distinct ways to do something, which is a hint that we should consider using dynamic programming, and we discussed how to do so in the previous chapter. Let's get started:

1. An **array** that answers the problem for a given state

State variables are usually easy to find in pathing problems. Similar to how we need one index ($i$) for 1D array inputs, with pathing problems on a 2D matrix, we need two indices ($row$ and $col$) to denote position. Some problems have added constraints that will require additional state variables, but there doesn't seem to be anything of the sort in this problem. Therefore, we will just use two state variables $row$ which represents the current row, and $col$ which represents the current column.

The problem is asking for the number of paths to the final square, so let's have $dp[row][col]$ represent how many paths there are from the start (top-left corner) to the square at $(row, col)$. We will return $dp[m - 1][n - 1]$ where $m$ and $n$ are the number of rows and columns respectively.

2. A **recurrence relation** to transition between states

The problem says that we are allowed to move down or right. That means, if we are at some square, we arrived from either the square above or the square to the left. These two squares are $(row - 1, col)$ and $(row, col - 1)$. Since we can arrive at the current square from either of these squares, the number of ways to get to the current square is the sum of the number of ways to get to these two squares. Either of these may be out of the grid bounds, so we should make sure to check for that. This gives us our simple recurrence relation:

$dp[row][col] = dp[row - 1][col] + dp[row][col - 1]$, where $dp[row - 1][col]$ and $dp[row][col - 1]$ is equal to 0 if out of bounds.

3. **Base cases**

In the previous chapter, when talking about counting DP problems, we said that the base cases need to be set to nonzero values so that the terms in the recurrence relation don't just stay stuck at zero. In this problem, we start in the top-left corner. How many ways are there for us to get to the first square? Only 1 - we start on it. Therefore, our base case is $dp[0][0] = 1$.

> Note: If you have trouble coming up with the recurrence relation, sometimes it helps to come up with the base case(s) first. Then walk through how you would find the result for states that are slightly more complicated than the base case(s), such as $dp[0][1]$, $dp[1][1]$, and $dp[2][1]$. Often, this process of manually solving the problem for simple states can help you understand what the recurrence relation should be.



### Bottom-up Implementation

Putting it all together for the final solution:

~~~java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        dp[0][0] = 1; // Base case
        
        for (int row = 0; row < m; row++) {
            for (int col = 0; col < n; col++) {
                if (row > 0) {
                    dp[row][col] += dp[row - 1][col];
                }
                if (col > 0) {
                    dp[row][col] += dp[row][col - 1];
                }
            }
        }
        
        return dp[m - 1][n - 1];
    }
}
~~~

Here's an animation showing the algorithm in action:

![img](Dynamic%20Programming.assets/a351ee7f-c3df-4d6d-ba50-a66575511b9a.png)
![img](Dynamic%20Programming.assets/81517792-5e2a-4e42-99a5-d0b98fbc2f9e.png)
![img](Dynamic%20Programming.assets/4bf859d6-68ed-4ff4-b9bc-45726e635936.png)
![img](Dynamic%20Programming.assets/4314fe87-80d6-4efe-88a4-f98db5379b1a.png)
![img](Dynamic%20Programming.assets/19e859e8-f44d-49b8-a770-e8483decd3b7.png)
![img](Dynamic%20Programming.assets/df8e0ccb-40de-4c88-be9f-475435c00070.png)
![img](Dynamic%20Programming.assets/3793ed67-4afd-4d62-b904-88cae1f4a97e.png)
![img](Dynamic%20Programming.assets/4d4a84b8-9f93-4097-ab1f-c8376233f230.png)
![img](Dynamic%20Programming.assets/42c3f470-07a7-4c09-8f06-d92735095b84.png)
![img](Dynamic%20Programming.assets/481e04e2-6911-467b-b009-7fc73157b341.png)

### Top-down Implementation

As we know, one of the advantages of top-down implementations is that they are usually easier to implement. However, for some, bottom-up dynamic programming is more intuitive when it comes to pathing problems. As such, we implemented the bottom-up approach first in this example. That said, it is rare to convert bottom-up solutions to top-down solutions since the typical flow for dynamic programming problems is to come up with a top-down DP solution first, and then convert it to a bottom-up DP solution, if the bottom-up approach is more efficient. Below, for completeness, we have also included the top-down implementation.

~~~java
class Solution {
    private int[][] memo;
    
    private int dp(int row, int col) {
        if (row + col == 0) {
            return 1; // Base case
        }
        
        int ways = 0;
        if (memo[row][col] == 0) {
            if (row > 0) {
                ways += dp(row - 1, col);
            }
            if (col > 0) {
                ways += dp(row, col - 1);
            }
            
            memo[row][col] = ways;
        }
        
        return memo[row][col];
    }
    
    public int uniquePaths(int m, int n) {
        memo = new int[m][n];
        return dp(m - 1, n- 1);
    }
}
~~~

The time and space complexity of both solutions is $O(m⋅n)$. We visit each square only once and do a constant amount of work. Here's a challenge to do on your own: try to use state reduction to improve the space complexity of the bottom-up approach to $O(n)$ without modifying the input.

### Up next

As always, try the next 3 practice problems on your own, and come back here if you need hints:

63. Unique Paths II

Click here to show hint regarding state variables and $dp$
Let $dp[row][col]$ represent how many paths there are from the start to the square at $(row, col)$.

Click here to show hint regarding recurrence relation
We arrive at each square from the squares at $(row - 1, col)$ and $(row, col - 1)$, if they're in bounds. Make sure that they aren't obstacles, and that the square we currently are on isn't an obstacle either.

Click here to show hint regarding base cases
There is 1 "path" to the starting square - $dp[0][0] = 1$.

64. Minimum Path Sum

Click here to show hint regarding state variables and $dp$
Let $dp[row][col]$ represent the smallest sum possible for any path from the start to the square at $(row, col)$.

Click here to show hint regarding recurrence relation
We arrive at each square from the squares at $(row - 1, col)$ and $(row, col - 1)$, if they're in bounds. Choose the smallest value and add it to the current square's value.

Click here to show hint regarding base cases
Initially, $dp = grid$.

931. Minimum Falling Path Sum

Click here to show hint regarding state variables and $dp$
Let $dp[row][col]$ represent the smallest sum possible for any path from the start to the square at $(row, col)$.

Click here to show hint regarding recurrence relation
We arrive at each square from the squares at $(row - 1, col - 1)$, $(row - 1, col)$, and $(row - 1, col + 1)$, if they're in bounds. Choose the smallest value and add it to the current square's value.

Click here to show hint regarding base cases
Initially, $dp = matrix$.



## Coding Question - Unique Paths

There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return *the number of possible unique paths that the robot can take to reach the bottom-right corner*.

The test cases are generated so that the answer will be less than or equal to `2 * 109`.

 

**Example 1:**

![img](Dynamic%20Programming.assets/robot_maze.png)

```
Input: m = 3, n = 7
Output: 28
```

**Example 2:**

```
Input: m = 3, n = 2
Output: 3
Explanation: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down
```

 

**Constraints:**

- `1 <= m, n <= 100`

## Coding Question - Unique Paths II

A robot is located at the top-left corner of a `m x n` grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and space is marked as `1` and `0` respectively in the grid.

 

**Example 1:**

![img](Dynamic%20Programming.assets/robot1.jpg)

```
Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
Output: 2
Explanation: There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```

**Example 2:**

![img](Dynamic%20Programming.assets/robot2.jpg)

```
Input: obstacleGrid = [[0,1],[0,0]]
Output: 1
```

 

**Constraints:**

- `m == obstacleGrid.length`
- `n == obstacleGrid[i].length`
- `1 <= m, n <= 100`
- `obstacleGrid[i][j]` is `0` or `1`.



**Hide Hint #1 **

The robot can only move either down or right. Hence any cell in the first row can only be reached from the cell left to it. However, if any cell has an obstacle, you don't let that cell contribute to any path. So, for the first row, the number of ways will simply be

```
if obstacleGrid[i][j] is not an obstacle
     obstacleGrid[i,j] = obstacleGrid[i,j - 1] 
else
     obstacleGrid[i,j] = 0
```

You can do a similar processing for finding out the number of ways of reaching the cells in the first column.

**Hide Hint #2 **

For any other cell, we can find out the number of ways of reaching it, by making use of the number of ways of reaching the cell directly above it and the cell to the left of it in the grid. This is because these are the only two directions from which the robot can come to the current cell.

**Hide Hint #3 **

Since we are making use of pre-computed values along the iteration, this becomes a dynamic programming problem.

```
if obstacleGrid[i][j] is not an obstacle
     obstacleGrid[i,j] = obstacleGrid[i,j - 1]  + obstacleGrid[i - 1][j]
else
     obstacleGrid[i,j] = 0
```



## Coding Question - Minimum Path Sum

Given a `m x n` `grid` filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

 

**Example 1:**

![img](Dynamic%20Programming.assets/minpath.jpg)

```
Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
Output: 7
Explanation: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.
```

**Example 2:**

```
Input: grid = [[1,2,3],[4,5,6]]
Output: 12
```

 

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `0 <= grid[i][j] <= 100`

## Coding Question - Minimum Falling Path Sum

Given an `n x n` array of integers `matrix`, return *the **minimum sum** of any **falling path** through* `matrix`.

A **falling path** starts at any element in the first row and chooses the element in the next row that is either directly below or diagonally left/right. Specifically, the next element from position `(row, col)` will be `(row + 1, col - 1)`, `(row + 1, col)`, or `(row + 1, col + 1)`.

 

**Example 1:**

![img](Dynamic%20Programming.assets/failing1-grid.jpg)

```
Input: matrix = [[2,1,3],[6,5,4],[7,8,9]]
Output: 13
Explanation: There are two falling paths with a minimum sum as shown.
```

**Example 2:**

![img](Dynamic%20Programming.assets/failing2-grid.jpg)

```
Input: matrix = [[-19,57],[-40,-5]]
Output: -59
Explanation: The falling path with a minimum sum is shown.
```

 

**Constraints:**

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 100`
- `-100 <= matrix[i][j] <= 100`



# More Practice Problem

## Coding Question -Best Time to Buy and Sell Stock with Transaction Fee

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `fee` representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

 

**Example 1:**

```
Input: prices = [1,3,2,8,4,9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
- Buying at prices[0] = 1
- Selling at prices[3] = 8
- Buying at prices[4] = 4
- Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```

**Example 2:**

```
Input: prices = [1,3,7,5,10,3], fee = 3
Output: 6
```

 

**Constraints:**

- `1 <= prices.length <= 5 * 104`
- `1 <= prices[i] < 5 * 104`
- `0 <= fee < 5 * 104`



**Hide Hint #1 ****

Consider the first K stock prices. At the end, the only legal states are that you don't own a share of stock, or that you do. Calculate the most profit you could have under each of these two cases.



## Coding Question - Paint House

Locked

## Coding Question - Paint House II

Locked

## Coding Question - Paint House III

There is a row of `m` houses in a small city, each house must be painted with one of the `n` colors (labeled from `1` to `n`), some houses that have been painted last summer should not be painted again.

A neighborhood is a maximal group of continuous houses that are painted with the same color.

- For example: `houses = [1,2,2,3,3,2,1,1]` contains `5` neighborhoods `[{1}, {2,2}, {3,3}, {2}, {1,1}]`.

Given an array `houses`, an `m x n` matrix `cost` and an integer `target` where:

- `houses[i]`: is the color of the house `i`, and `0` if the house is not painted yet.
- `cost[i][j]`: is the cost of paint the house `i` with the color `j + 1`.

Return *the minimum cost of painting all the remaining houses in such a way that there are exactly* `target` *neighborhoods*. If it is not possible, return `-1`.

 

**Example 1:**

```
Input: houses = [0,0,0,0,0], cost = [[1,10],[10,1],[10,1],[1,10],[5,1]], m = 5, n = 2, target = 3
Output: 9
Explanation: Paint houses of this way [1,2,2,1,1]
This array contains target = 3 neighborhoods, [{1}, {2,2}, {1,1}].
Cost of paint all houses (1 + 1 + 1 + 1 + 5) = 9.
```

**Example 2:**

```
Input: houses = [0,2,1,2,0], cost = [[1,10],[10,1],[10,1],[1,10],[5,1]], m = 5, n = 2, target = 3
Output: 11
Explanation: Some houses are already painted, Paint the houses of this way [2,2,1,2,2]
This array contains target = 3 neighborhoods, [{2,2}, {1}, {2,2}]. 
Cost of paint the first and last house (10 + 1) = 11.
```

**Example 3:**

```
Input: houses = [3,1,2,3], cost = [[1,1,1],[1,1,1],[1,1,1],[1,1,1]], m = 4, n = 3, target = 3
Output: -1
Explanation: Houses are already painted with a total of 4 neighborhoods [{3},{1},{2},{3}] different of target = 3.
```

 

**Constraints:**

- `m == houses.length == cost.length`
- `n == cost[i].length`
- `1 <= m <= 100`
- `1 <= n <= 20`
- `1 <= target <= m`
- `0 <= houses[i] <= n`
- `1 <= cost[i][j] <= 104`



**Hide Hint #1 **

Use Dynamic programming.

**Hide Hint #2 **

Define dp[i][j][k] as the minimum cost where we have k neighborhoods in the first i houses and the i-th house is painted with the color j.



## Coding Question - Count Vowels Permutation

Given an integer `n`, your task is to count how many strings of length `n` can be formed under the following rules:

- Each character is a lower case vowel (`'a'`, `'e'`, `'i'`, `'o'`, `'u'`)
- Each vowel `'a'` may only be followed by an `'e'`.
- Each vowel `'e'` may only be followed by an `'a'` or an `'i'`.
- Each vowel `'i'` **may not** be followed by another `'i'`.
- Each vowel `'o'` may only be followed by an `'i'` or a `'u'`.
- Each vowel `'u'` may only be followed by an `'a'.`

Since the answer may be too large, return it modulo `10^9 + 7.`

 

**Example 1:**

```
Input: n = 1
Output: 5
Explanation: All possible strings are: "a", "e", "i" , "o" and "u".
```

**Example 2:**

```
Input: n = 2
Output: 10
Explanation: All possible strings are: "ae", "ea", "ei", "ia", "ie", "io", "iu", "oi", "ou" and "ua".
```

**Example 3:** 

```
Input: n = 5
Output: 68
```

 

**Constraints:**

- `1 <= n <= 2 * 10^4`



  **Hide Hint #1 **

Use dynamic programming.

  **Hide Hint #2 **

Let dp[i][j] be the number of strings of length i that ends with the j-th vowel.

  **Hide Hint #3 **

Deduce the recurrence from the given relations between vowels.

## Coding Question - Maximum Length of Repeated Subarray

Given two integer arrays `nums1` and `nums2`, return *the maximum length of a subarray that appears in **both** arrays*.

 

**Example 1:**

```
Input: nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
Output: 3
Explanation: The repeated subarray with maximum length is [3,2,1].
```

**Example 2:**

```
Input: nums1 = [0,0,0,0,0], nums2 = [0,0,0,0,0]
Output: 5
```

 

**Constraints:**

- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 100`



  **Hide Hint #1 **

Use dynamic programming. dp[i][j] will be the answer for inputs A[i:], B[j:].



## Coding Question - Number of Dice Rolls With Target Sum

You have `n` dice and each die has `k` faces numbered from `1` to `k`.

Given three integers `n`, `k`, and `target`, return *the number of possible ways (out of the* `kn` *total ways)* *to roll the dice so the sum of the face-up numbers equals* `target`. Since the answer may be too large, return it **modulo** `109 + 7`.

 

**Example 1:**

```
Input: n = 1, k = 6, target = 3
Output: 1
Explanation: You throw one die with 6 faces.
There is only one way to get a sum of 3.
```

**Example 2:**

```
Input: n = 2, k = 6, target = 7
Output: 6
Explanation: You throw two dice, each with 6 faces.
There are 6 ways to get a sum of 7: 1+6, 2+5, 3+4, 4+3, 5+2, 6+1.
```

**Example 3:**

```
Input: n = 30, k = 30, target = 500
Output: 222616187
Explanation: The answer must be returned modulo 109 + 7.
```

 

**Constraints:**

- `1 <= n, k <= 30`
- `1 <= target <= 1000`



  **Hide Hint #1 **

Use dynamic programming. The states are how many dice are remaining, and what sum total you have rolled so far.

## Coding Question - Domino and Tromino Tiling

You have two types of tiles: a `2 x 1` domino shape and a tromino shape. You may rotate these shapes.

![img](Dynamic%20Programming.assets/lc-domino.jpg)

Given an integer n, return *the number of ways to tile an* `2 x n` *board*. Since the answer may be very large, return it **modulo** `109 + 7`.

In a tiling, every square must be covered by a tile. Two tilings are different if and only if there are two 4-directionally adjacent cells on the board such that exactly one of the tilings has both squares occupied by a tile.

 

**Example 1:**

![img](Dynamic%20Programming.assets/lc-domino1.jpg)

```
Input: n = 3
Output: 5
Explanation: The five different ways are show above.
```

**Example 2:**

```
Input: n = 1
Output: 1
```

 

**Constraints:**

- `1 <= n <= 1000`



## Coding Question - Minimum Cost For Tickets

You have planned some train traveling one year in advance. The days of the year in which you will travel are given as an integer array `days`. Each day is an integer from `1` to `365`.

Train tickets are sold in **three different ways**:

- a **1-day** pass is sold for `costs[0]` dollars,
- a **7-day** pass is sold for `costs[1]` dollars, and
- a **30-day** pass is sold for `costs[2]` dollars.

The passes allow that many days of consecutive travel.

- For example, if we get a **7-day** pass on day `2`, then we can travel for `7` days: `2`, `3`, `4`, `5`, `6`, `7`, and `8`.

Return *the minimum number of dollars you need to travel every day in the given list of days*.

 

**Example 1:**

```
Input: days = [1,4,6,7,8,20], costs = [2,7,15]
Output: 11
Explanation: For example, here is one way to buy passes that lets you travel your travel plan:
On day 1, you bought a 1-day pass for costs[0] = $2, which covered day 1.
On day 3, you bought a 7-day pass for costs[1] = $7, which covered days 3, 4, ..., 9.
On day 20, you bought a 1-day pass for costs[0] = $2, which covered day 20.
In total, you spent $11 and covered all the days of your travel.
```

**Example 2:**

```
Input: days = [1,2,3,4,5,6,7,8,9,10,30,31], costs = [2,7,15]
Output: 17
Explanation: For example, here is one way to buy passes that lets you travel your travel plan:
On day 1, you bought a 30-day pass for costs[2] = $15 which covered days 1, 2, ..., 30.
On day 31, you bought a 1-day pass for costs[0] = $2 which covered day 31.
In total, you spent $17 and covered all the days of your travel.
```

 

**Constraints:**

- `1 <= days.length <= 365`
- `1 <= days[i] <= 365`
- `days` is in strictly increasing order.
- `costs.length == 3`
- `1 <= costs[i] <= 1000`



## Coding Question - Interleaving String

Given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an **interleaving** of `s1` and `s2`.

An **interleaving** of two strings `s` and `t` is a configuration where they are divided into **non-empty** substrings such that:

- `s = s1 + s2 + ... + sn`
- `t = t1 + t2 + ... + tm`
- `|n - m| <= 1`
- The **interleaving** is `s1 + t1 + s2 + t2 + s3 + t3 + ...` or `t1 + s1 + t2 + s2 + t3 + s3 + ...`

**Note:** `a + b` is the concatenation of strings `a` and `b`.

 

**Example 1:**

![img](Dynamic%20Programming.assets/interleave.jpg)

```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true
```

**Example 2:**

```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
Output: false
```

**Example 3:**

```
Input: s1 = "", s2 = "", s3 = ""
Output: true
```

 

**Constraints:**

- `0 <= s1.length, s2.length <= 100`
- `0 <= s3.length <= 200`
- `s1`, `s2`, and `s3` consist of lowercase English letters.

 

**Follow up:** Could you solve it using only `O(s2.length)` additional memory space?

