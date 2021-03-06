消息格式：固定报文头 | 可变报文头 | 负荷
# 固定报文头
（2-5字节）：1字节-消息类型和QOS。2到5字节-可变报文头加负荷的总长度（单个字节可以描述的最大长度是127字节，第八位起“延续位”的作用，能发送的最大消息长度是256MB）
## QOS
QoS 0：最多分发一次。消息的传递完全依赖底层的TCP/IP网络，协议里没有定义应答和重试，消息要么只会到达服务端一次，要么根本没有到达  
QoS 1：至少分发一次。服务器的消息接收由PUBACK消息进行确认，如果通信链路或发送设备异常，或者指定时间内没有收到确认消息，发送端会重发这条在消息头中设置了DUP位的消息  
QoS 2：只分发一次。这是最高级别的消息传递，消息丢失和重复都是不可接受的。 
通过下面的例子可以更深刻的理解上面三个传输质量等级  
# 可变报文头
可变报文头主要包含协议名、协议版本、连接标志（Connect Flags）、心跳间隔时间（Keep Alive timer）、连接返回码（Connect Return Code）、主题名（Topic Name）等
## Retained标志位
消息即为保留消息，1个主题只有一个Retained消息，Broker会保存每个Topic的最后一条Retained消息。每个MQTT客户端连接服务器，订阅Topic主题后会立即收到该Topic的Retained消息.只要向这个主题发布一个Payload长度为0的Retained消息即可删除
## keep alive标志位
在没有业务数据的情况下，客户端会根据设置周期发一个PINGREQ消息，相应的，服务器会返回一个PINGRESP消息进行确认。如果服务器在1.5心跳周期内没有收到来自客户端的消息，就会断开与客户端的连接。心跳间隔时间最大值大约可以设置为18个小时，0值意味着客户端不断开。
## will flag
配合Will QoS、Will Retain。服务器与客户端通信时，当遇到异常或客户端心跳超时的情况，MQTT服务器会替客户端发布一个Will消息。当然如果服务器收到来自客户端的DISCONNECT消息，则不会触发Will消息的发送。 
# 负荷
消息类型是CONNECT、PUBLISH、SUBSCRIBE、SUBACK、UNSUBSCRIBE时，则会带有负荷
