- Wrapper当前DataSource如果是代理实现，提供获取目标实例的能力。unwrap获取目标实例，isWrapperFor判断能否获取指定Class的目标实例。
```java
public interface Wrapper {
    <T> T unwrap(java.lang.Class<T> iface) throws java.sql.SQLException;
    boolean isWrapperFor(java.lang.Class<?> iface) throws java.sql.SQLException;
}

```


- CommonDataSource：获取输出日志的Logger/PrintWriter，设置登录数据库超时时长。
```java
public interface CommonDataSource {
    java.io.PrintWriter getLogWriter() throws SQLException;
    void setLogWriter(java.io.PrintWriter out) throws SQLException;
    void setLoginTimeout(int seconds) throws SQLException;
    int getLoginTimeout() throws SQLException;
    public Logger getParentLogger() throws SQLFeatureNotSupportedException;
}

```


- DataSource：获取Connection。
```java
public interface DataSource  extends CommonDataSource, Wrapper {
  Connection getConnection() throws SQLException;
  Connection getConnection(String username, String password)
    throws SQLException;
}

```


- HikariConfig：HikariConfig保存了所有连接池配置，另外实现了HikariConfigMXBean接口，有些配置可以利用JMX运行时变更

- HikariDataSource：操作HikariPool获取连接。可以看到HikariDataSource有两个HikariPool的成员变量。
    - fastPathPool：final修饰，构造时决定。如果使用无参构造为null，使用有参构造和pool一样。
    - pool：volatile修饰。无参构造不会设置pool，在getConnection时构造pool，有参构造和fastPathPool一样。


- PoolBase：PoolBase是HikariPool的父类，主要负责操作实际的DataSource获取Connection，并设置Connection的一些属性

- DriverDataSource:实现了DataSource接口，实际操作java.sql.Driver获取Connection

- HikariPool：主要的连接池类，为HikariCP提供基本的池化行为

- ConcurrentBag：是HikariCP为连接池设计的一个并发类

- PoolEntry:PoolEntry实际拥有Connection的实例，并且实现IConcurrentBagEntry接口，可以放入ConcurrentBag容器。

