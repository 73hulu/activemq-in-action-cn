# 12、客户端高级选项

独家消费者（Exclusive Consumers）

ActiveMQ可以配置为只有一个使用者监听队列，以确保到达的顺序。如果使用者失败，另一个使用者将被激活。

消息组（Message Groups）

通过设置消息头JMSXGroupID，可以为单个使用者将消息分组在一起。

ActiveMQ流（ActiveMQ streams）

高级功能转移非常大的文件，通过分割成块

Blob messages

blob消息不包含要发送的数据，但是通知blob是可用的

ActiveMQ定时发送消息

可以将消息安排在延迟之后或定期发送。要发送的消息被持久存储。

