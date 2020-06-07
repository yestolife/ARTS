Algorithm做算法题，Review点评英文文章，Tip总结技术技巧，Share做技术分享。每周打卡一次，这就是ARTS打卡。

# 1. 做算法题

#### [Leetcode3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

**题目描述**：

> 给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

**示例** :

> 输入: "abcabcbb"
> 输出: 3 
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
>
> 输入: "bbbbb"
> 输出: 1
> 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
>
> 输入: "pwwkew"
> 输出: 3
> 解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。请注意，你的答案必须是 **子串** 的长度，"pwke" 是一个子序列，不是子串。

**解题思路**：为了找到最长的无重复字串，最直接的想法就是以每个字符为起点遍历下去，查看是否出现重复字符。双重循环，时间复杂度O(n^2)，未增加额外空间，空间复杂度O(1)。实现为下面代码1，这时间复杂度太高了，在leetcode中执行超时。有能够压缩时间的地方吗？利用set取代list，由于set底层是用hash表存储，查询速度比list快，不过增加了存储set的空间。实现为下面代码2，时间复杂度为O(n)，空间复杂度为O(len_max)。

**解题代码1**：

```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        n = len(s)
        i, j = 0, 1
        len_max = 0
        while j < n:
            if s[j] not in s[i:j]:
                len_max = max(len_max, j-i+1)
                j += 1
            else:
                i += 1
        return len_max
            

```

**解题代码2**：

```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        n = len(s)
        i, j = 0, 0
        len_max = 0
        s_set = set()
        while j < n:
            if s[j] not in s_set:
                s_set.add(s[j])
                len_max = max(len_max, j-i+1)
                j += 1
            else:
                s_set.remove(s[i])
                i += 1
        return len_max
            
```

# 2.点评英文文章

文章[Nvidia’s AI recreates Pac-Man from scratch just by watching it being played](https://www.theverge.com/2020/5/22/21266251/nvidia-ai-gamegan-recreate-pac-man-virutal-environment)，介绍了英伟达最近的AI成果，通过AI观看吃豆人游戏，AI自己做出同样的游戏。也就是说AI能自己编程，未来这要抢走多少程序员的饭碗。

# 3. 技术技巧

使用markdown写笔记中经常需要插入图片，如果把图片存储在本地，在其他平台就不能正常展示。图床是一类存储图片的服务器。把图片存储在图床上，通过url访问图片资源。这就可以解决markdown图片存储和显示的问题。可利用[PicGo](https://github.com/Molunerfinn/PicGo)+Github构建图床，参考[此文](https://juejin.im/entry/5c4ec5aaf265da614420689f)。如果github图片显示不了，参考[此文](https://blog.csdn.net/qq_38232598/article/details/91346392)。解决思路是编辑hosts文件，以绕过DNS污染。

# 4. 技术分享

服务器运维过程中总会遇到磁盘占用过高，而要删除一些无关文件的情况，写了一些自动清理磁盘的shell脚本放在[github](https://github.com/yestolife/cleardata)上。