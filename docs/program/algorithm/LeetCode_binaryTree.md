# 二叉树

### [144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

```java
/*前序递归方法*/
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        helper(root , list);
        return list;
    }   
    public void helper(TreeNode root , List list){
        if(root == null){
            return;
        }
        list.add(root.val);
        helper(root.left , list);
        helper(root.right , list);
    }
}
```

```java
/*前序迭代方法*/
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if(root == null) return list;
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.empty()){
            TreeNode node = stack.pop();
            list.add(node.val);
            if(node.right != null){
                stack.push(node.right);
            }
            if(node.left != null){
                stack.push(node.left);
            }
        }
        return list;
    }
}
```

### [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

```java
/*中序递归方法*/
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        helper(root , list);
        return list;
    }   
    public void helper(TreeNode root , List list){
        if(root == null){
            return;
        }
        helper(root.left , list);
        list.add(root.val);
        helper(root.right , list);
    }
}
```

```java
/*中序迭代*/
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        
        TreeNode head = root;
        while(!stack.empty() || head!=null){
            while(head != null){
                stack.push(head);
                head = head.left;
            }
            head = stack.pop();
            list.add(head.val);
            head = head.right;
        } 
        return list;
    }
}
```

### [145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

```java
/*后序递归方法*/
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        helper(root , list);
        return list;
    }   
    public void helper(TreeNode root , List list){
        if(root == null){
            return;
        }
        helper(root.left , list);
        helper(root.right , list);
        list.add(root.val);
    }
}
```

```java
/*后序的迭代*/
public List<Integer> postorderTraversal(TreeNode root) {//非递归写法
    List<Integer> res = new ArrayList<Integer>();
    if(root == null)
        return res;
    Stack<TreeNode> stack = new Stack<TreeNode>();
    TreeNode pre = null;
    stack.push(root);
    while(!stack.isEmpty()){
        TreeNode curr = stack.peek();            
        if((curr.left == null && curr.right == null) ||
           (pre != null && (pre == curr.left || pre == curr.right))){ 
            //如果当前结点左右子节点为空或上一个访问的结点为当前结点的子节点时，当前结点出栈
            res.add(curr.val);
            pre = curr;
            stack.pop();
        }else{
            if(curr.right != null) stack.push(curr.right); //先将右结点压栈
            if(curr.left != null) stack.push(curr.left);   //再将左结点入栈
        }            
    }
    return res;        
}
```

### [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

```java
/*层序遍历*/
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> list = new ArrayList<>();
        if(root == null) return list;

        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.add(root);
        while(!queue.isEmpty()){
            int size = queue.size();
            List<Integer> li = new ArrayList<>();
            for(int i = 0 ;i < size; i++){
                TreeNode cur = queue.poll();
                li.add(cur.val);
                if(cur.left != null) queue.add(cur.left);
                if(cur.right != null) queue.add(cur.right);
            }
            list.add(li);
        }
        return list;
    }
}
```

递归写法

![](D:\TyporaFile\图片\层序遍历.gif)

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> list = new ArrayList<>();
        if(root == null){
            return list;
        }
        dfs(list, root, 1);
        return list;
    }

    public void dfs(List<List<Integer>> list, TreeNode root, int leval){
       
        if(list.size() < leval){
            list.add(new ArrayList<Integer>());
        }
        list.get(leval - 1).add(root.val);
        if(root.left != null){
            dfs(list, root.left, leval + 1);
        }
        if(root.right != null){
            dfs(list, root.right, leval + 1);
        }
    }
}
```

### **[面试题 04.02. ](https://leetcode-cn.com/problems/minimum-height-tree-lcci/)最小高度树**

**给定一个有序整数数组，元素各不相同且按升序排列，编写一个算法，创建一棵高度最小的二叉搜索树。**

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return buildTree(nums, 0 ,nums.length);
    }

    public TreeNode buildTree(int[] nums, int l, int r){
        if(l == r) return null;

        int mid = l + (r - l) / 2;

        TreeNode node = new TreeNode(nums[mid]);

        node.left = buildTree(nums, l ,mid);
        node.right = buildTree(nums, mid + 1, r);
        return node;
    }
}
```

### [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q) return root;

        TreeNode l = lowestCommonAncestor(root.left, p, q);
        TreeNode r = lowestCommonAncestor(root.right, p, q);

        if(l == null) return r;
        if(r == null) return l;
        return root;
    }
}
```

### [面试题 04.04. 检查平衡性](https://leetcode-cn.com/problems/check-balance-lcci/)

 实现一个函数，检查二叉树是否平衡。在这个问题中，平衡树的定义如下：任意一个节点，其两棵子树的高度差不超过 1。 

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root == null) return true;

        int left  = getDept(root.left, 0);
        int right = getDept(root.right, 0);
        int h = Math.abs(left - right);
        if(h > 1){
            return false;
        }
        return isBalanced(root.left) && isBalanced(root.right);
        
    }

    public int getDept(TreeNode root, int dept){
        if(root == null) return dept;
        int left = getDept(root.left, dept + 1);
        int right = getDept(root.right , dept + 1);

        return Math.max(left, right);
    }
}
```

### [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)

给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。 

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null){
            return false;
        }
        
        if(root.left == null && root.right == null && sum == root.val){
            return true;
        }

        return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right,sum - root.val);
    }
}
```

### [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

给定一个二叉树，检查它是否是镜像对称的

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return ismirror(root,root);
    }
    public static boolean ismirror(TreeNode t1,TreeNode t2){
        if(t1 == null && t2 == null){
            return true;
        }
        if(t1 == null || t2 == null){
            return false;
        }
        return (t1.val == t2.val) 
        && ismirror(t1.left ,t2.right) 
        && ismirror(t1.right ,t2.left);
    }
}
```

### [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

 给定一个二叉树，判断其是否是一个有效的二叉搜索树。 

```java
class Solution {
    
    ArrayList<Integer> list = new ArrayList<>();
    
    public boolean isValidBST(TreeNode root) {
        if(root == null){
            return true;
        }
        InOrdertravsersal(root);
        for(int i = 1;i < list.size();i++){
            if(list.get(i) <= list.get(i - 1)){
                return false;
            }
        }
        return true;
    }

       public void InOrdertravsersal(TreeNode root){
        if(root == null){
            return;
        }
        InOrdertravsersal(root.left);
        list.add(root.val);
        InOrdertravsersal(root.right);
    }
}
```

### [129. 求根到叶子节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)

给定一个二叉树，它的每个结点都存放一个 0-9 的数字，每条从根到叶子节点的路径都代表一个数字。

```java
class Solution {
    public int sumNumbers(TreeNode root) {
        return helper(root , 0);
    }
    public int helper(TreeNode root ,int sum){
        if(root == null){
            return 0;
        }
        if(root.left == null && root.right == null){
            return sum * 10 + root.val; 
        }
        
        return helper(root.left , sum * 10 + root.val) + helper(root.right , sum * 10 + root.val);
  
    }
}
```

### [105. 从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

根据一棵树的前序遍历与中序遍历构造二叉树。 

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        if(preorder == null || preorder.length == 0){
            return null;
        }

        TreeNode root = new TreeNode(preorder[0]);

        int index = findIndex(preorder, inorder);
        root.left = buildTree(Arrays.copyOfRange(preorder, 1 ,index + 1),
                              Arrays.copyOfRange(inorder, 0 ,index));

        root.right = buildTree(Arrays.copyOfRange(preorder, index + 1,preorder.length),
                               Arrays.copyOfRange(inorder, index + 1, inorder.length));

        return root;
    }

    public int findIndex(int[] preorder, int[] inorder){
        for(int i = 0;i < inorder.length ; i++){
            if(inorder[i] == preorder[0]){
                return i;
            }
        }
        return 0;
    }
}
```

### 257.[二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/)

给定一个二叉树，返回所有从根节点到叶子节点的路径。 

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> list = new ArrayList<>();
        allPath(root,"",list);
        return list;
    }
    public void allPath(TreeNode root,String s, List<String> list){
        if(root != null){
            s = s + root.val;
            if(root.left == null && root.right == null){
                list.add(s);
            }else{
                s = s + "->";
                allPath(root.left, s, list);
                allPath(root.right, s, list);
            }
        }
    }
}
```

### [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

```java
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> list = new ArrayList<>();
        List<Integer> li = new ArrayList<>();
        sum(root,sum,li,list);
        return list;
    }

    public void sum(TreeNode root, int sum,List<Integer> list, List<List<Integer>> result) {
         if(root != null){
             List<Integer> subList = new ArrayList<>(list);
             subList.add(root.val);
             if(root.left == null && root.right == null && sum == root.val){
                result.add(subList);
                return;
             }
            sum(root.left,sum - root.val, subList, result);
            sum(root.right,sum - root.val, subList, result);
         }
    }
}
```



















































