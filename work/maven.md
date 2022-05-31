# maven相关

本地install

```bash
mvn install:install-file -Dfile=/execute-agent/target/execute-agent-1.0-SNAPSHOT.jar -DgroupId=com.ry -DartifactId=eureka -Dpackaging=jar -Dversion=1.0.0-test4
```

pom.xml 引用：

```bash
<dependency>
    <groupId>com.ry</groupId>
    <artifactId>eureka</artifactId>
    <version>1.0.0-test4</version>
    <scope>provided</scope>
    <type>jar</type>
</dependency>

```
