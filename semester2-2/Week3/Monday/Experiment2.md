---
title: 计网第二个实验
categories: 计网
top: true
---



五、实验步骤                                      

1.数据规划（课程目标1）

表1-交换机VLAN划分及端口

| 设备名  | VLAN编号  | VLAN名称 | 端口范围              | 连接计算机    |
| ---- | ------- | ------ | ----------------- | -------- |
| LSW1 | vlan 10 | 技术部    | E 0/0/1 - E 0/0/2 | PC1, PC3 |
| LSW2 | vlan 20 | 财务部    | E 0/0/1 - E 0/0/2 | PC2, PC4 |

表2-PC计算机IP地址及端口信息

| 设备名 | 端口      | IP地址信息    | 网关            |
| --- | ------- | --------- | ------------- |
| PC1 | E 0/0/1 | 10.10.1.1 | 255.255.255.0 |
| PC2 | E 0/0/2 | 10.10.1.2 | 255.255.255.0 |
| PC3 | E 0/0/3 | 10.10.1.3 | 255.255.255.0 |
| PC4 | E 0/0/4 | 10.10.1.4 | 255.255.255.0 |

 2.启动有eNSP模拟器，按实验要求和规划，搭建网络拓扑。（课程目标3）
![Pasted image 20240325105802.png](https://img2.imgtp.com/2024/03/25/trp0rx6s.png)
![Pasted image 20240325105819.png](https://img2.imgtp.com/2024/03/25/zXYjCsX3.png)
![Pasted image 20240325105918.png](https://img2.imgtp.com/2024/03/25/BT39huMp.png)

**上图IP地址应为10.10.1.3，当时打错了，后面有纠正**

![Pasted image 20240325105929.png](https://img2.imgtp.com/2024/03/25/YWoj26hw.png)

3.设备配置。（课程目标3）
本次配置比起实验一来说，就是多了对于两个交换机的控制，其余的配置都是一样的，当我们配置完每台PC的IP之后，我们就可以进入交换机进行配置了。
首先我们进入LSW(交换机1)，然后我们首先关闭之后可能会出现的提示信息
```
undo info-center enable
```
接下来，我们首先配置第一个交换机所连接的两台PC和对应连接的一台交换机的端口。
操作还是和之前一样的操作
```
sys
vlan batch 10 20
```
首先进入sys
然后创建两个vlan 10和20
```
interface G 0/0/1 //G是一种端口的名称，指千兆以太网端口，而上个实验的E指的是普通以太网端口
port link-type access
port default vlan 10
```
这是第一台PC的配置
```
interface G 0/0/2
port link-type access
port default vlan 20
```
这是第二台PC的配置
接下来是本实验的核心内容，就是配置24端口的两台交换机，使得他们可以通过vlan10 和 vlan20这两个线路。这样的话就可以连接起来这两台交换机了。
```
interface G 0/0/24
port link-type trunk
port trunk allow-pass vlan 10 20
```
这样的话，这个交换机和交换机之间的线缆就可以通过10和20的接口了。
剩下的就是配置第二台交换机的配置，方法和上述方法一样，重复一下即可。
```
undo info-center enable
sys
vlan batch 10 20
interface G 0/0/1 //G是一种端口的名称，指千兆以太网端口，而上个实验的E指的是普通以太网端口
port link-type access
port default vlan 10
interface G 0/0/2
port link-type access
port default vlan 20
interface G 0/0/24
port link-type trunk
port trunk allow-pass vlan 10 20
```
![Pasted image 20240325114246.png](https://img2.imgtp.com/2024/03/25/Wax8a7rI.png)
我们不难看出vlan10和20这两个线路均有24接口通过。那么实验就完成了，接下来验证实验就可以了。

4.验证。（课程目标3）
PC2验证
![Pasted image 20240325114548.png](https://img2.imgtp.com/2024/03/25/nGHjP7zq.png)
PC1配置
![Pasted image 20240325114813.png](https://img2.imgtp.com/2024/03/25/5fgk27XM.png)
由图可知，pc1可以连3，2可以连4，这样的话我们的实验目的也就完成了

![Pasted image 20240325114956.png](https://img2.imgtp.com/2024/03/25/Eg8vjkjM.png)
拓扑图如上。
六、实验心得或存在的问题（课程目标1)
本次实验学到了有关不同楼层的两台交换机之间应该如何配置，以至于达到楼层之间的互联而不与同楼层之间的互联。
本次实验还学习到了有关`link-type trunk`以及trunk接口是用于交换机之间连接的端口，trunk口可以1加入多个vlan，以及接收和发送多个Vlan的Tagged帧。
还了解到了G口是千兆以太网口，E口是以太网口的知识点。