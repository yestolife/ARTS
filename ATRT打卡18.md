Algorithm做算法题，Review点评英文文章，Tip总结技术技巧，Share做技术分享。每周打卡一次，这就是ARTS打卡。

# 1. 做算法题

#### [LeetCode 20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

**题目描述**：

>给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
>
>有效字符串需满足：左括号必须用相同类型的右括号闭合。左括号必须以正确的顺序闭合。
>
>注意空字符串可被认为是有效字符串。

**示例**:

>示例 1:
>
>输入: "()"
>输出: true
>示例 2:
>
>输入: "()[]{}"
>输出: true
>示例 3:
>
>输入: "(]"
>输出: false
>示例 4:
>
>输入: "([)]"
>输出: false
>示例 5:
>
>输入: "{[]}"
>输出: true

**解题思路**：判断匹配的括号，应该是编译器经常做的事情，用栈的数据结构最合适。一句话解释就是，若遇到左括号入栈，遇到右括号时将对应栈顶左括号出栈，遍历完所有括号后 `stack` 仍然为空，则为有效的括号。

**解题代码**：

```
class Solution:
    def isValid(self, s: str) -> bool:
        dic = {'{': '}',  '[': ']', '(': ')', '?': '?'}
        stack = []
        for c in s:
            if c in dic: stack.append(c)
            elif len(stack) == 0 or dic[stack.pop()] != c: return False 
        return len(stack) == 0
```



# 2. 点评英文文章

最近读到一本英文书[《Concise Guide to Databases》](https://link.springer.com/book/10.1007%2F978-1-4471-5601-7)，是来自Springer出版的，近期Springer免费了很多英文书籍，有人整理了一下[书籍目录]([https://hnarayanan.github.io/springer-books/#Computer%20Science](https://hnarayanan.github.io/springer-books/#Computer Science))。回到这本讲数据库基础的书，开头就介绍一下本书的目的是什么，结构是什么，怎么阅读，适合什么人群。看得出作者读的书肯定很多，知道读者关心的是什么。

# 3. 技术技巧

logstash使用中主要是配置参数，一个参数配置不对，可能会让你调试一上午。例如beats作为输入，配置时写成

```
input {
  beats {
    host => "127.0.0.1"
    port => 5044
  }
}
```

看上去没问题，监听本地的5044端口。但这么写死了host ip，其他主机是找不到logstash输入地址的，只能在本机使用beats作为输入了。像这种配置性的工具严格按照官网的介绍配一般不容易出错。

# 4. 技术分享

参考[此文](https://blog.itswincer.com/posts/1352252a/)实现反向代理“外网”网站，代码见[这里](https://github.com/yestolife/mirror)。使用[vercel](https://vercel.com/)的云托管服务，可以免费托管页面，而且支持github导入project，一键实现在线生成、部署、访问。注意选择分支now_sh。

