# 开通流程

## STEP1 售前咨询
>  [欢迎咨询](https://spt.ucloud.cn/30001)  —— 和专业售前咨询进入开通流程

## STEP2 售前咨询
### 1、测试资源对比
为便于用户进行相关测试，UCloud提供如下三种方式的测试：
|测试资源类型	|公有云主机测试	|混合云物理机测试	|特定需求组网测试
| ---	| ---	| ---	| ---	
|测试目标	|带宽、时延、丢包等	|带宽、时延、丢包等,综合测试，例如同时测试托管内外网	|带宽、时延、丢包等,进行相关的组网测试，例如设备线路等容灾测试
|交付时间	|提交测试工单后，1个工作日	|提交测试工单后，已存资源环境，2个工作日,若需新建, 预计一周起步	|按照实际需求沟通，准备时间两周起步
|配置	|2核/4G/10Mbps/系统按需	|万兆服务器/10Mbps	|万兆交换机/万兆服务器/外网带宽
|数量	|默认1台，超出部分沟通	|默认1台，超出部分沟通	|按需搭建
|测试时间	|一周	|一周	|一周


### 2、公有云主机/混合云物理机测试需求单
BU填写-略
### 3、特定需求组网测试
BU填写-略
### 4、售前测试资源规范
测试时间：7个工作日
测试带宽：5M
测试IP：4个
### 5、专线询价流程
若涉及到专线，且需要UCloud负责代签对应专线，执行专线询价流程。
BU关注-略
### 6、公有云测试资源
参考UCloud控制台对应的地域与可用区云主机资源。
### 7、 物理机测试资源
|可用区| 逻辑机房 |操作系统	| 数量
| ---	| ---	| ---	| ---	
|北京二B	|HB11	|CentOS 7.6 X64	|1
|北京二B	|HB11	|CentOS 7.9 X64	|1
|北京二B	|HB11	|Windows Server 2012 R2 Datacenter	|1
|北京二C	|HB03	|CentOS 7.6 X64	|1
|北京二C	|HB03	|Windows Server 2012 R2 Datacenter	|1
|北京二E	|HB06	|CentOS Linux release 7.6.1810	|1
|乌兰察布A	|HB13	|CentOS Linux release 7.9.2009 (Core)	|1
|上海二C	|HD06	|CentOS 7.6 X64	|2
|上海二C	|HD06	|Windows Server 2012 R2 Datacenter	|1
|上海二A	|HD08	|CentOS Linux release 7.6.1810 (Core)	|1
|广州二B	|HN02	|CentOS 7.6 X64	|1
|广州二B	|HN02	|Windows 2012	|1


## STEP3 需求确认
架构师负责填写确认

## STEP4 项目实施
### 项目实施前准备工作：
**1）新托管用户需在控制台上填写对应的机柜开电、入室、机柜布线以及提交对应需求工单等，相关工作准备就绪后UCloud运维人员会联系用户进行联调，操作过程有疑问请联系对应的客户经理/运维人员寻求支持；
2）存量托管用户新需求需在控制台上填写对应的入室（按照实际情况选择）、机柜布线以及提交对应需求工单等，相关工作准备就绪后UCloud运维人员会联系用户进行联调，操作过程有疑问请联系对应的客户经理/运维人员寻求支持；**

### 以双线双点模式来介绍开通流程
- 描述：双线双点模式中，用户 IDC 分别通过一条物理专线/光纤，与同地域两个UCloud接入点相连，再连接至互联网。
- 使用场景：适用于对关键业务可用性和弹性较高的场景。
- 成本：中。
![access_step41](/images/access_step41.png)

### 初始资源分配


#### 数据面配置
##### 静态路由对接场景

- UCloud侧配置
##### 互联IP端口配置

```python
interface Vlanif「客户vlan id」//本地属性
description uhybrid-bw-3cmqbu|客户名称
ip address x.x.x.x 255.255.255.252

interface Vlanif「客户vlan id」//本地属性
description uhybrid-bw-3cmqbu|客户名称
ip address x.x.x.x 255.255.255.252
```

##### 南向静态路由配置
```python
ip route-static x.x.x.0/24 Vlanif xx x.x.x.x tag 1000 description 客户名称
ip route-static x.x.x.0/24 Vlanif xx x.x.x.x tag 1000 description 客户名称
```

##### 北向路由发布
```python
route-policy S-to-O permit node 10
if-match tag 1000 //匹配南向用户静态路由

bgp 59077
ipv4-family unicast
import-route static route-policy S-to-O //发布南向用户静态路由
```

- 客户侧配置

互联IP端口配置
```python
interface 端口名称 //客户自定义三层口模式
description To_UCloud
ip address x.x.x.x 255.255.255.252

interface 端口名称 //客户自定义三层口模式
description To_UCloud
ip address x.x.x.x 255.255.255.252
```

##### 北向静态路由配置
```python
ip route-static 0.0.0.0/0 客户端口 x.x.x.x  description To_UCloud
ip route-static 0.0.0.0/0 客户端口 x.x.x.x  description To_UCloud
```
**北向路由说明**
如果用户有多条互联网线路冗余场景，北向路由配置需要通过策略路由实现。

**南向路由发布**
客户根据内网的架构规划，做合理的路由打通即可。

##### 动态路由对接场景「BGP对接为例」
- UCloud侧配置
端口配置忽略，这里仅展示BGP部分配置，仅供参考！
**UCloud侧BGP配置**

```python
ip ip-prefix IN-IPP-客户名称-V4 index 10 permit {ipv4-address mask-length}
ip ip-prefix IN-IPP-客户名称-V4 index 1000 deny 0.0.0.0 0 less-equal 32
bgp 59077
router-id 设备环回口IP
group UCloud-to-客户名称-V4 external
peer  UCloud-to-客户名称-V4 as-number 客户ASN
peer  UCloud-to-客户名称-V4 timer keepalive 3 hold 9
peer  UCloud-to-客户名称-V4 bfd min-tx-interval 200 min-rx-interval 200 detect-multiplier 5
peer  UCloud-to-客户名称-V4 bfd enable
peer x.x.x.x as-number 客户ASN
peer x.x.x.x group  UCloud-to-客户名称-V4

ipv4-family unicast
compare-different-as-med
preference 20 200 200
maximum load-balancing ebgp 32
ext-community-change enable
peer  UCloud-to-客户名称-V4 enable
peer  UCloud-to-客户名称-V4 advertise-community
peer  UCloud-to-客户名称-V4 advertise-ext-community
peer  UCloud-to-客户名称-V4 ip-prefix IN-IPP-客户名称-V4 import
peer x.x.x.x enable
peer x.x.x.x group UCloud-to-客户名称-V4
```
**客户侧BGP配置**
```python
bgp 客户ASN
router-id 设备环回口IP
group TO_UCloud_IPT-V4 external
peer  TO_UCloud_IPT-V4 as-number 59077
peer  TO_UCloud_IPT-V4 timer keepalive 3 hold 9
peer  TO_UCloud_IPT-V4 bfd min-tx-interval 200 min-rx-interval 200 detect-multiplier 5
peer  TO_UCloud_IPT-V4 bfd enable
peer x.x.x.x as-number 59077
peer x.x.x.x group  TO_UCloud_IPT-V4

ipv4-family unicast
compare-different-as-med
preference 20 200 200
maximum load-balancing ebgp 32
ext-community-change enable
peer  TO_UCloud_IPT-V4 enable
peer  TO_UCloud_IPT-V4 advertise-community
peer  TO_UCloud_IPT-V4 advertise-ext-community
peer x.x.x.x enable
peer x.x.x.x group TO_UCloud_IPT-V4
```

**客户北向路由配置**
UCloud侧可以通过BGP下发默认路由给用户当作北向默认网关，鉴于客户内网的架构规划不同，特别是有多互联网线路备份接入场景；强烈建议客户北向路由自己定义配置方案

## STEP5 项目验收
> 主要用于介绍互联网带宽引入后指导客户完成产品测试及验收。

### 一、对接端口状态验：
包括端口up/down、端口错误包、光口情况下端口收发光、电口情况下对应的双工与速录协商情况、端口流量情况以及直连连通性测试
#### 1、检查项：端口up/down
##### 检查方法：
> dis int 10ge 1/0/1 | in current state|up|down （端口按照实际情况替换）
##### 预期结果
10GE1/0/1 current state : UP (ifindex: 53)
 Line protocol current state : UP
 Duplex: FULL, Negotiation: DISABLE
 Last physical up time : 2021-09-07 18:56:45
 Last physical down time : 2021-09-07 18:47:18
 
**端口UP;**
**up/down 时间戳与操作时间一致，无端口状态翻动情况**

#### 2、检查项：端口错误包
##### 检查方法：
> dis int 10ge 1/0/1 | in Total Error
##### 预期结果
Total Error: 0
**无错误包，若存在错误包，多次执行检查计数器不应增长**

#### 3、检查项：光口情况下端口收发光
##### 检查方法：
> dis int 10ge 1/0/1 tr ver
##### 预期结果

10GE1/0/1 transceiver information:

————————————————————————————————————————————

 Common information:
 Transceiver Type :10GBASE_SR
 Connector Type :LC
 Wavelength (nm) :850
 Transfer Distance (m) :30(62.5um/125um OM1)
 80(50um/125um OM2)
 300(50um/125um OM3)
 400(50um/125um OM4)
 Digital Diagnostic Monitoring :YES
 Vendor Name :OEM
 Vendor Part Number :SFP+10G300MSR
 Ordering Name :
 
————————————————————————————————————————————

 Manufacture information:
 Manu. Serial Number :STHC85SA2000042
 Manufacturing Date :2020-05-15
 Vendor Name :OEM
 
————————————————————————————————————————————

 Alarm information:
 Non-Huawei-Ethernet-Switch-Certified Transceiver
 
————————————————————————————————————————————

 Diagnostic information:
 Temperature (Celsius) :44.13
 Voltage (V) :3.23
 Bias Current (mA) :7.37
 Bias High Threshold (mA) :15.00
 Bias Low Threshold (mA) :0.50
 Current RX Power (dBm) :-0.28
 Default RX Power High Threshold (dBm) :1.00
 Default RX Power Low Threshold (dBm) :-15.00
 Current TX Power (dBm) :-1.98
 Default TX Power High Threshold (dBm) :1.00
 Default TX Power Low Threshold (dBm) :-9.00
 
 **两侧收发光均在正常范围内**

#### 4、检查项：电口情况下对应的双工与速录协商情况
##### 检查方法：
> dis int ge 1/0/1
##### 预期结果
Port Mode: COMMON COPPER, Port Split: -
 Speed: 1000, Loopback: NONE
 Duplex: FULL, Negotiation: ENABLE
**双工以及协商出对应端口/模块(光转电场景下）速率，采用自动协商端口无法up的情况下，可考虑关闭协商，强制双工与速率。**

#### 5、检查项：连通性测试
##### 检查方法：
> ping -a 172.20.200.6 -c 100 -m 50 172.20.200.5 (按需替换为对应的互联地址）
##### 预期结果
--- 172.20.200.5 ping statistics ---
 100 packet(s) transmitted
 100 packet(s) received
 0.00% packet loss
 round-trip min/avg/max = 1/1/3 ms
 
**无丢包且时延稳定**


### 二、 丢包率/延时指标测试
- 本地终端设备测试连通性和延时
打开本地终端执行PING命令，即可测试。
![access_step51](/images/access_step51.png)

- 第三方评测工具，如：基调/博睿测试
主要用于国内传统互联网IDC 访问网页的性能、网络质量测试。
![access_step52](/images/access_step52.png) 

**进入企业版后可选择即时测试和持续测试**

#### 即时测试
![access_step53](/images/access_step53.png)
![access_step54](/images/access_step54.png) 

#### 持续测试
![access_step55](/images/access_step55.png)
 
#### 测试报告查看
![access_step56](/images/access_step56.png)

### 三、带宽、带宽吞吐量测试：
互联网转接带宽接入完成后，您需要测试链路的性能，确保接入带宽可以满足您的业务需求。本文介绍通过Netperf和iPerf3工具测试带宽性能的方法。
安装Netperf和iPerf3
#### 1、安装Netperf
##### 执行以下命令，下载Netperf安装包
```python
wget -c "https://codeload.github.com/HewlettPackard/netperf/tar.gz/netperf-2.5.0" -O netperf-2.5.0.tar.gz
```
##### 依次执行以下命令，安装Netperf
```python
tar -zxvf netperf-2.5.0.tar.gz
cd netperf-netperf-2.5.0
./configure 
make 
make install
```
##### 执行netperf -V或netserver -V，验证安装是否成功
```python
回显以下信息表示成功
Netperf version 2.5.0
```

#### 2、安装iPerf3
##### 执行以下命令，下载iPerf3
```python
yum install git -y  
git clone https://github.com/esnet/iperf
```
##### 执行以下命令，安装iPerf3
```python
cd iperf
./configure && make && make install && cd ..
cd src
ADD_PATH="$(pwd)" 
PATH="${ADD_PATH}:${PATH}"
export PATH
```
##### 执行iperf3 -v命令，验证安装是否成功
```python
系统回显以下信息时，表示安装成功。
iperf 3.10.1+ (cJSON 1.7.13)
Linux iZbp15y0zrhx2ry6vo1b4wZ 3.10.0-957.21.3.el7.x86_64 #1 SMP Tue Jun 18 16:35:19 UTC 2019 x86_64
```
##### 开启多队列功能
假设与互联网转接产品相连的接口为eth0，在服务器端执行ethtool -L eth0 combined 4命令，开启多队列功能。执行命令后，系统回显以下信息：
##### 开启网卡多队列
```python
echo "ff" > /sys/class/net/eth0/queues/rx-0/rps_cpus
echo "ff" > /sys/class/net/eth0/queues/rx-1/rps_cpus
echo "ff" > /sys/class/net/eth0/queues/rx-2/rps_cpus
echo "ff" > /sys/class/net/eth0/queues/rx-3/rps_cpus
```

### 三、使用Netperf工具测试物理专线的包转发性能
#### Netperf概述
Netperf安装完成后会创建两个命令行工具：netserver（服务端：接收端工具）和netperf（客户端：发送端工具），主要参数说明如下表所示。
|工具名称	|主要参数	|参数说明
| ---	|---		|---	
|netserver	|-p	|监听的端口号。
|netperf	|-H	|IDC网络接入设备或ECS实例的IP地址。
|netperf	|-p	|IDC网络接入设备或ECS实例的端口。
|netperf	|-l	|运行时间。
|netperf	|-t	|发送报文的协议类型：TCP_STREAM或UDP_STREAM。推荐使用UDP_STREAM。
|netperf	|-m	|数据包大小: 测试pps（packet per second）时，建议设置为1;测试bps（bit per second）时，建议设置为1400。

#### 分析测试结果
客户端的netperf进程执行完毕后，会显示以下结果。通过发送成功的报文数除以测试时间，计算出测试链路的pps，即pps=发送成功的报文数÷测试时间。
**分析测试结果**
```python
Socket  Message  Elapsed      Messages
Size    Size     Time         Okay Errors   Throughput
bytes   bytes    secs            #      #   10^6bits/sec
124928       1   10.00     4532554      0       3.63
212992           10.00     1099999              0.88
```
**显示结果中各字段含义如下表所示。**
|字段	|含义
| ---		| ---	
|Socket Size	|缓冲区大小
|Message Size	|数据包大小（Byte）
|Elapsed Time	|测试时间（s）
|Message Okay	|发送成功的报文数
|Message Errors	|发送失败的报文数
|Throughput	|网络吞吐量（Mbps）



### 四、使用iPerf3测试物理专线的带宽
#### iPerf3概述
iPerf3的主要参数说明如下表所示。

|主要参数	|参数说明
| ---		| ---	
|-s	|服务端专用参数，表示iPerf3以服务端模式运行。
|-c	|客户端专用参数，表示iPerf3以客户端模式运行。
|-i	|设置每次报告之间的时间间隔，单位为秒。
|-p	|服务端：指定服务端监听的端口，默认为5201，同时监听TCP/UDP。
|	|客户端：指定客户端连接服务端的端口，默认为5201。如果同时有-u参数，表示通过UDP发起连接，否则默认使用TCP连接。
|	|
|-u	|表示使用UDP协议发送报文。若不指定该参数则表示使用TCP协议。
|-l	|设置读写缓冲区的长度。通常测试包转发性能时建议该值设为16，测试带宽时建议该值设为1400。
|-b	|UDP模式使用的带宽，单位bit/s。
|-t	|设置传输的总时间。iPerf3在指定时间内，重复发送指定长度数据包的时间，默认值为10秒。
|-A	|设置CPU亲和性，可以将iPerf3进程绑定对应编号的逻辑CPU，避免iPerf3进程在不同的CPU间被调度。

##### 1、服务端启动进程，开启端口
**开启服务端口**
```python
iperf3 -s -i 1 -p 16001
```

##### 2、客户端上执行iperf3 -u -I 16 -b 100m -t 120 -c server_ip -i 1 -p port -A 1命令
**客户端执行**
```python
iperf3 -u -l 16 -b 100m -t 120 -c 192.168.100.1 -i 1 -p 16001 -A 1
```

##### 分析测试结果
客户端的iPerf3进程执行完毕后，会显示以下结果。通过将对端收到的包数除以时间，计算出测试链路的pps，即pps=对端收到的包÷时间。
**结果分析**
```python
[ ID]  Interval        Transfer    Bandwidth      Jitter    Lost/Total Datagrams
[  4]  0.00-10.00 sec  237 MBytes  199 Mbits/sec  0.027 ms  500/30352 (1.6%)
[  4]  Sent 30352  datagrams
```
**显示结果中各字段含义如下表所示**
|字段	|含义
| ---		| ---	
|Transfer	|传输的总数据量
|Bandwidth	|带宽大小
|Jitter	|抖动

**备注
项目实施完毕后，客户默认有3个工作日的验收时间，验收期间有任何问题请联系对应架构师；如验收通过，第四个工作日开始执行项目计费。**



## STEP6 项目计费
用户验收通过后， 产品运营针对运维提交的资源ID与销售确认计费事宜。计费内容包括 带宽费用、IP租赁费用、端口月租费用，具体详见网络类计费方式

## STEP7 日常运维
- 现场准备工作完成前提下，UCloud侧2个工作日完成对应的开通操作、带宽与IP等各类调整工作；
- 提供7*24小时的技术支持和故障维修服务电话热线，15分钟的故障响应，重点用户提供VIP技术支持服务，其他业务需求一律咨询客户经理；
- 主动网络维护提前72小时通知；
- 常见业务问题30min内回复，疑难杂症24h内予以回复；
- 专业团队负责对线路进行维护、故障排除和检修。

