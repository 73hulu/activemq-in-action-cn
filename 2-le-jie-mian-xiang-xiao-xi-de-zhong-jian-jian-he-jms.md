# 2、面向消息的中间件和JMS



ActiveMQ是一个MOM产品，为业务系统提供异步的消息功能。企业消息传递的目的是通过将消息从一个系统传递到另一个系统，在不同系统之间传递数据。

目前已有的产品：WebSphere MQ, Sonic MQ, TIBCO, Apache Active MQ

MOM背后的思想是，它在消息的发送和接收中间充当消息中介。

JMS为企业消息提供一套API。MessageProducer类往目的的发送消息。MessageConsumer类从目的的获取消息。当消息到达目的的时候讲调用MessageListener类的onMessage\(\)方法。JMS的提供者是实现JMS API的特定于供应商的MOM。JMS 消息传递数据和事件。

**发送模式**：持久化（消息只传递一次）和非持久化（最多一次）

**属性**：自定义的，JMS定义，特定的供应商定义

**选择器**：使用boolean逻辑过滤消息------只针对消息头和属性，而不是消息body

例如：

```text
      String selector = "SYMBOL = 'AAPL' AND PRICE &gt; " + getPrice\(\); 

      MessageConsumer consumer =  session.createConsumer\(destination, selector\);
```

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

