[TOC]



​	**伪造源 IP 地址可以是一种隐蔽扫描的好方法**。然而，**伪造只在特定的网络环境下有效**。它**要求你处于能够监控流量的位置**。考虑到这些限制，**伪造 IP 地址的用途可能有限**；不过，我们可以**通过闲置扫描对其进行升级。**

​	空闲扫描，或称僵尸扫描，需要一个连接到网络且处于空闲状态的系统，您可以与之通信。实际上，**Nmap 会使每个探测看起来像是来自空闲（僵尸）主机，然后检查空闲（僵尸）主机是否收到了伪造探测的任何响应。**这是**通过检查 IP 头中的 IP 标识（IP ID）值来实现的**。您可以使用 `nmap -sI ZOMBIE_IP 10.10.4.173` 运行空闲扫描，其中` ZOMBIE_IP `是空闲主机（僵尸）的 IP 地址。



### 空闲（僵尸）扫描需要以下三个步骤来确定端口是否开放：

1. 触发空闲主机响应，以便您可以记录空闲主机当前的 IP ID。
2. 向目标的 TCP 端口发送 SYN 数据包。该数据包应伪装成来自空闲主机（僵尸）IP 地址。
3. 再次触发空闲机器响应，以便您可以将新的 IP ID 与之前收到的进行比较。



让我们用图示来说明。下图中，攻击者系统正在**探测一台空闲机器**，**一台多功能打印机**。通过**发送 SYN/ACK**，它会**收到一个包含新递增 IP ID 的 RST 数据包作为响应**。

![a93e181f0effe000554a8b307448bbb2](./../img/a93e181f0effe000554a8b307448bbb2.png)



​	攻击者将在下一步**向目标机器**上他们**想要检查的 TCP 端口发送一个 SYN 数据包**。然而，该**数据包将使用空闲主机（僵尸主机）的 IP 地址作为源地址**。

**会出现三种情况**

- 在第一种情况中，如下图所示，**目标主机TCP 端口是关闭的**；因此，**目标机器向空闲主机响应一个 RST 数据包**。空闲主机**不响应**，因此其 **IP ID 不会增加**。

![8e28bf940936ddbc2367b193ea3550b8](./../img/8e28bf940936ddbc2367b193ea3550b8.png)

- 在第二种情况下，如下所示，**目标主机TCP 端口是开放**的，因此**目标机器向空闲主机（僵尸）响应一个 SYN/ACK**。**空闲主机**对这个意外的数据包**响应一个 RST 包**，从而**增加了它的 IP ID**。

![2b0de492e2154a30760852e07cebae0e](./../img/2b0de492e2154a30760852e07cebae0e.png)

- 在第三种情况下，由于**防火墙规则**，**目标机器完全没有响应**。这种**无响应将导致与端口关闭时相同的结果**；**空闲主机不会增加 IP ID**。



​	在最后一步，**攻击者向空闲主机发送另一个 SYN/ACK**。**空闲主机响应一个 RST 包，IP ID 再次加一**。攻击者需要**比较第一步收到的 RST 包的 IP ID 与第三步收到的 RST 包的 IP ID**。

- 如果**差值为 1**，表示目**标机器上的端口被关闭或过滤**；

- 如果**差值为 2，表示目标上的端口是开放的**。



# 获取更多详细信息

如果您希望 Nmap 提供更多关于其推理和结论的细节，您可以考虑添加 `--reason` 。请考虑下面对系统的两次扫描；然而，后者添加了 `--reason `

```powershell
pentester@TryHackMe$ sudo nmap -sS 10.10.252.27

Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 10:39 BST
Nmap scan report for ip-10-10-252-27.eu-west-1.compute.internal (10.10.252.27)
Host is up (0.0020s latency).
Not shown: 994 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
25/tcp  open  smtp
80/tcp  open  http
110/tcp open  pop3
111/tcp open  rpcbind
143/tcp open  imap
MAC Address: 02:45:BF:8A:2D:6B (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.60 seconds
```



```powershell
pentester@TryHackMe$ sudo nmap -sS --reason 10.10.252.27

Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 10:40 BST
Nmap scan report for ip-10-10-252-27.eu-west-1.compute.internal (10.10.252.27)
Host is up, received arp-response (0.0020s latency).
Not shown: 994 closed ports
Reason: 994 resets
PORT    STATE SERVICE REASON
22/tcp  open  ssh     syn-ack ttl 64
25/tcp  open  smtp    syn-ack ttl 64
80/tcp  open  http    syn-ack ttl 64
110/tcp open  pop3    syn-ack ttl 64
111/tcp open  rpcbind syn-ack ttl 64
143/tcp open  imap    syn-ack ttl 64
MAC Address: 02:45:BF:8A:2D:6B (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.59 seconds
```



提供 --reason 标志可以让我们明确知道 Nmap 为什么判断系统在线或某个端口开放。在上面的控制台输出中，我们可以看到该系统被认为是在线的，因为 Nmap “收到了 arp-response”。另一方面，我们知道 SSH 端口被认为是开放的，因为 Nmap 收到了一个 “syn-ack” 包。

对于更详细的输出，您可以考虑使用 -v 以获得详细输出，或使用 -vv 以获得更详细的输出。

```powershell
pentester@TryHackMe$ sudo nmap -sS -vv 10.10.252.27

Starting Nmap 7.60 ( https://nmap.org ) at 2021-08-30 10:41 BST
Initiating ARP Ping Scan at 10:41
Scanning 10.10.252.27 [1 port]
Completed ARP Ping Scan at 10:41, 0.22s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 10:41
Completed Parallel DNS resolution of 1 host. at 10:41, 0.00s elapsed
Initiating SYN Stealth Scan at 10:41
Scanning ip-10-10-252-27.eu-west-1.compute.internal (10.10.252.27) [1000 ports]
Discovered open port 22/tcp on 10.10.252.27
Discovered open port 25/tcp on 10.10.252.27
Discovered open port 80/tcp on 10.10.252.27
Discovered open port 110/tcp on 10.10.252.27
Discovered open port 111/tcp on 10.10.252.27
Discovered open port 143/tcp on 10.10.252.27
Completed SYN Stealth Scan at 10:41, 1.25s elapsed (1000 total ports)
Nmap scan report for ip-10-10-252-27.eu-west-1.compute.internal (10.10.252.27)
Host is up, received arp-response (0.0019s latency).
Scanned at 2021-08-30 10:41:02 BST for 1s
Not shown: 994 closed ports
Reason: 994 resets
PORT    STATE SERVICE REASON
22/tcp  open  ssh     syn-ack ttl 64
25/tcp  open  smtp    syn-ack ttl 64
80/tcp  open  http    syn-ack ttl 64
110/tcp open  pop3    syn-ack ttl 64
111/tcp open  rpcbind syn-ack ttl 64
143/tcp open  imap    syn-ack ttl 64
MAC Address: 02:45:BF:8A:2D:6B (Unknown)

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 1.59 seconds
           Raw packets sent: 1002 (44.072KB) | Rcvd: 1002 (40.092KB)
```

如果 `-vv `不能满足您的好奇心，您可以使用`-d `查看调试详情，或使用 -dd 查看更详细的信息。您可以保证使用 `-d `将生成超出单个屏幕的输出。
