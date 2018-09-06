# 6、ActiveMQ安全

安全代理最简单的方式是通过直接在代理的XML配置文件中使用身份验证凭证。

```text
<plugins>
    <simpleAuthenticationPlugin>
    <users>
        <authenticationUser username="admin" password="password"
            groups="admins,publishers,consumers"/>
        <authenticationUser username="publisher" password="password"
            groups="publishers,consumers"/>
        <authenticationUser username="consumer" password="password"
            groups="consumers"/>
        <authenticationUser username="guest" password="password"
            groups="guests"/>
    </users>
    </simpleAuthenticationPlugin>
</plugins>
```

提供用户名/密码

```text
factory = new ActiveMQConnectionFactory(brokerURL);
connection = factory.createConnection(username, password);
```

JAAS\(  Java Authentication Authorization Service,Java验证和授权API\)插件

JAAS插件提供的功能与简单身份认证插件相同，但使用了标准的java机制

```text
<plugins>
    <jaasAuthenticationPlugin configuration="activemq-domain" />
</plugins>
```

**授权**

ActiveMQ提供两个级别的授权：目的地/操作级别和消息级别

有三种具有JMS目的地的用户级操作:读/写/管理（read/write/admin）

```text
<authorizationEntry topic="STOCKS.>" read="consumers" write="publishers"
admin="publishers" />
```

消息级别的授权控制对目标中的特定消息的访问。使用MessageAuthorizationPolicy类中的isAllowedToConsume\(\)方法。

SSL证书

证书可以用来避免存储使用普通的用户名和密码放入凭据。



