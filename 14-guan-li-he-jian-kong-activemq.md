# 14、管理和监控ActiveMQ

**JMX Agent：**用于公开ActiveMQ mbean。变量SUNJMX持有JVM识别的JMX属性

```text
SUNJMX="-Dcom.sun.management.jmxremote"
```

JVM中的JMX代理由com.sun.management控制。jmxremote属性，而ActiveMQ域是由代理配置文件中的useJmx属性控制的

**创建到MBean服务器的连接：**

```text
JMXServiceURL url = ...
JMXConnector connector = ...
MBeanServerConnection connection = connector.getMBeanServerConnection();
ObjectName name = new ObjectName("my-broker:BrokerName=localhost,
Type=Broker");
```

**查询代理MBean：**

```text
BrokerViewMBean mbean = (BrokerViewMBean)
MBeanServerInvocationHandler.newProxyInstance(connection, name,
BrokerViewMBean.class, true);
System.out.println("Statistics for broker " + mbean.getBrokerId() + " - " +
mbean.getBrokerName());
```

**MBean名字：**

```text
<jmx domain name>:BrokerName=<name of the broker>,Type=Broker
```

**ActiveMQ管理工具**

Bin/activemq：启动代理

Bin/activemq-admin：从命令行监控代理状态

命令代理:允许您使用纯JMS消息向代理发出管理命令。

JConsole:客户端应用程序，允许您浏览和调用公开的mbean的方法。

Web Console: [http://localhost:8161/admin](http://localhost:8161/admin)

代理日志：cf. Data/activemq.log

