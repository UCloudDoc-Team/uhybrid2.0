# 服务等级协议
本服务等级协议（Service Level Agreement，以下简称 “SLA”）规定了UCloud向客户提供的线路类型为BGP带宽以及线路类型为单线带宽的服务可用性等级指标及赔偿方案。
## 定义
- 服务周期：一个服务周期为一个自然月。
- 服务周期总分钟数：按照服务周期内的总天数╳24（小时）╳60（分钟）计算。
- 服务不可用分钟数：当某一分钟内，带宽出方向所有数据包都在UCloud网络出口丢弃时，则视为该分钟内该带宽服务不可用。在一个服务周期内带宽不可用分钟数之和即服务不可用分钟数。
- 月度服务费用：客户在一个自然月中就带宽所支付的服务费用总额，如果客户一次性支付了多个月份的服务费用，则将按照所购买的月数分摊计算月度服务费用。
## 服务可用性
服务可用性按照服务周期进行统计，以单个带宽实例为维度，按照如下方式计算：
服务可用性=(服务周期总分钟数-服务不可用分钟数)/服务周期总分钟数×100%
假设当月为30天，服务月度可用时间应为 30天 × 24小时 × 60分钟 × 99.95% = 43178.4分钟，即可存在43200 - 43178.4 = 21.6分钟的不可用时间。
UCloud提供的BGP带宽服务可用性不低于99.95%，单线带宽服务可用性不低于99.9%。如未达到上述可用性标准（属于免责条款情形的除外），您可以根据本协议后续预定获得赔偿。赔偿范围不包括以下原因所导致的服务不可用时间：
- UCloud预先通知客户后进行系统维护所引起的，包括割接、维修、升级和模拟故障演练。
- 任何UCloud所属设备以外的网络、设备故障和配置调整引起的。
- 客户的应用程序受到黑客攻击而引起的，包括但不限于在遭到 DDoS 攻击时，公网 IP 被UCloud设置为黑洞状态和清洗状态而无法使用公网网络。
- 由后端服务器异常导致的问题。
- 客户维护不当和保密不当致数据、口令、密码等丢失或泄漏所引起的。
- 客户的疏忽和由客户授权的操作所引起的。
- 客户未遵循UCloud产品使用文档或使用建议引起的。
- 不可抗力引起的。
- 非UCloud原因造成的服务不可用或服务不达标的情况。
- 属于相关法律法规、相关协议、相关规则或UCloud单独发布的相关规则、说明等中所述的UCloud可以免责、免除赔偿责任等的情况。