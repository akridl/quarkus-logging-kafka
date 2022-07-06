# Introduction
quarkus-logging-kafka allows your Quarkus app to send logs to a Kafka server. Internally it uses the `KafkaLog4jAppender` to make this happen.

# Usage
Add the dependency in your Quarkus app's pom.xml:
```xml
<dependency>
    <groupId>org.jboss.pnc.logging</groupId>
    <artifactId>quarkus-logging-kafka</artifactId>
    <version>${quarkus-logging-kafka.version}</version>
</dependency>
<dependency>
    <groupId>org.jboss.pnc.logging</groupId>
    <artifactId>quarkus-logging-kafka-deployment</artifactId>
    <version>${quarkus-logging-kafka.version}</version>
</dependency>
```

You can find the latest version of quarkus-logging-kafka [here](https://repo1.maven.org/maven2/org/jboss/pnc/logging/quarkus-logging-kafka/).

In your application.yaml, you can activate the quarkus-logging-kafka as such:
```yaml
quarkus:
  log:
    handler:
      kafka:
        enabled: true
        broker-list: <kafka server>
        topic: <kafka topic>
        security-protocol: <security protocol>
        level: INFO
        # if you want to only send logs for some logger names only
        filter-logger-name-pattern: org.jboss.*
```

For a full list of configuration options, see Java file: `runtime/src/main/java/org/jboss/pnc/logging/kafka/KafkaLogConfig.java`
## Authentication
If you are authenticating using SSL, you should define those properties in your application.yaml:
```yaml
quarkus:
  log:
    handler:
      kafka:
        ...
        security-protocol: SSL
        ssl-truststore-location: <path>
        ssl-truststore-password: <password>
```

If you are authenticating using SASL\_SSL, then you should use this application.yaml:
```yaml
quarkus:
  log:
    handler:
      kafka:
        ...
        security-protocol: SASL_SSL
        sasl-mechanism: PLAIN
        sasl-jaas-conf: org.apache.kafka.common.security.plain.PlainLoginModule required username="<username>" password="<password>";

```

At this moment, the SASL mechanism `OAUTHBEARER` is not supported simply because `KafkaLog4jAppender` doesn't support it.

# Devel
Building the application can be done by running:
```bash
mvn clean install
```
