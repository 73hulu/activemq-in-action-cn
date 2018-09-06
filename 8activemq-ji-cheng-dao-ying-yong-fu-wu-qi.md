# 8、ActiveMQ集成到应用服务器

应用程序服务器提供容器体系结构，它接受应用程序的部署和提供应用程序运行的环境。第一种类型实现Java Servlet规范，web容器如：Tomcat, Jetty。第二种类型Java EE规范，J2EE容器：Geronimo, JBoss, WebLogic, WebSphere。

spring可以用来启动ActiveMQ和提供对JMS 目的地的访问，也可以使用JMS的连接创建代理的实例。

java提供一个JNDI的实现，，它暴露了部署到容器中的应用程序使用的对象。本地JNDI用来配置将公开给特定应用程序的对象，而全局JNDI适用于整个web容器中的任何应用程序。

web.xml:

```text
<resource-ref>
    <res-ref-name>jms/ConnectionFactory</res-ref-name>
    <res-type>org.apache.activemq.ActiveMQConnectionFactory</res-type>
    <res-auth>Container</res-auth>
</resource-ref>
<resource-ref>
    <res-ref-name>jms/FooQueue</res-ref-name>
    <res-type>javax.jms.Queue</res-type>
    <res-auth>Container</res-auth>
</resource-ref>
```

resource-ref元素引用注册在AS中的JNDI资源。这使web应用程序可以使用这些资源。

**Spring application context:**

```text
<jee:jndi-lookup id="connectionFactory"
jndi-name="java:comp/env/jms/ConnectionFactory"
cache="true"
resource-ref="true"
lookup-on-startup="true"
expected-type="org.apache.activemq.ActiveMQConnectionFactory"
proxy-interface="javax.jms.ConnectionFactory" /
```

jndi-lookup元素利用spring来执行指定资源的JNDI查找。资源被注入到messageSenderService Bean中

```text
<bean id="messageSenderService"
class="org....book.ch8.jms.service.JmsMessageSenderService"
p:jmsTemplate-ref="jmsTemplate" />
```

这个bean被web应用程序用来发送JMS消息

结合Tomcat

JNDI资源被定义在一个名为META-INF/context.xml的文件中

```text
<Context reloadable="true">
    <Resource auth="Container"
    name="jms/ConnectionFactory"
    type="org.apache.activemq.ActiveMQConnectionFactory"
    description="JMS Connection Factory"
    factory="org.apache.activemq.jndi.JNDIReferenceFactory"
    brokerURL="vm://localhost?brokerConfig=xbean:activemq.xml"
    brokerName="MyActiveMQBroker"/>
    <Resource auth="Container"
    name="jms/FooQueue"
    type="org.apache.activemq.command.ActiveMQQueue"
    description="JMS queue"
    factory="org.apache.activemq.jndi.JNDIReferenceFactory"
    physicalName="FOO.QUEUE"/>
</Context>
```

如果资源已经被定义在Context元素里，它就没有必要将这些资源定义咋、WEB-INF/web。xml。然而，建议将资源保存在WEB-INF/web.xml，用来记录web应用程序的资源需求。

