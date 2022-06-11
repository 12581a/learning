# 二分查找

### [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。 

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int right = nums.length;
        int left = 0;
        while(left < right){
            int mid = (right + left) >>> 1;
            if(nums[mid] == target){
                return mid;
            }else if(nums[mid] > target){
                right = mid;
            }else if(nums[mid] < target){
                left = mid + 1;
            }
        }
        return left;
    }
}
```

### [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)

实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

```java
class Solution {
    public int mySqrt(int x) {
        int left = 0;
        int right = x;
        while(left < right){
            int mid = (left + right + 1) >>> 1;
            if(x / mid == mid){
                return mid;
            }else if(x /mid > mid){
                left = mid + 1;
            }else if(x / mid < mid){
                right = mid - 1;
            }
        }
        return ;
    }
}
```

### [面试题 08.03. 魔术索引](https://leetcode-cn.com/problems/magic-index-lcci/)

魔术索引。 在数组A[0...n-1]中，有所谓的魔术索引，满足条件A[i] = i。给定一个有序整数数组，编写一种方法找出魔术索引，若有的话，在数组A中找出一个魔术索引，如果没有，则返回-1。若有多个魔术索引，返回索引值最小的一个

```java
class Solution {
    int res = -1;
    public int findMagicIndex(int[] nums) {
       search(nums,0,nums.length - 1);
       return res;
    }

    public void search(int[] nums,int start,int end){
        if(start > end){
            return;
        }
        int mid = start + (end-start) / 2;
        if(nums[mid] == mid){
            res = mid;
            search(nums,start,mid-1);
        }else{
            search(nums,start,mid-1);
            if(-1 == res){
                search(nums,mid+1,end);
            }
        }            
    } 
}
```









