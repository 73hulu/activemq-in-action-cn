# 4、连接ActiveMQ

**连接器：**提供客户机到代理通信\(传输连接器\)或代理到代理通信\(网络连接器\)的机制

连接器URI：

URI=用于标识抽象的资源或者物理资源的字符串

tcp://localhost:61616 =在本机物理机上的61616创建TCP连接  

**传输连接器**

**用于接收和监听客户机连接的机制**

```text
<transportConnectors>
    <transportConnector name="openwire" uri="tcp://localhost:61616"
        discoveryUri="multicast://default"/>
    <transportConnector name="ssl" uri="ssl://localhost:61617"/>
    <transportConnector name="stomp" uri="stomp://localhost:61613"/>
    <transportConnector name="xmpp" uri="xmpp://localhost:61222"/>
</transportConnectors>
```

总结：

| protocol（协议） | 描述 |
| :--- | :--- |
| TCP | 传输控制协议。大多数用例的默认网络协议。高度可靠的主机对主机协议。tcp://hostname:port?key=value&key=value |
| NIO | NIO协议。为从生产者到代理的连接提供更好的可伸缩性                       nio://hostname:port?key=value |
| UDP | 用户数据报协议。处理客户端和代理之间的防火墙。UDP不保证包的顺序和唯一性，而且是无连接的。                                                                                                  udp://hostname:port?key=value |
| SSL | 安全套接层协议。允许安全通信。                                                             ssl://hostname:port?key=value                                                                                           System parameters:                                                                                                                           -Djavax.net.ssl.keyStore=client.ks                                                                                                   -Djavax.net.ssl.keyStorePassword="password"                                                                            -Djavax.net.ssl.trustStore=client.ts |
| HTTP\(S\) | 处理客户端和代理之间的防火墙。                                                                  http\[s\]://hostname:port?key=value |
| VM | 用于在相同的JVM进行通信                                                                                                                               vm://brokerName?key=value |

Keystores=持有您的私有证书                  Truststores=持有其他受信任证书的应用程序

代理网络创建一个由多个ActiveMQ实例组成的集群，这些实例相互连接以满足更高级的场景。

发现是检测远程代理服务的过程。

静态网络连接器用于创建多个代理的静态配置:

static:\(uri1,uri2,uri3,...\)?key=value  

![](.gitbook/assets/1%20%281%29.png)

IP组播是一种网络技术，用于传输到一组感兴趣的接收器。组地址是224.0.0.0到239.255.255.255之间的IP地址。代理使用多播技术通知其服务并定位其他代理的服务。客户端使用多播技术定位代理并建立连接。

multicast://ipadaddress:port?key=value

发现协议允许客户端发现代理并随机选择要连接的代理

对等协议允许创建嵌入式代理网络

客户端使用扇出协议连接多个代理并且在代理之间直接复制操作

