# 网络类常见问题

Q1：外网的相关操作中，哪些是实时生效的，哪些是实际通过工单操作实现的，具体的SLA是多少？

目前实时生效的操作有：固定带宽调整限速大小

实际通过工单操作的有：

|           操作名称           | 操作SLA |
| :--------------------------: | :-----: |
|       带宽计费方式切换       |   2天   |
| 95峰值带宽，调整保底带宽大小 |   2天   |
|         创建外网出口         |   4天   |
|         删除外网出口         |   4天   |
|       IP段移入外网出口       |   4天   |
|       IP段移出外网出口       |   4天   |


Q2：托管网段有没有什么使用限制？

  为了保障客户业务的稳定性，实时检测网段连通性，客户申请的每一个网段的网络号将会UCloud侧当作测试连通性的IP使用，即：每一个网段的网络号默认需保留，不能为客户所使用。如果强行使用，则当此网段出现故障时，UCloud侧无法及时发现问题。请客户知悉。
