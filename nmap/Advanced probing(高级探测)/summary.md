|                Port Scan Type  端口扫描类型                 |              Example Command  示例命令               |
| :---------------------------------------------------------: | :--------------------------------------------------: |
|                  TCP Null Scan  TCP 空扫描                  |              sudo nmap -sN 10.10.112.7               |
|                 TCP FIN Scan  TCP FIN 扫描                  |              sudo nmap -sF 10.10.112.7               |
|                TCP Xmas Scan  TCP 圣诞树扫描                |              sudo nmap -sX 10.10.112.7               |
|              TCP Maimon Scan  TCP Maimon 扫描               |              sudo nmap -sM 10.10.112.7               |
|                 TCP ACK Scan  TCP ACK 扫描                  |              sudo nmap -sA 10.10.112.7               |
|                TCP Window Scan  TCP 窗口扫描                |              sudo nmap -sW 10.10.112.7               |
|              Custom TCP Scan  自定义 TCP 扫描               | sudo nmap --scanflags URGACKPSHRSTSYNFIN 10.10.112.7 |
|               Spoofed Source IP  伪造的源 IP                |         sudo nmap -S SPOOFED_IP 10.10.112.7          |
|            Spoofed MAC Address  伪造的 MAC 地址             |               --spoof-mac SPOOFED_MAC                |
|                    Decoy Scan  诱饵扫描                     |           nmap -D DECOY_IP,ME 10.10.112.7            |
|            Idle (Zombie) Scan  空闲（僵尸）扫描             |         sudo nmap -sI ZOMBIE_IP 10.10.112.7          |
|  Fragment IP data into 8 bytes<br/>将 IP 数据分片为 8 字节  |                          -f                          |
| Fragment IP data into 16 bytes<br/>将 IP 数据分片为 16 字节 |                         -ff                          |



| Option  选项           | Purpose  目的                                                |
| ---------------------- | ------------------------------------------------------------ |
| --source-port PORT_NUM | specify source port number<br/>指定源端口号                  |
| --data-length NUM      | append random data to reach given length<br/>附加随机数据以达到指定长度 |



这些扫描类型依赖于以意想不到的方式设置 TCP 标志来促使端口回复。Null、FIN 和 Xmas 扫描会引发关闭端口的响应，而 Maimon、ACK 和 Window 扫描会引发开放和关闭端口的响应。

| Option  选项 | Purpose  目的                                                |
| ------------ | ------------------------------------------------------------ |
| --reason     | explains how Nmap made its conclusion<br/>解释 Nmap 如何得出结论 |
| -v           | verbose  详细模式                                            |
| -vv          | very verbose  非常冗长                                       |
| -d           | debugging  调试                                              |
| -dd          | more details for debugging<br/>更多调试细节                  |

