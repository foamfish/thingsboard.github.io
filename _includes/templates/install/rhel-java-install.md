ThingsBoard服务正在Java 8上运行。请按照以下说明安装OpenJDK 8：

```bash
sudo yum install java-1.8.0-openjdk
```

请不要忘记将操作系统配置为默认使用OpenJDK 8。

您可以使用以下命令配置哪个版本是默认版本：

```bash
sudo update-alternatives --config java
```

您可以使用以下命令检查安装：

```bash
java -version
```

命令输出结果：

```text
openjdk version "1.8.0_xxx"
OpenJDK Runtime Environment (...)
OpenJDK 64-Bit Server VM (build ...)
```