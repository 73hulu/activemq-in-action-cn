# activemq-in-action

## **1、ActiveMQ Action入门**

MOM\(Message-Oriented Middleware\)：面向消息的中间件

ActiveMQ为应用程序体系结构提供了松耦合的好处。向ActiveMQ发送消息的应用程序不关心消息是如何或者何时~~传递~~的。消费应用程序也不关心消息的来源或者他们如何发送到ActiveMQ。

ActiveMQ充当中间件的作用，允许以异步方式进行异构集成和交互。

耦合指两个或多个应用程序或系统之间的相互依赖。使用RPC，当一个应用程序调用另一个应用程序时，调用方将被阻塞，直到被调用方将控制权返回给调用方。

![](.gitbook/assets/1%20%283%29.png)

什么时候使用ActiveMQ

* 异构的应用程序（跨语言）
* RPC的替代品
* 应用程序之间的解耦
* 作为事件驱动架构的主干:当在Amazon下订单时，MQ将接受消息并立即应答。流程中的其余步骤是异步处理的。如果发生错误，将会通过邮件通知用户。这允许大量的可伸缩性和高可用性。
* 提高应用程序的可伸缩性

## 2、了解面向消息的中间件和JMS

ActiveMQ是一个MOM产品，为业务系统提供异步的消息功能。企业消息传递的目的是通过将消息从一个系统传递到另一个系统，在不同系统之间传递数据。

目前已有的产品：WebSphere MQ, Sonic MQ, TIBCO, Apache Active MQ

MOM背后的思想是，它在消息的发送和接收中间充当消息中介。

JMS为企业消息提供一套API。MessageProducer类往目的的发送消息。MessageConsumer类从目的的获取消息。当消息到达目的的时候讲调用MessageListener类的onMessage\(\)方法。JMS的提供者是实现JMS API的特定于供应商的MOM。JMS 消息传递数据和事件。

**发送模式**：持久化（消息只传递一次）和非持久化（最多一次）

**属性**：自定义的，JMS定义，特定的供应商定义

**选择器**：使用boolean逻辑过滤消息------只针对消息头和属性，而不是消息body

例如：

          String selector = "SYMBOL = 'AAPL' AND PRICE &gt; " + getPrice\(\); 

          MessageConsumer consumer =  session.createConsumer\(destination, selector\);

**消息类型**（总共6种）：

* Message:这表示没有消息主体的消息
* StreamMessage:包含Java原语类型流的消息。它是按顺序写和读的
* MapMessage:包含一组名称/值对的消息。条目的顺序没有定义
* TextMessage:其主体包含Java字符串的消息…例如XML消息
* ObjectMessage:其主体包含序列化Java对象的消息
* BytesMessage:其正文包含未解释字节流的消息

**模式**：P2P（ Queue队列模式）和pub/sub（ Topic主题模式）

**管理对象**：ConnectionFactory and Destination

**流程**：

* 获得ConnectionFactory 
* 通过ConnectionFactory 创建Connection连接
* 启动JMS连接
* 从Connetion连接获取session
* 获取JMS的目的的
* 创建JMS的消息生产者
  * 创建生产者
  * 创建JMS消息并将其地址定位到目的地
* 创建JMS的消息消费者
  * 创建消费者
  * 可选地注册JMS消息侦听器
* 发送或者接收消息
* 关闭JMS的所有资源

消息接收有两种方式：

* 同步接收：调用receive\(\)方法。如：consumer.receive\(1000\);
* 异步接收：通过监听器方式接收。如：consumer.setMessageListener\(MessageListener\);

## 3、**示例**

**示例1：主题模式**

![topic&#x6A21;&#x5F0F;](.gitbook/assets/2.png)

示例2：队列模式

![Queue&#x6A21;&#x5F0F;](.gitbook/assets/3%20%281%29.png)

## **4、连接ActiveMQ**



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



