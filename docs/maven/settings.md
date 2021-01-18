# settings.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <!-- 本地存储库路径 -->
  <localRepository>/***/repo/p2</localRepository>

  <!-- 是否与用户输入进行交互,输入将提示 -->
  <interactiveMode>true</interactiveMode>

  <!-- 执行build时是否尝试联网 -->
  <offline>false</offline>

  <pluginGroups>
    <!-- 插件列表, 默认包含org.apache.maven.plugins 和 org.codehaus.mojo, 其他插件需要配置, 如jetty -->
    <!-- <pluginGroup>org.eclipse.jetty</pluginGroup> -->
  </pluginGroups>

  <!-- 如果不能直接访问外网中央服务器, 可以配置代理 -->
  <proxies>
  <!-- proxy
     | Specification for one proxy, to be used in connecting to the network.
     |
    <proxy>
      <id>optional</id>
      <active>true</active>
      <protocol>http</protocol>
      <username>proxyuser</username>
      <password>proxypass</password>
      <host>proxy.host.net</host>
      <port>80</port>
      <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
    </proxy>
    -->
  </proxies>

  <!-- 私服用户名密码 -->
  <servers>
    <server>
      <id>releases</id>
      <username>***</username>
      <password>***</password>
    </server>
    <server>
      <id>snapshots</id>
      <username>***</username>
      <password>***</password>
    </server>
  </servers>

  <!-- 阿里云镜像 -->
  <mirrors>
    <mirror>
      <id>alimaven</id>
      <!-- 如果此处配置为*, 则所有远程仓库都走此处, 如果配置为central,则只有请求中央仓库时才走此处, 
      优先级: 本地仓库> settings profile > pom profile > mirrorOf -->
      <mirrorOf>central</mirrorOf>
      <name>ali repository</name>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
  </mirrors>

  <!-- 仓库配置 -->
  <profiles>
    <profile>
        <id>local_nexus_profile</id>
        <!-- jdk版本可忽略,在项目里定制配置 -->
        <!-- <properties>
          <maven.compiler.source>1.8</maven.compiler.source>
          <maven.compiler.target>1.8</maven.compiler.target>
          <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
        </properties> -->
        <repositories>
            <repository>
                <id>local_nexus</id>
                <name>company</name>
                <url>http://***/</url>
                <releases>
                    <enabled>true</enabled>
                </releases>
                <snapshots>
                    <enabled>true</enabled>
                    <updatePolicy>always</updatePolicy>
                </snapshots>
            </repository>
        </repositories>
        <pluginRepositories>
          <pluginRepository>
            <id>local_nexus</id>
            <name>company</name>
            <url>http://***</url>
            <releases>
              <enabled>true</enabled>
            </releases>
            <snapshots>
              <enabled>true</enabled>
              <updatePolicy>always</updatePolicy>
            </snapshots>
          </pluginRepository>
			  </pluginRepositories>
    </profile>
  </profiles>

  <!-- 激活的配置 -->
  <activeProfiles>
    <activeProfile>local_nexus_profile</activeProfile>
  </activeProfiles>
</settings>
```