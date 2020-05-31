Algorithm做算法题，Review点评英文文章，Tip总结技术技巧，Share做技术分享。每周打卡一次，这就是ARTS打卡。

# 1. 做算法题

LeetCode第2题，[两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

**题目**：给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例**：

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807

```

**解题思路**：题目用链表存储数据进行加法，可以避免int整型数据相加溢出的风险。由于是按照整数的逆序存储数据，直接将对应位置相加，得到相应位置的数值。其中需要考虑，进位和两个数量级不同的处理。

**解题代码**：

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        l3 = ListNode((l1.val + l2.val) % 10) 
        l3_tmp = l3
        tmp = (l1.val + l2.val) // 10
        l1_tmp = l1.next
        l2_tmp = l2.next
        while l1_tmp is not None or l2_tmp is not None or tmp == 1:  
            if l1_tmp is None:
                l1_tmp =ListNode(0)
            if l2_tmp is None:
                l2_tmp =ListNode(0)
            l3_tmp.next = ListNode((l1_tmp.val + l2_tmp.val + tmp) % 10)
            tmp = (l1_tmp.val + l2_tmp.val + tmp) // 10
            l1_tmp = l1_tmp.next
            l2_tmp = l2_tmp.next
            l3_tmp = l3_tmp.next
        return l3
```



# 2. 点评英文文章

英文文章[sort of handy](https://leancrew.com/all-this/2020/05/sort-of-handy/)介绍了查看linux文件占用大小排序情况的小技巧，`du -hd 1 . | sort -hr`。文章首先介绍命令`df`，`du`的用法，再尝试各种排序的方法，循循善诱得到最后的方法。如果直接告诉结论，用完就忘了来龙去脉。这种介绍技术技巧的方式值得学习。

# 3. 技术技巧

查看计算机方面英文资料更容易获得有价值的信息，stack overflow上的问答一定比知乎至少新鲜1年。除了大量阅读英文资料，还有什么方法能提高英语水平呢？看英文电影吗！那你能保证不看中文字幕？还真有看电影学英文的方法，[voscreen](https://voscreen.com/) APP截取电影中10几秒的视频，没有字幕，播放完后立刻选择与台词意思相近的句子。像刷抖音一样学英语。

# 4. 技术分享

使用Typora编写markdown文章，一般保留在电脑本地。有时候在外面查看自己写的笔记和文章不方便。[此文](https://juejin.im/post/5e6ae645e51d4527235b7658)介绍了Typora+Git实现本地编辑的markdown文件上传github，本地git执行`git add ARTS打卡13.md`，`git commit -m "ARTS打卡13"`，`git push -u origin master`即可上传编写的笔记，迅速将笔记变为一个博客文章。



