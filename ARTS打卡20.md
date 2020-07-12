Algorithm做算法题，Review点评英文文章，Tip总结技术技巧，Share做技术分享。每周打卡一次，这就是ARTS打卡。

# 1. 做算法题

#### [LeetCode 234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

**题目描述**：请判断一个链表是否为回文链表。你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

**示例**：

>示例 1:输入: 1->2
>输出: false
>示例 2:输入: 1->2->2->1
>输出: true

**解题思路**：如果数组L存储字符串的回文检查，很简单`if L == L[::-1]`就能判断是否回文。而题目中使用链表存储，最简单的思路是把链表转换为数组（python中是列表），再判断是否回文。这中方法要遍历整个链表，时间复杂度O(N)，由于增加列表存储，空间复杂度O(N)。具体实现，参考代码1。题目还有进阶部分，要将空间复杂度降为O(1)，也就是不能使用额外的存储空间，只能在链表上做文章。使用快慢指针，慢指针每次前进1步，快指针每次前进2步，一直到快指针知道链表尾部，慢指针正好指向链表中间位置。慢指针继续向后推移逆转后半部分链表。最后比较链表的前半部分和后半部分判断是否回文。具体实现，参考代码2。

**解题代码**1：

```
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        L = []
        while head is not None:
            L += [head.val]
            head = head.next
        return L == L[::-1]
```

**解题代码**2：

```
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        slow,fast,prev = head,head,None
        while fast is not None:
            slow = slow.next
            fast = fast.next.next if fast.next is not None else fast.next
        while slow is not None:
            slow.next, slow, prev= prev, slow.next, slow
        while head and prev:
            if head.val != prev.val:
                return False
            head = head.next
            prev = prev.next
        return True
```



# 2. 点评英文文章

阅读[《Concise Guide to Databases》](https://link.springer.com/book/10.1007%2F978-1-4471-5601-7)第二章，介绍了数据库发展的历程，数据库随着业务需求和技术更新的发展而发展出了关系型数据库、面向对象数据库、NoSQL数据库、数据仓库、云上数据库服务、空间数据库、时序数据库等等。文中详细描述了这些数据库的发展脉络，让你对数据库有更立体的认识。

# 3. 技术技巧

写正则表达式之后应该测试一下，避免出现逻辑漏洞，介绍一个在线正则匹配[工具](https://tool.lu/regex/)。这个工具不仅可以测试正则表达式，还可以生成邮箱、ip、手机号等正则表达式。

# 4. 技术分享

[参考官网说明](https://vercel.com/guides/deploying-vuepress-to-vercel)使用[Vuepress](https://segmentfault.com/a/1190000016333850)+[Vercel](https://vercel.com/)实现免费托管博客。注意先在本地跑通Vuepress，能正确访问页面。在配置Vercel时注意，Vercel访问github仓库权限要打开，配置编译生成时用`npm init -y && npm i -D vuepress && npm run build`，以在云端部署vuepress环境在跑页面出来。选择一文件夹public为输出，缺少的话会报错。代码参考我的[github](https://github.com/yestolife/blog/)。