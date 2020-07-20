# jdk9

## jdk和jre的改变

### 区别
jdk: java开发工具包  
jre: java运行环境  
![jdk_jre](static/jdk.png)
JDK = JRE + 开发工具集（例如 Javac 编译工具等）  
JRE = JVM + Java SE 标准类库

### jdk8目录结构
```mermaid
graph TD;
  A-->B;
  A-->C;
  B-->D;
  C-->D;
```