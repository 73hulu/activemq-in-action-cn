# 13、ActiveMQ性能调优

持久化

默认发送模式是持久化。非持久消息比持久消息快得多\(生产者确实需要等待代理的收据\)。

```text
MessageProducer producer = session.createProducer(topic);
producer.setDeliveryMode(DeliveryMode.NON_PERSISTENT);
```

ActiveMQ非持久化发送是可靠的：只要生产者是活动的，消息的传递将在网络中断和系统崩溃中存活:它在故障转移传输缓存中保存消息以进行重新传递。

事务

可以批量生产或者消费消息来提高性能

```text
Session session = connection.createSession(true, Session.SESSION_TRANSACTED);
producer.send(message);
session.commit();
```

嵌入代理

```text
ActiveMQConnectionFactory cf = new ActiveMQConnectionFactory("vm://service");
```

Open Wire Protocol

用于将命令通过传输\(如TCP\)传输到代理的二进制格式

异步发送

告诉MessageProducer，除了发送给ActiveMQ代理的消息的收据外，不要发送其他信息。

```text
ActiveMQConnectionFactory cf = new ActiveMQConnectionFactory();
cf.setUseAsyncSend(true);
```

生产流程控制

生产流程控制允许消息代理在资源不足的情况下减慢消息的发送速率

消费者

一些最大的性能收益是通过调优消费者获得的。

ActiveMQ应答模式

* Session.SESSION\_TRANSACTED： 事务，用Session.commit\(\)
*  Session.CLIENT\_ACKNOWLEDGE： 手动应答
*  Session.AUTO\_ACKNOWLEDGE：自动应答
*  Session.DUPS\_OK\_ACKNOWLEDGE：延迟提交
* ActiveMQSession.INDIVIDUAL\_ACKNOWLEDGE:对所使用的每个消息发送一个确认

