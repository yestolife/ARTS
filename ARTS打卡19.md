Algorithm做算法题，Review点评英文文章，Tip总结技术技巧，Share做技术分享。每周打卡一次，这就是ARTS打卡。

# 1. 做算法题

#### [LeetCode78. 子集](https://leetcode-cn.com/problems/subsets/)

**题目描述：**

> 给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。说明：解集不能包含重复的子集。

**示例:**

```
输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```



**解题思路：**把所有的子集列出来过程中，有些规律可以遵循。以`nums = [1,2,3]`为例，二进制位为1处取出数字合成子集，从000到111即可穷举所有的子集。子集数量为2^N，子集存储空间最少为0，最大为N，所以算法空间复杂度为O(N*2^N)。每个子集都要从二进制位为1的位置取数出来，时间复杂度为O(N*2^N)。

```
1 2 3位
0 0 0 -> [     ]
0 0 1 -> [    3]
0 1 0 -> [  2  ]   
0 1 1 -> [  2 3]  
1 0 0 -> [1    ]
1 0 1 -> [1   3] 
1 1 0 -> [1 2  ]
1 1 1 -> [1 2 3]
```

**解题代码：**

```
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        l_num = len(nums)
        subsets_L = []
        for i in range(2**l_num, 2**(l_num+1)):
            bitmask = bin(i)[-l_num:]
            subsets_L += [[nums[j] for j in range(l_num) if bitmask[j] == '1']]
        return subsets_L
```

# 2. 点评英文文章

阅读[《Concise Guide to Databases》](https://link.springer.com/book/10.1007%2F978-1-4471-5601-7)第一章，主要讲数据作为组织重要资产的发展历史。大组织（如公司、教会、政府）的兴起，不仅把资源聚集起来，也把数据汇集在一起，需要大规模存储和深度分析处理，给组织带来价值。各行业的发展历史也是数据的发展历史，不同阶段对数据的使用有不同的需求，也有不同的技术满足这些需求。

# 3. 技术技巧

[此文](https://medium.com/applied-data-science/the-google-vs-trick-618c8fd5359f)介绍了一个搜索引擎的技巧，在搜索某个知识点时，在关键词后面加上`vs`会自动带出相关的信息。例如搜索`database vs`，可以找到相关的概念。

![搜索database vs](https://github.com/yestolife/figurebed/blob/master/img/vs.PNG?raw=true)

# 4. 技术分享

这里分享一本书[《微习惯》](https://book.douban.com/subject/26877306/)，书大意是设置一些简单到不可能失败的习惯，比如每天做一个俯卧撑。微小的习惯几乎不会消耗什么精力，但能积累出良好的生活、学习、工作习惯。每天看一页书，几乎没什么困难，可是一旦看起来了，很可能就放不下来。原本以为根本不可能读完的书，也许花几个月就读完了。其实每天做一个俯卧撑和每天做一百个俯卧撑的开头都是一样的，只不过一想到要做一百个，很有可能就放弃了。