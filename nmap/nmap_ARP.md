[TOC]

**ARP(地址解析协议):** 就是将mac地址转换成ip地址存储到交换机中（存在于子网内的协议）

运行流程:

PC1 --[我想要寻找这个mac地址的ip]-->交换机 ----[询问子网内的所有主机]--->PC2--[查询到了mac地址]--->交换机(并且进行存储) ----[将映射的ip进行返回]--->PC1

#  使用APR进行nmap主机发现:

```shell
nmap [option] target
--- sn   不进行端口扫描
--- PR   进行ARP扫描


### 特权用户与普通用户(扫描本地网络外的目标):
## 特权用户
--- Nmap使用ICMP回显请求、TCP ACK(确认)到80端口、TCP SYN(同步)到443端口以及ICMP时间戳请求
## 普通用户
--- Nmap通过向80和443端口发送SYN数据包来进行TCP三次握手

nmap -PR -sn target
PC1 ---(ARP Request)--> Server
		||
	如果主机存活返回以下数据
		||
		\/
PC1 <---(ARP Reply)--- Server
```

![局部截取_20250420_143419](.\img\局部截取_20250420_143419.png)

![局部截取_20250420_143540](.\img\局部截取_20250420_143540.png)

![1_mX7o0vd12cfNN2r4uEaI_Q](.\img\1_mX7o0vd12cfNN2r4uEaI_Q.png)





# 使用arp-scan

```shell
arp-scan --localhost   ====  arp-scan -l


sudo arp-scan -I eht0 -l
---向接口eth0上的所有有效IP地址发送ARP查询
```

![局部截取_20250420_144307](.\img\局部截取_20250420_144307.png)![局部截取_20250420_144340](.\img\局部截取_20250420_144340.png)