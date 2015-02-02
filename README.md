## logback-syslog4j

A [Logback][] appender that leverages [syslog4j][] to send log messages to
remote systems via syslog.

### Why?

The [existing syslog appender for Logback][logback-syslog-appender] only
provides the ability to send messages via UDP. Using syslog4j allows us to
send messages via TCP and optionally to encrypt them by sending over TCP with
TLS.


### How?

Add this to your `pom.xml`:

``` xml
    <dependency>
      <groupId>com.papertrailapp</groupId>
      <artifactId>logback-syslog4j</artifactId>
      <version>1.0.0</version>
    </dependency>
```

Then add the appender to your `logback.xml`.


#### Logging via TCP with TLS

``` xml
  <appender name="SYSLOG-TLS" class="com.papertrailapp.logback.Syslog4jAppender">
    <layout class="ch.qos.logback.classic.PatternLayout">
      <pattern>%-5level %logger{35}: %m%n%xEx</pattern>
    </layout>

    <syslogConfig class="org.productivity.java.syslog4j.impl.net.tcp.ssl.SSLTCPNetSyslogConfig">
      <!-- remote system to log to -->
      <host>localhost</host>
      <!-- remote port to log to -->
      <port>514</port>
      <!-- program name to log as -->
      <ident>java-app</ident>
    </syslogConfig>
  </appender>

  <root level="DEBUG">
    <appender-ref ref="SYSLOG-TLS" />
  </root>
```

#### Logging via TCP

``` xml
  <appender name="SYSLOG-TCP" class="com.papertrailapp.logback.Syslog4jAppender">
    <layout class="ch.qos.logback.classic.PatternLayout">
      <pattern>%-5level %logger{35}: %m%n%xEx</pattern>
    </layout>

    <syslogConfig class="org.productivity.java.syslog4j.impl.net.tcp.TCPNetSyslogConfig">
      <!-- remote system to log to -->
      <host>localhost</host>
      <!-- remote port to log to -->
      <port>514</port>
      <!-- program name to log as -->
      <ident>java-app</ident>
    </syslogConfig>
  </appender>

  <root level="DEBUG">
    <appender-ref ref="SYSLOG-TCP" />
  </root>
```

#### Logging via UDP

``` xml
  <appender name="SYSLOG-UDP" class="com.papertrailapp.logback.Syslog4jAppender">
    <layout class="ch.qos.logback.classic.PatternLayout">
      <pattern>%-5level %logger{35}: %m%n%xEx</pattern>
    </layout>

    <syslogConfig class="org.productivity.java.syslog4j.impl.net.udp.UDPNetSyslogConfig">
      <!-- remote system to log to -->
      <host>localhost</host>
      <!-- remote port to log to -->
      <port>514</port>
      <!-- program name to log as -->
      <ident>java-app</ident>
    </syslogConfig>
  </appender>

  <root level="DEBUG">
    <appender-ref ref="SYSLOG-UDP" />
  </root>
```


[Logback]: http://logback.qos.ch/
[syslog4j]: http://syslog4j.org/
[logback-syslog-appender]: http://logback.qos.ch/manual/appenders.html#SyslogAppender