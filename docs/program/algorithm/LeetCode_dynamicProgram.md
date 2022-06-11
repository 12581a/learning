# 递归与动态规划

### [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**注意：**给定 *n* 是一个正整数。

```java
class Solution {
    public int climbStairs(int n) {
       int[] memory = new int[n+1];
       return climb(n, memory); 
    }
    public int climb(int n, int[] memory){
        if(memory[n] > 0){
            return memory[n];
        }
        if(n <= 2){
            memory[n] = n;
        }else{
            memory[n] = climb(n - 1, memory) + climb(n - 2, memory);
        }
        return memory[n];
    }
}

class Solution {
    public int climbStairs(int n) {
        if(n == 1 || n == 0){
            return n;
        }
        int[] memory = new int[n + 1];
        memory[1] = 1;
        memory[2] = 2;
        for(int i = 3;i <= n; i++){
            memory[i] = memory[i - 1] + memory[i - 2];
        }
        return memory[n];
    }
}
```

### [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

 数字 *n* 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。 

```java
class Solution {

    private List<String> list;

    public List<String> generateParenthesis(int n) {
        list = new ArrayList<>();
        generate(0, 0, n, "");
        return list;

    }
    public void generate(int left, int right, int n, String s){
        if(left == n && right == n){
            list.add(s);
            return;
        }
        if(left < n){
            generate(left + 1, right, n, s+"(");
        }
        if(left > right){
           generate(left, right + 1, n, s+")");
        }
    }
}
```

### [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明:** 叶子节点是指没有子节点的节点。

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);
        return java.lang.Math.max(leftDepth,rightDepth)+1;
    }
}
```

#### [剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

 定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。 

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        ListNode reList = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return reList;
    }
}
```

### [62. 不同路径](https://leetcode-cn.com/problems/unique-paths/)

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

问总共有多少条不同的路径？

```java
class Solution {
    public int uniquePaths(int m, int n) {
        if( m <= 0 || n <= 0){
            return 0;
        }
        int[][] res = new int[m][n];
        for(int i = 0;i < m; i++){
            res[i][0] = 1;
        }
        for(int j = 0;j < n; j++){
            res[0][j] = 1;
        }

        for(int i = 1; i < m; i++){
            for(int j = 1; j < n; j++){
                res[i][j] = res[i - 1][j] + res[i][j - 1];
            }
        }
        return res[m - 1][n - 1];
    }
}
```

### [64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)

给定一个包含非负整数的 *m* x *n* 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

```java
class Solution {
    public int minPathSum(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int rows = grid.length, columns = grid[0].length;
        int[][] dp = new int[rows][columns];
        dp[0][0] = grid[0][0];
        for (int i = 1; i < rows; i++) {
            dp[i][0] = dp[i - 1][0] + grid[i][0];
        }
        for (int j = 1; j < columns; j++) {
            dp[0][j] = dp[0][j - 1] + grid[0][j];
        }
        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < columns; j++) {
                dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
            }
        }
        return dp[rows - 1][columns - 1];
    }
}
```

### [连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int res = nums[0];
        for(int i = 1; i < nums.length; i++) {
            nums[i] += Math.max(nums[i - 1], 0);
            res = Math.max(res, nums[i]);
        }
        return res;
    }
}
```

### [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

给定一个无序的整数数组，找到其中最长上升子序列的长度。

```java
public class Solution {
    public int lengthOfLIS(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        int[] dp = new int[nums.length];
        dp[0] = 1;
        int maxans = 1;
        for (int i = 1; i < dp.length; i++) {
            int maxval = 0;
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    maxval = Math.max(maxval, dp[j]);
                }
            }
            dp[i] = maxval + 1;
            maxans = Math.max(maxans, dp[i]);
        }
        return maxans;
    }
}
```

### [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int res[][] = new int[n + 1][n + 1];
        for(int i = n - 1; i >= 0; i--){
            for(int j = 0; j <= i ; j++){
                res[i][j] = Math.min(res[i + 1][j + 1],res[i + 1][j]) 
                + triangle.get(i).get(j);
            }
        }
        return res[0][0];
    }
}
```


































































