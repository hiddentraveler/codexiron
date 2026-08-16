---
date: 2026-08-16T18:24:12+05:30
lastmod: 2025-06-07
showTableOfContents: true
title: Dynamic Programming
draft: "false"
tags:
  - DSA
  - DP
---

Dynamic Programming (DP) is a powerful algorithmic technique used to solve problems by breaking them down into smaller sub-problems, solving each subproblem only once, and storing their results (memoization) to avoid redundant work.

---

## Why use Dynamic Programming?

Instead of recalculating the result for the same input multiple times (like in plain recursion), DP saves the result in memory and reuses it, leading to a significant performance boost.

### When to use DP:
Use DP when a problem has:
1. **Overlapping sub-problems** → sub-problems are solved multiple times (e.g., Fibonacci). → TO deal with it we store solutions of the sub-problem in array/matrix.
	![Overlapping sub-problems](Pasted%20image%2020260816114217.png)
		*We can see that there are multiple times we need to solve f[1],f[2]...etc.*


	![Fibbonaci stack call](Pasted%20image%2020260816115751.png)
	*If we store the result of each function we save computation on redundant function calls in grey*

2. **Optimal Substructure** → Optimal solution of the problem depends on the optimal solutions of its sub-problems. 
	![Fibonacci formula](Pasted%20image%2020260816114840.png)
	*We can see that the solution for f[n] is derived from match of sub-problems f[n-1] and f[n-2]*

---

## Types of DP

| Type                   | Description                                             | Example Problem                      |
| :--------------------- | :------------------------------------------------------ | :----------------------------------- |
| Top-Down (Memoization) | Use recursion + store results in a table                | Fibonacci using recursion and `dp[]` |
| Bottom-Up (Tabulation) | Solve sub-problems iteratively, build solution from base | Fibonacci using a loop and `dp[]`    |
| Space Optimization     | Optimize space by storing only required results         | Fibonacci using two variables        |

---

## 1. Top-Down Approach (Memoization)

### Why this name?
- **"Top-Down"**: We start from the main problem and break it down into sub-problems recursively.
- **"Memoization"**: We store answers to sub-problems in a table (`dp[]`) to avoid recomputation.

### How to convert Recursion solution to Top-Down DP solution ?

**The following steps will always hold true no matter what.**
### Recursive solution

```java
int solve(int n, int[] dp) {
    if (n == 0 || n == 1) return n;
    return solve(n - 1, dp) + solve(n - 2, dp);
}
```

**Step 1**: Find out is it 1D/2D/3D dp.

> - How many  variables are being modified provided in the parameter in the recursive function that number is the type of dp. 
> - Initialize dp array/matrix according to the type of dp with -1/0/MIN/MAX values according to the need of the question. ( Consider this throughly before moving to next stage)

**Step 2**: Store the result of each recursive function in the dp array/matrix.

**Step 3**: If the answer is in dp array → return answer from dp array. 
>- This will always be after base case check.

### Top-Down DP solution (Recursive + Cache)

```java
public class Main {
    static int solve(int n, int[] dp) {
        // Step 1:
        // Only 'n' is being modified in the recursive function parameter,
        // so this is a 1D DP problem.

        // Base case for recursion
        if (n == 0 || n == 1) {
            return n;
        }
        // Step 3:
        // Check if the subproblem is already solved.
        // This must always be after the base case check.
        if (dp[n] != -1) {
            return dp[n];
        }
        // Step 2:
        // Store the result of the current subproblem in dp[n].
        return dp[n] = solve(n - 1, dp) + solve(n - 2, dp);
    }

    public static void main(String[] args) {
        int n = 10;
        // Step 1:
        // Initialize the 1D dp array with -1.
        // -1 means that the subproblem has not been solved yet.
        int[] dp = new int[n + 1];
        Arrays.fill(dp, -1);
        // Call the Top-Down DP function.
        int answer = solve(n, dp);
        System.out.println("Fibonacci(" + n + ") = " + answer);
    }
}
```

---

## 2. Bottom-Up Approach (Tabulation)

### Why this name?
- **"Bottom-Up"**: We solve smaller sub-problems first, and use them to solve bigger problems.
- **"Tabulation"**: We use a table (array) to store the result in a bottom-up manner.

### How to convert Top-Down DP solution to Bottom-Up DP solution ?
### Top-Down DP solution

```java
int solve(int n, int[] dp) {
    int dp = new int[n+1];
    Arrays.fill(dp,-1);
    
    if (n == 0 || n == 1) return n; 
    
    if (dp[n] != -1) return dp[n];
    
    return dp[n] = solve(n - 1, dp) + solve(n - 2, dp);
}
```

**Step 1**: Find DP type → 1D/2D/3D → Inillialize array -> -1/0/MIN/MAX ( Same as previous step 1).

**Step 2**: Analyse base cases and fill it in dp array.

**Step 3**: Reverse the flow from n→0 to 0→n;  copy paste previous top-Down logic to for loop.
>- Convert function calls to array lookup: solve(n-1) → dp[n-1]
> - Change looping variable.

**Step 4**: Return dp[n].

### Bottom-Up DP solution

```java
int solve(int n) {
    // Step 1: Only 'n' changes -> 1D DP.
    int[] dp = new int[n + 1];
    // we still have this because of n=0 then we can't fill dp[1]=1.
    // to escape such edge cases we return early
    if (n == 0 || n == 1) return n;
    
    // Step 2: Fill the base cases.
    dp[0] = 0;
	dp[1] = 1;
    // Step 3: Reverse Top-Down flow (n -> 0) into Bottom-Up (0 -> n).
    for (int i = 2; i <= n; i++) {
        // Top-Down:
        // dp[n] = solve(n - 1, dp) + solve(n - 2, dp);
        //
        // Replace recursive calls with dp lookups
        // and replace n with looping variable i.
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    // Step 4: Return the answer for the original problem.
    return dp[n];
}
```

---

## 3. Space Optimization

### Why this name?
We optimize space usage by noticing that we don't need the whole `dp[]` table – only a few previous values.

### Bottom-Up solution
```java
int solve(int n) {
    int[] dp = new int[n + 1];
    if (n == 0 || n == 1) return n;
    
    dp[0] = 0;
	dp[1] = 1;
	
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
}
```

### How to convert Bottom-Up solution to Space Optimized solution ?
**Step 1**: Convert the base cases into the variables like prev1,prev2.

**Step 2**: Instead of dp array inilialize a current value variable.

**Step 3**: Convert the logic of the for loop to be variable based.

**Step 4**: Shift the prev1 → prev2 and curr → prev1.

**Step 5**: Return curr as the answer.
### Space Optimized solution

```java
int solve(int n) {
    if (n == 0 || n == 1) return n;
    // Step 1:
    // Convert the base cases from the dp array into variables.
    // We use prev2 for i - 2 and prev1 for i - 1.
    
    int prev2 = 0;  // dp[0]
    int prev1 = 1;  // dp[1]
    
    // Step 2:
    // We no longer need the dp array.
    // We only need to store the current value.
    int curr = -1;
    for (int i = 2; i <= n; i++) {
    // Step 3:
    // Convert the Bottom-Up dp logic into variable-based logic.
        curr = prev1 + prev2;

        // Step 4:
        // Shift the variables for the next iteration.
        // prev2 -> prev1
        // prev1 -> curr
        prev2 = prev1;
        prev1 = curr;
    }
    // Step 5:
    // curr contains the answer for n.
    return curr;
}
```

---

## Complexity Comparison

| Approach | Time Complexity | Space Complexity | Notes |
|:---|:---|:---|:---|
| Top-Down (Memoization) | O(n) | O(n) | Uses recursion + dp[] |
| Bottom-Up (Tabulation) | O(n) | O(n) | Iterative, no recursion |
| Space Optimization | O(n) | O(1) | Best for linear recurrence problems |

---

## DP Problem List

| Problem | DP Type | LeetCode Link |
|:---|:---|:---|
| Fibonacci Number | 1D DP | [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) |
| Climbing Stairs | 1D DP | [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) |
| Min Cost Climbing Stairs | 1D DP | [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/) |
| House Robber | 1D DP | [198. House Robber](https://leetcode.com/problems/house-robber/) |
| Unique Paths | 2D Grid DP | [62. Unique Paths](https://leetcode.com/problems/unique-paths/) |
| Minimum Path Sum | 2D Grid DP | [64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/) |
| Longest Common Subsequence | 2D DP on Strings | [1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) |
| Longest Palindromic Subsequence | 2D DP on Strings | [516. Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/) |
| Partition Equal Subset Sum | 0/1 Knapsack (1D DP) | [416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/) |
| Combination Sum IV | Subset Sum / Count Ways | [377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/) |
| Target Sum | 0/1 Knapsack / Subset Sum | [494. Target Sum](https://leetcode.com/problems/target-sum/) |
| Edit Distance | 2D DP on Strings | [72. Edit Distance](https://leetcode.com/problems/edit-distance/) |
| Dungeon Game | 2D DP (Reverse Build) | [174. Dungeon Game](https://leetcode.com/problems/dungeon-game/) |
| Jump Game II | 1D DP + Greedy | [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/) |
| Cherry Pickup II | 3D DP (Advanced Grid) | [1463. Cherry Pickup II](https://leetcode.com/problems/cherry-pickup-ii/) |

---