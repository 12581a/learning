# 链表

### [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照逆序的方式存储的，并且它们的每个节点只能存储一位数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例：**

> 输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
> 输出：7 -> 0 -> 8
> 原因：342 + 465 = 807

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode root = new ListNode(0);
        ListNode cur = root;
        int carry = 0;
        while(l1 != null || l2!= null){
            int x = (l1 != null) ? l1.val : 0;
            int y = (l2 != null) ? l2.val : 0;
            int sum = x + y + carry;
            carry = sum / 10;
            cur.next = new ListNode(sum % 10);
            cur = cur.next;
            if(l1 != null) l1 = l1.next;
            if(l2 != null) l2 = l2.next;
            if(carry > 0){
                cur.next = new ListNode(carry);
            }
        }
        return root.next;
    }
}
```

### [445. 两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/)

给你两个 非空链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
            Stack<Integer> stack1 = new Stack<>();
            Stack<Integer> stack2 = new Stack<>();
            
            while(l1 != null){
                stack1.push(l1.val);
                l1 = l1.next;
            }

            while(l2 != null){
                stack2.push(l2.val);
                l2 = l2.next;
            }

            int carry = 0;
            ListNode head = null;
            while(!stack1.isEmpty() || !stack2.isEmpty() || carry > 0){
                int sum = carry;
                sum += stack1.isEmpty() ? 0 : stack1.pop();
                sum += stack2.isEmpty() ? 0 : stack2.pop();
                ListNode node = new ListNode(sum % 10);
                node.next = head;
                head = node;
                carry = sum / 10;
            }
            return head;
    }
}
```

### [面试题24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。 

**示例:**

> 输入: 1->2->3->4->5->NULL
> 输出: 5->4->3->2->1->NULL

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head.next == null || head == null){
            return head;
        }
        ListNode pre = null;
        ListNode cur = head;
        ListNode next = null;
        while(cur != null){
            next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}
```

 反转一个单链表 

```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev
}
```

### [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

 给定一个链表，删除链表的倒数第 *n* 个节点，并且返回链表的头结点。 

思路： 采用两个间隔为n的指针，同时向前移动。当快指针的下一个节点为最后一个节点时，要删除的节点就是慢指针的下一个节点。 

 **示例：** 

> 给定一个链表: 1->2->3->4->5, 和 n = 2.
>
> 当删除了倒数第二个节点后，链表变为 1->2->3->5.

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode node = new ListNode(0);
        node.next = head;
        ListNode fast = node;
        ListNode slow = node;
        for(int i = 0 ;i < n; i++){
            fast = fast.next;
        }

        while(fast.next != null){
            slow = slow.next;
            fast = fast.next;
        }
        slow.next = slow.next.next;
        return node.next;
    }
}
```

### [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

 将两个升序链表合并为一个新的升序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

 **示例：** 

> 输入：1->2->4, 1->3->4
> 输出：1->1->2->3->4->4

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dump = new ListNode(0);
        ListNode cur = dump;
        while(l1 != null && l2 != null){
            if(l1.val <= l2.val){
                cur.next = l1;
                l1 = l1.next;
            }else{
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }

        if(l1 == null) cur.next = l2;
        if(l2 == null) cur.next = l1;
        return dump.next;
    }
}
```

### [109. 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)

 给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。 

```java
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if(head == null){
            return null;
        }
        return buildTree(head, null);
    }

    public TreeNode buildTree(ListNode head , ListNode tail){
        
        if(head == tail){
            return null;
        }
        ListNode first = head;
        ListNode slow = head;

        while(first.next != tail && first.next.next != tail ){
            first = first.next.next;
            slow = slow.next;
        }
        TreeNode root = new TreeNode(slow.val);
        
        root.left = buildTree(head, slow);
        root.right = buildTree(slow.next, tail);
        return root;    
    }
}
```

### [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。 

```java
class Solution {
    public ListNode oddEvenList(ListNode head) {

        if(head == null){
            return null;
        }
        ListNode even = head.next, evenHead = even, odd = head;

        while(even != null && even != null){
            odd.next = even.next;
            odd = odd.next;
            even.next = odd.next;
            even = even.next;
        }
        odd.next = evenHead;
        return head;
    }
}
```

### [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。 

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        
        if(head == null){
            return null;
        }

        ListNode cur = head;
        while(cur != null && cur.next != null ){
            if(cur.val == cur.next.val){
               cur.next = cur.next.next;
            }else{
               cur = cur.next;
            }
        }
        return head;
    }
}
```

### [面试题 02.06. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list-lcci/)

编写一个函数，检查输入的链表是否是回文的。 

```java
 class Solution {

    public boolean isPalindrome(ListNode head) {
        // 快慢指针找中点
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        // 反转后半部分
        ListNode pre = null;
        while (slow != null) {
            ListNode next = slow.next;
            slow.next = pre;
            pre = slow;
            slow = next;
        }
        // 前后两段比较是否一致
        ListNode node = head;
        while (pre != null) {
            if (pre.val != node.val) {
                return false;
            }
            pre = pre.next;
            node = node.next;
        }
        return true;
    }
}
```

### [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if(head== null || head.next == null){
            return head;
        }
        ListNode next = head.next;
        head.next = swapPairs(next.next);
        next.next = head;
        return next;
    }
}
```

### [面试题 02.07. 链表相交](https://leetcode-cn.com/problems/intersection-of-two-linked-lists-lcci/)

给定两个（单向）链表，判定它们是否相交并返回交点。请注意相交的定义基于节点的引用，而不是基于节点的值。换句话说，如果一个链表的第k个节点与另一个链表的第j个节点是同一节点（引用完全相同），则这两个链表相交。

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode l1 = headA;
        ListNode l2 = headB;

        while(l1 != l2){
            if(l1 != null){
                l1 = l1.next;
            }else{
                l1 = headB;
            }
            if(l2 != null){
                l2 = l2.next;
            }else{
                l2 = headA;
            }
        }

        if(l1 == null){
            return null;
        }
        return l1;
    }
}
```

### [61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)

给定一个链表，旋转链表，将链表每个节点向右移动 *k* 个位置，其中 *k* 是非负数。 

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if(head == null){
            return null;
        }
        if(head.next == null){
            return head;
        }
       
       ListNode cur = head;
       
       int n;
       for(n = 1; cur.next != null ;n++){
           cur = cur.next;
       }
       cur.next = head;

       ListNode new_tail = head;

       for(int i = 0;i < n - k % n - 1;i++ ){
           new_tail = new_tail.next;
       }

       ListNode new_head = new_tail.next;
       new_tail.next = null;

       return new_head; 
    }
}
```

### [面试题 02.01. 移除重复节点](https://leetcode-cn.com/problems/remove-duplicate-node-lcci/)

编写代码，移除未排序链表中的重复节点。保留最开始出现的节点。 

**示例1:** 

>  输入：[1, 2, 3, 3, 2, 1]
>  输出：[1, 2, 3]

```java
class Solution {
    public ListNode removeDuplicateNodes(ListNode head) {
       if(head == null){
           return null;
       }
       ListNode dummyHead = new ListNode(0);
       dummyHead.next = head;
       ListNode pre = dummyHead;

       while(pre.next != null){
           ListNode cur = pre.next;
           while(cur.next != null){
               if(pre.next.val == cur.next.val){
                   cur.next = cur.next.next;
               }else{
                   cur = cur.next;
               }
           }
           pre = pre.next;
       }
       return dummyHead.next;
    }
}
```

### [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

给定一个链表，判断链表中是否有环。 

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        
        if(head == null || head.next == null){
            return false;
        }

        ListNode fast = head.next;
        ListNode slow = head;

        while(fast != slow){
            if(fast == null || fast.next == null){
                return false;
            }
            slow = slow.next;
            fast = fast.next.next;
        }
        return true;
    }
}
```

### [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;

        while(true){
            if(fast == null || fast.next == null){
                return null;
            }
            slow = slow.next;
            fast = fast.next.next;
            if(slow == fast) break;
        }
        fast = head;
        while(slow != fast){
            fast = fast.next;
            slow = slow.next;
        }
        return fast;
    }
}
```

### [剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

输入两个链表，找出它们的第一个公共节点。 

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode l1 = headA;
        ListNode l2 = headB;

        while(l1 != l2){
            l1 = l1==null ? headB : l1.next;
            l2 = l2==null ? headA : l2.next;
        }
        return l1;
    }
}
```

**将两个升序链表合并为一个新的升序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。**  

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null){
            return l2;
        }
        if(l2 == null){
            return l1;
        }
        if(l1.val < l2.val){
            l1.next = mergeTwoLists(l1.next,l2);
            return l1;
        }else{
            l2.next = mergeTwoLists(l1,l2.next);
            return l2; 
        }
    }
}
```

**定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。** 

```java
class Solution {
    public ListNode reverseList(ListNode head) {
      ListNode pre = null;
      ListNode cur = head;
      while(cur != null){
          ListNode temp = cur.next;
          cur.next = pre;
          pre = cur;
          cur = temp;
        }
         return pre;
        }
}
```

