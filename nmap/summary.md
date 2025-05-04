```shell
nmap [option] target
##普通权限用户与特殊权限用户：特殊权限用户扫描本地网络以外的目标可以不用完全进行TCP三次握手方式，普通权限用户必须完成三次握手

--- P  可以指定端口号，也可以组合使用，
示例: nmap P21 target
组合使用: nmap PR21 target 对目标主机的21号端口进行ARP扫描


--- sn  不对目标主机进行端口扫描
示例: nmap -PE -sn target 对目标主机进行ICMP回显扫描,并且不进行端口扫描


--- R  使用ARP扫描。
原理:利用ARP(地址解析协议)
示例: sudo nmap -PR -sn target/24 对目标子网进行ARP扫描，不进行端口扫描


--- E  使用ICMP回显扫描(INFO: ICMP echo)。
原理:发送ping请求,通过观察是否有返回数据包(reply)来判断主机是否存活
示例: sudo nmap -PE -sn target  对target进行ICMP回显扫描
## ICMP不能像TCP/UDP之类的指定端口号


--- p  使用ICMP时间戳扫描
原理:发送一个时间戳请求,如果返回了Reply，则代表着主机存活
sudo nmap -PP -sn target/24  对target子网进行主机探测 


--- M  使用ICMP地址掩码扫描
原理: 发送一个地址掩码请求，如果返回了Reply，则代表主机存货
sudo nmap -PM -sn target/24  对target子网进行主机探测 


--- S  使用TCP SYN(同步) Ping 扫描
原理:进行TCP/IP连接,如果返回了数据则代表主机存活
sudo nmap -PS -sn target/24   对target子网进行主机探测 


--- A  使用TCP ACK(确认) Ping 扫描
原理: 直接创建一个ACK标识的数据包发送到目标主机，如果对面主机返回了一个RST(重置)标识的数据包，则代表目标主机存活。因为TCP协议中，服务器在接收到ACK(确认)数据包的时候,需要进行确认是否建立了连接，而他是直接发送了一个ACK标识数据包，必定报错返回一个RST(重置数据包) ---必须使用特殊用户权限，不然会进行完整的TCP三次握手
sudo nmap -PA -sn target/24   对target子网进行主机探测


--- U  使用UDP ping扫描 
原理: 发送UDP数据不会进行返回，但是发送UDP数据包到关闭的端口，则会返回一个ICMP端口不可达的数据包，这也代表着主机存活
sudo nmap -PU -sn target/24   对target子网进行主机探测


--- n  无DNS查询
--- R  对所有主机进行反向DNS查询
--- sn  仅限主机发现
```

