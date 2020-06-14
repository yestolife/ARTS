Algorithm做算法题，Review点评英文文章，Tip总结技术技巧，Share做技术分享。每周打卡一次，这就是ARTS打卡。

# 1. 做算法题

#### [leetcode第11题. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

**题目**:给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。说明：你不能倾斜容器，且 n 的值至少为 2。

 ![示例](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

**示例**：

> 输入：[1,8,6,2,5,4,8,3,7]
> 输出：49

**解题思路**：题目是要求找到柱子（垂直线）形成的容器中最大的那个，容器的容水量等于两个柱子中最短的柱高乘以两柱子的距离。最简单的方法是穷举出所有柱子形成的容器计算容水量，找到最大的。遍历任意两个柱子的时间复杂度O(n^2)，再找出最大的容器时间复杂度O(n)，总共时间复杂度O(n^3)。没有增加额外空间，空间复杂度O(1)。这显然是无法忍受的。

有什么办法能减少遍历次数呢？从容器水量计算来看，有两个变量，两柱最短的高度和两柱的距离。控制一个变量，两柱的距离。这个距离当然越大容器水量就越大，假设初始状态是首尾两个柱子，距离最大，计算出水容量。再改变这个变量，左右柱子进行移动，找到更大的水容量。怎么移动呢？将两柱中较短的那跟移向中间，才可能是的水容量变大。因为移动较长的那跟柱子的话，两柱距离缩小了，即使移动后的柱子更高但还是受限于之前的短柱子，水容量一定是减少的。只有移动较短的柱子才有可能找到更大的容器。这样遍历只需要一遍，时间复杂度O(n)，空间复杂度O(1)。

**解题代码**：

```
class Solution:
    def maxArea(self, height: List[int]) -> int:
        l, r, max_area = 0, len(height)-1, 0
        while l < r:
            max_area = max(max_area, (r - l) * min(height[l], height[r]))
            if height[r] < height[l]:
                r -= 1
            else:
                l += 1
        return max_area
```



# 2.点评英文文章

使用logstash工具配置过程中要参考[官方文档](https://www.elastic.co/guide/en/logstash/current/plugins-codecs-multiline.html)，英文的技术手册看起来虽然慢点，但逻辑还算是比较清楚。在逻辑复杂的地方，给出案例，这无疑是一剂良方。例如下面这段描述：

>The `pattern` should match what you believe to be an indicator that the field is part of a multi-line event.
>
>The `what` must be `previous` or `next` and indicates the relation to the multi-line event.
>
>The `negate` can be `true` or `false` (defaults to `false`). If `true`, a message not matching the pattern will constitute a match of the multiline filter and the `what` will be applied. (vice-versa is also true)

初看上去非常绕，正则、前后、真假？什么东西啊！再看看下面的例子就好理解多了，

> Another example is to merge lines not starting with a date up to the previous line..
>
> ```ruby
>     input {
>       file {
>         path => "/var/log/someapp.log"
>         codec => multiline {
>           # Grok pattern names are valid! :)
>           pattern => "^%{TIMESTAMP_ISO8601} "
>           negate => true
>           what => "previous"
>         }
>       }
>     }
> ```
>
> This says that any line not starting with a timestamp should be merged with the previous line.

# 3. 技术技巧

用vmware跑Linux虚拟机是很常见的操作，那么你是如何把宿主机中的文件拷贝到虚拟机中呢？如果linux是图形界面，直接拖拽即可。但如果是命令行界面怎么办。可以通过优盘挂载到Linux中，实现文件转移，不过有些麻烦。既然虚拟机可以看作是一台独立的计算机，网络是通的就可以用[xftp](https://www.netsarang.com/zh/xftp/)工具传输文件，也可以通过[xshell](https://www.netsarang.com/zh/xshell/)工具连接虚拟机。

# 4. 技术分享 

不管是公司服务器运维还是自搭云主机，经常要登入看看磁盘、CPU、内存和进程运行情况。用logstash+shell实现自动监控并发邮件报告，具体参考我写在github的[文章](https://github.com/yestolife/ARTS/blob/master/logstash%2Bshell实现自动监控并发邮件报告.md)。