Algorithm做算法题，Review点评英文文章，Tip总结技术技巧，Share做技术分享。每周打卡一次，这就是ARTS打卡。

# 1. 做算法题

#### [LeetCode 448. 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

**题目**：

> 给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。
>
> 找到所有在 [1, n] 范围之间没有出现在数组中的数字。
>
> 您能在不使用额外空间且时间复杂度为O(n)的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。
>

**示例**:

> ```
> 输入:
> [4,3,2,7,8,2,3,1]
> 输出:
> [5,6]
> ```

**解题思路**：由于题目限制了空间复杂度为O(1)，时间复杂度为O(n)，不可能通过数组转换为集合再比较缺少的元素（增加集合存储空间），也不可能双重循环遍历数组查找缺少的元素（时间复杂度O(n^2)）。怎么标记一个数字在数组中出现了，而不增加额外空间呢？使用数字的负号。遍历一遍数组，如果某个数字出现了，就将相应位置的数字置为负数，遍历后还是正数的位置序号加1即代表消失的数字。

**解题代码**：

```
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        nums_dis = []
        for num in nums:
            if nums[abs(num)-1] > 0:
                nums[abs(num)-1] = nums[abs(num)-1]*(-1)
        for i, num in enumerate(nums):
            if num > 0:
                nums_dis.append(i+1)
        return nums_dis      
```



# 2. 点评英文文章

文章[Basic tips learning about tiktok](https://hackernoon.com/basic-tips-learning-about-tiktok-0f4m3ucf)，介绍并分析了字节跳动出品的国际版抖音-Tiktok。Tiktok可以说是国内互联网公司打入国际市场的杀手级应用，拥有8亿全球用户。Tiktok主要抓住青少年群体，在全球155个国家75种语言中使用，使用量最大的是印度人。原本想下载Tiktok看一看世界人民怎么玩“抖音”的，可惜不对大陆用户开放。

# 3. 技术技巧

上一篇文章介绍了logstash监控一台服务器运行信息，如果要同时监控很多台服务器怎么办？可以使用[filebeat](https://www.elastic.co/cn/beats/filebeat)与logstash配合，部署在各服务器上，将日志信息发送到logstash，由logstash汇总处理。

# 4. 技术分享

搭建ntp服务器[参考](https://www.cnblogs.com/Sungeek/p/10197345.html)，yum一下就安装好了，再配置/etc/ntp.conf文件即可。使用命令`sudo ntpd`启动ntp，使用命令`service ntpd status`查看ntp运行状态。用`nptp -p`查看时钟同步情况，如果ntp服务器自动退出了，很可能是同步时间相差太大，解决[办法](https://my.oschina.net/erichd/blog/620864)。得到以下同步信息（含义[参考](https://blog.csdn.net/kingzdd/article/details/80106081)）：

```
[andy@localhost ~]$ ntpq -p
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
 stratum2-1.ntp. 89.109.251.24    2 u    -   64    1  494.679  -119.12   0.000
 time.cloudflare 10.4.0.166       3 u    -   64    1  254.013   -7.624   0.000
 ntp1.flashdance 192.36.143.152   2 u    -   64    1  494.920  -44.923   0.000
 ntp1.ams1.nl.le .STEP.          16 u    -   64    0    0.000    0.000   0.000
[andy@localhost ~]$ date
Thu Jun 18 06:51:02 CST 2020

```

由于ntp搭建在vmware的linux虚拟机中，可以使用windows宿主机连接linux虚拟机进行时间同步测试。首先要打开linux防火墙的123udp端口，[参考](https://blog.csdn.net/gycool21/article/details/51746174)。然后在windows中设置时间服务器为linux虚拟机ip。



