# 请求流程
## 客户端
应用层
    DNS:
        浏览器
        hosts文件
        路由器
        服务运营商
        根域名服务器
    http
会话层
    会话控制和同步（视频聊天：sip）
表示层
    解决两个系统间交换信息的语法与语义问题，还有数据表示转化，转为主机无关编码，加解密和压缩与解压缩功能（视频的编码与压缩 H.264）
安全层
    TLS(建立在SSL 3.0协议规范之上,所支持的加密算法不同):
        hello   
                            hello
                            certificate
                            hellodone
        clientkeyexchange
        changecipherspec
        finish
                            changecipherspec
                            finish
    SSL:
        为Netscape所研发，用以保障在Internet上数据传输之安全，利用数据加密（Encryption）技术，可确保数据在网络上之传输过程中不会被截取。
传输层
    TCP
    UDP
ip层
    IP协议
数据链路层
    ARP（上层使用的udp广播）
    RARP（dhcp动态分配ip）
物理层
    有线：802.3
    无线：802.11

## 服务端
防火墙
    网络防火墙(只允许符合特定规则的封包通过）
    应用防火墙(拦截进出某应用程序的所有封包)
负载均衡
    nginx(对请求进行分配，让压力不集中在一台服务器)

    