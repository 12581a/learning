给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

## [滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        if(n < 0 || k < 0) return nums;
        Deque<Integer> dq = new LinkedList<>();
        int res[] = new int[n - k + 1];
        for(int i = 0;i < n;i++){
            //头部出队:移除头部，保证窗口的范围
            if(!dq.isEmpty() && dq.getFirst() < i- k + 1 ){
               dq.removeFirst(); 
            }
            //尾部清理：
            while(!dq.isEmpty() && nums[i] >= nums[dq.getLast()] ){
                dq.removeLast();
            }
            //尾部入队
            dq.addLast(i);
            //返回头部
            if(i >= k - 1){
                res[i - k + 1] = nums[dq.getFirst()];
            }
        }
        return res;  
    }
}
```

