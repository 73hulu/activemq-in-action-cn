# 11、运行中ActiveMQ代理的特性

**使用通配符从多个目的地消费**

Session.createTopic\(“_._.SubSubGroup”\);

向多个目的地发送消息

**复合目的地使用逗号分隔的名称作为目的地名称**

session.createQueue\("store.orders, topic://store.orders"\);

#### 通知消息（Advisory Message）

通知消息是在系统定义的主题上生成的常规JMS消息。它们是JMX的一个很好的替代选择。

**虚拟主题（Virtual Topics）**

虚拟主题运行发布者往正常的JMS主题发送消息，消费者从正常的队列接收消息。主题名字应遵循以下模式：VirtualTopic.&lt;topic name&gt;。虚拟主题是一种方便的机制，可以将队列的负载平衡和故障转移方面与主题的持久性结合起来。如果一个消费者宕机，其他的消费者将会继续从队列接收消息。

**追溯消费者\(Retroactive consumers\)**

ActiveMQ能够缓存发送到主题的可配置大小的消息。消费者需要通知ActiveMQ它感兴趣的可追溯的消息，代理需要配置缓存的大小

**消息传递和死信队列（Message delivery and dead-letter queues）**

当消息在ActiveMQ代理失效（它们超过的生命周期），或者不能被发送，它们将会被移送到死信队列。可配置的POJO\(消息重发策略\)与可以调优到不同策略的连接相关联。

所有消息都有一个死信队列，称为ActiveMQ.DLQ。当一个消息被发送到死信队列时，将为它产生一个通知消息。

**使用拦截器插件扩展功能**

ActiveMQ提供自定义代码以补充代理功能的能力。可视化绘制所有连接和目的地的图表。日志记录消息在一个ActiveMQ代理中的发送和确认。

**Apache Camel framework**

Camel framework是一个路由引擎构建器，它允许你定义自己的规则，源、目的地以及如何处理消息。它扩展ActiveMQ的灵活性和功能性

