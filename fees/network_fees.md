# 网络类计费方式

## 计费概述
本文介绍互联网转接带宽的费用组成和计费模式。
## 计费组成
互联网转接带宽产品的费用由端口费用、IP 地址费用和互联网带宽费用组成。

| 计费项       | 计费说明 | 计费类型 | 
| -------------- | ------------- | --------- |
| 端口费用 | 接入UCloud交换机端口，按照一次性计费 | 一次性  |
| IP地址费用 | 分配给用户的公网IP费用，按照个数/月计费 | 月租  |
| 互联网带宽费用 | 公网IP绑定用户资源后，访问互联网时，产生的带宽费 | 月租  |
| 专线带宽费用 | 接入UCloud的物理专线，访问内部资源时，产生的带宽费 | 月租  |




## 端口费用
互联网端口仅接入UCloud侧收取费用，按照个数/月计费。
### 计费模式
端口费用的计费周期为按月计费。计费时长精确到天，不足一个月的按时间占比（即计费天数 / 自然月天数）计费。
### 计费价格
端口费用(仅UCloud侧，包含UCloud侧使用耗材)统一为115元/月/个，建议与UCloud设备对接的耗材客户自备，如需购买UCloud耗材请咨询客户经理。

## IP地址费用
互联网IP是可租用的互联网IP地址，因此UCloud互联网IP按照个数/月计费。
### 计费模式
IP资源费用的计费周期为按月计费。计费时长精确到天，**不足一个月的按时间占比（即计费天数 / 自然月天数）计费**
### 计费公式
IP资源费用 = 互联网IP所在地域的单价 ×互联网IP数量×计费时长

## 互联网带宽费用
根据客户选择的不同计费模式所产生的互联网带宽费用。
### 计费模式
互联网转接的计费模式分为固定带宽和月95峰值带宽，均为按月计费。
- 固定带宽：适用于业务流量峰值在不同时间段比较平稳，且长期使用的场景。  
固定带宽后付费计费模式下，用户可以根据业务需求对固定峰值带宽进行调整（限速），实时生效。每次调整带宽，将实时生成一张后付费账单，账单结算费用为调整前带宽的实际使用情况（旧带宽数值＊实际使用时间），精确到小时。每月1号会自动产生一张开始时间为上次结算完成的后付费账单，账单结算费用为最新带宽值的实际使用情况（新带宽数值＊实际使用时间），精确到小时。
- 月95峰值带宽：适用于业务流量峰值在不同时间段波动较大的场景。  
95峰值带宽后付费计费模式下，用户需要设置保底带宽（最低值为100Mbps），UCloud对峰值带宽的限制为用户设置保底带宽的5倍。计费按自然月结算，在一个自然月内，按外网出口的出入方向分别取每5分钟有效带宽值进行升序排列(若外网出口为多端口，优先分别求和各端口出入带宽值)，然后取数据总条目数的第95%条（如果95%为小数，则向下取整）位置的值。对比出入向的95峰值带宽和保底带宽，以二者中最大值作为计费值。每月1号会自动产生一张上月带宽的后付费账单，若本月实际带宽未超过保底带宽，月初按保底带宽结算。调整保底带宽，实时生效，预计2个工作日完成；若执行月95峰值带宽升速，实时生效，下月初统一计费；月95峰值带宽降速则需完成本月计费后下月执行。
### 计费调整
- 固定带宽调整至月95峰值带宽：  
固定带宽后付费计费模式下，用户根据实际情况将计费模式调整为95峰值带宽，需结算上一笔固定带宽订单开始时间到当前时间的费用，且保底带宽调整至少为100Mbps。
- 月95峰值带宽调整至固定带宽：  
95峰值带宽后付费计费模式下，用户根据实际情况将计费模式调整为固定带宽，则需完成本月计费后下月执行。
### 计费示例
- 固定带宽：  
假设您在某月(自然月为30天)15号购买了50Mbps北京地域的普通BGP 类型的带宽，选择了固定带宽计费模式。则用户在下月初产生总费用为：（100元/Mbps/月 × 50Mbps）× 0.50个月 = 2500元；之后每月初结算，产生整月总费用为：（100元/Mbps/月 × 50Mbps）× 1个月 = 5000元。若中途需调整固定带宽（调速立即执行），且需结算上一笔固定带宽订单开始时间到当前时间的费用。
- 月95峰值带宽：  
基于以上固定带宽示例，若下月15号需将固定带宽调整为月95峰值带宽计费（调速立即执行），且需结算上一笔固定带宽订单开始时间到当前时间的费用，保底带宽调整至100Mbps,产生月95带宽峰值为200Mbps，则用户在下月初产生总费用为：（100元/Mbps/月 × 200Mbps）× 0.50个月 = 10000元；之后每月初结算，产生整月总费用为：（100元/Mbps/月 × 200Mbps）× 1个月 = 20000元。