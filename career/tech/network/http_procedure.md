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
    CA证书
        协商对称加密算法过程使用非对称加密本身不安全，会有中间人篡改公钥，所以不直接使用公钥，而是使用数字证书签发结构办法的证书来保证非对称加密本身的安全。
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

# option说明
“域”（Origin）是指浏览器地址栏中显示的主机地址。“跨域”，就是网页程序想要请求的地址和地址栏中的“域”不同。合起来就是，一个网页程序想要去其他主机地址索要资源。
* 简单请求
    - 请求方式只能是：GET、POST、HEAD
    - HTTP请求头限制这几种字段：Accept、Accept-Language、Content-Language、Content-Type、Last-Event-ID
    - Content-type只能取：application/x-www-form-urlencoded、multipart/form-data、text/plain
* 非简单请求
    - 请求方式是PUT或者DELETE
    - Content-Type字段类型是application/json
预检请求：非简单请求且跨域的情况下会发起options预检请求。在真正的请求发送出去之前，浏览器会先发送一个options请求向服务询问此接口是否允许访问，如果通过再次发送正式请求。如果没通过，那么浏览器不会发送正式请求。
    