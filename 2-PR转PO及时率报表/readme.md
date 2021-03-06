# 需求: 
    PR 转 PO 及时率报表
# 目的: 
    每日抓取PR 转 PO 的 及时情况，月底，按执行采购员输出及时率报表

# 需求思路:
    条件是一个时间区间，抓出时间区间范围内的所有PR，抓出PR转的PO，一个PR可能会对应多个PO的，要判定PO的采购数量 >= PR的请购数量，

    PR 转 PO 的要具体到 PRLine 中去查找


## 什么样的PR转PO算及时呢?
    就是数量满足了，同时采购单的制单时间-PR的审核时间<24小时

## sql的思路应该是：
    你首先明确你需要取哪些字段，然后筛选条件是什么，最后把你得到的字段进行处理（求和，求平均啊，分组啊）得到你要的结果
    要按照执行采购员 输出 及时率报表 所以需要找出 料品报表中对应的 执行采购字段 之前没有加入 （执行采购 对应 料品下的 可扩展字段 23  DescFlexField_PrivateDescSeg23）

## 变更说明
    2018/12/10 写出存储过程，可以查找出对应单号的及时与否， 但还未添加 执行采购人员 以及料号进去。

    2018/12/11 可以再SQL SERVER 中算出及时率，但是还不清楚如何再 UBF 中进行对应的分组

    2018/12/17 更新存储过程为 P_PR_PO_TimelinessRate0.sql 文件所示(LJ) 并利用UBF 制作报表导出文件 见 UBF/P_PR_PO_TimelinessRate0 文件夹
    参考 Cust_SearchSOToMOATRate 加入 及时率

    2018/12/19 更新存储过程为 P_PR_PO_TimelinessRate2.sql 主要加入了MARK 作为UBF中计算及时率的 计数字段  完成UBF 报表页面 如UBF/prpo 文件下备份文件所示  


- 及时率的计算，UBF 中加入嵌入的代码，并对"及时"进行计数 具体如下图所示
![blockchain](https://raw.githubusercontent.com/mt-yu/material/master/screenshot/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20181219173112.png "嵌入代码")
![blockchain](https://raw.githubusercontent.com/mt-yu/material/master/screenshot/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20181219173204.png "设置及时率到计算公式")