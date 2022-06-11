# 数组

### [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。 

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return buildTree(nums, 0 ,nums.length);
    }

    public TreeNode buildTree(int[] nums,int l,int r){
        
        if(l == r){
            return null;
        }
        int mid = l + (r - l) / 2;
        
        TreeNode node = new TreeNode(nums[mid]);
        
        node.left = buildTree(nums,l,mid);
        node.right = buildTree(nums,mid + 1,r);
        return node;
    }
}
```

### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

 给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。 

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> map = new HashMap<>();

        for(int i = 0;i < nums.length;i++){
            int temp = target - nums[i];
            if(map.containsKey(temp)){
                return new int[] {i, map.get(temp)};
            }
            map.put(nums[i], i);
        }
        return null;
    }
}
```

### [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

```java
class Solution {
    public int maxArea(int[] height) {
        int i = 0;
        int j = height.length - 1;
        int maxWater = 0;
        while(i < j){
            maxWater = Math.max(maxWater,Math.min(height[i],height[j]) * (j - i));
            if(height[i] > height[j]){
                --j;
            }else{
                ++i;
            }
        }
        return maxWater;
    }
}
```

### [695. 岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/)

给定一个包含了一些 0 和 1 的非空二维数组 grid 。

一个 岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在水平或者竖直方向上相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。

找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 0 。)

```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        if(grid.length == 0 && grid[0].length == 0){
            return 0;
        }
        int res = 0;
        for(int i = 0;i < grid.length; i++){
            for(int j = 0;j < grid[0].length ;j++){
                if(grid[i][j] == 1){
                    int a = getArea(grid,i,j);
                    res = Math.max(res,a);
                }
            }
        }
        return res;
    }
    public int getArea(int[][] grid,int x ,int y){
        if(!(0 <= x && x < grid.length && 0 <= y && y < grid[0].length)){
            return 0;
        }
        if(grid[x][y] != 1){
            return 0;
        }
        grid[x][y] = 2;
        return 1 + getArea(grid,x-1,y)+getArea(grid,x+1,y)+getArea(grid,x,y-1)+getArea(grid,x,y+1);
    }
}
```

### [867. 转置矩阵](https://leetcode-cn.com/problems/transpose-matrix/)

给定一个矩阵 `A`， 返回 `A` 的转置矩阵。

矩阵的转置是指将矩阵的主对角线翻转，交换矩阵的行索引与列索引。

```java
class Solution {
    public int[][] transpose(int[][] A) {
        int r = A.length;
        int c = A[0].length;
        int[][] B = new int[c][r];
        for(int i = 0; i < r ;i++ ){
            for(int j = 0;j < c;j++){
                B[j][i] = A[i][j];
            }
        }
        return B;
    }
}
```

### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。  

 ```java
class Solution {
    public void moveZeroes(int[] nums) {
        for(int i = 0;i < nums.length-1; i++){
            for(int j = i+1;j <nums.length; j++){
                if(nums[i] == 0){
                  int temp = nums[i];
                  nums[i] = nums[j];
                  nums[j]  = temp; 
                }
            }    
        }
    }
}
-------------------------------------------------
class Solution {
    public void moveZeroes(int[] nums) {
      int j = 0;
      for(int i = 0;i < nums.length;i++){
           if(nums[i] != 0){
            nums[j] = nums[i];
            j++;
        }
      }
      for(int i = j; i < nums.length;i++){
          nums[i] = 0;
      }
    }
}
 ```

### [287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

给定一个包含 n + 1 个整数的数组 nums，其数字都在 1 到 n 之间（包括 1 和 n），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

```java
class Solution {
    public int findDuplicate(int[] nums) {
        for(int i=0;i<nums.length-1;i++){
            for(int j=i+1;j<nums.length;j++){
                if(nums[i] == nums[j]){
                    return nums[i];
                }
            }
        }
        return 0;
    }
}
--------------------------------------------------
class Solution {
    public int findDuplicate(int[] nums) {
        Map<Integer,Integer> map = new HashMap<>();
        for(int i = 0;i<nums.length ;i++){
            if(map.containsKey(nums[i])){
                return nums[i];
            }
            map.put(nums[i],i);
        }
        return 0;
    }
}
```

### [485. 最大连续1的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)

给定一个二进制数组， 计算其中最大连续1的个数。 

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int count = 0;
        int Maxcount = 0;
        for(int i = 0;i<nums.length;i++){
            if(nums[i] == 1){
                count+=1;
            }else{
                Maxcount = Math.max(Maxcount,count);
                count = 0;
            }
        }
        return Math.max(Maxcount,count);
    }
}
```

### [905. 按奇偶排序数组](https://leetcode-cn.com/problems/sort-array-by-parity/)

给定一个非负整数数组 `A`，返回一个数组，在该数组中， `A` 的所有偶数元素之后跟着所有奇数元素。

你可以返回满足此条件的任何数组作为答案。

```java
class Solution {
    public int[] sortArrayByParity(int[] A) {
       int[] count = new int[A.length];
       int t = 0;
       for(int i = 0;i < A.length ;i++){
           if(A[i] % 2 == 0){
            count[t++] = A[i];
           }
       }
       for(int j = 0;j < A.length;j++){
          if(A[j] % 2 != 0){
            count[t++] = A[j];
           }
       }
        return count;
    }
}
```

### [922. 按奇偶排序数组 II](https://leetcode-cn.com/problems/sort-array-by-parity-ii/)

给定一个非负整数数组 A， A 中一半整数是奇数，一半整数是偶数。

对数组进行排序，以便当 A[i] 为奇数时，i 也是奇数；当 A[i] 为偶数时， i 也是偶数。

```java
class Solution {
    public int[] sortArrayByParityII(int[] A) {
        int[] arr = new int[A.length];
        int t = 0;
        for(int i = 0;i < A.length ;i++){
            if(A[i] % 2 == 0){          
                arr[t] = A[i];
                t += 2;
            }
        }
        int n = 1;
        for(int i = 0;i < A.length ;i++){
            if(A[i] % 2 == 1){          
                arr[n] = A[i];
                n += 2;
            }
        }
        return arr;
    }
}
-------------------------------------------------
class Solution {
    public int[] sortArrayByParityII(int[] A) {
        int j = 1;
        for(int i = 0;i < A.length ;i+=2){
            if(A[i] % 2 == 1){
                while(A[j] % 2 == 1){
                    j += 2;
                }
                int temp = A[i];
                A[i] = A[j];
                A[j] = temp;
            }
        }
        return A;
    }
}
```

### [766. 托普利茨矩阵](https://leetcode-cn.com/problems/toeplitz-matrix/)

如果一个矩阵的每一方向由左上到右下的对角线上具有相同元素，那么这个矩阵是托普利茨矩阵。

给定一个 M x N 的矩阵，当且仅当它是托普利茨矩阵时返回 True。

```java
class Solution {
    public boolean isToeplitzMatrix(int[][] matrix) {
        for(int i = 0;i < matrix.length; i++){
            for(int j = 0;j < matrix[0].length; j++){
                if(i > 0 && j > 0 && matrix[i-1][j-1] != matrix[i][j]){
                    return false;
                }
            }
        }
        return true;
    }
}
```

### [832. 翻转图像](https://leetcode-cn.com/problems/flipping-an-image/)

给定一个二进制矩阵 A，我们想先水平翻转图像，然后反转图像并返回结果。

水平翻转图片就是将图片的每一行都进行翻转，即逆序。例如，水平翻转 [1, 1, 0] 的结果是 [0, 1, 1]。

反转图片的意思是图片中的 0 全部被 1 替换， 1 全部被 0 替换。例如，反转 [0, 1, 1] 的结果是 [1, 0, 0]。

```java
class Solution {
    public int[][] flipAndInvertImage(int[][] A) {
        int x = A.length;
        int y = A[0].length;
        int[][] B = new int[x][y];
        for(int i = 0;i < x;i++){
            for(int j = 0;j < y;j++){
                B[i][y-j-1] = 1-A[i][j]; 
            }
        }
        return B;
    }
}
```

