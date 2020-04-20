以管理员用户身份打开记事本或其他编辑器（右键单击应用程序图标，然后选择“以管理员身份运行”）。

打开以下文件进行编辑（在文件选择对话框中选择“所有文件”而不是“文本文档”，编码为UTF-8）：

```text 
C:\Program Files (x86)\thingsboard\thingsboard.xml
``` 
{: .copy-code}


找到"SQL DAO Configuration"代码块将postgres用户密码替换"postgres"：

```xml
    <startargument>-Xms512m</startargument>
    <startargument>-Xmx1024m</startargument>
```
{: .copy-code}

并将它们更改为

```xml
    <startargument>-Xms256m</startargument>
    <startargument>-Xmx256m</startargument>
```
{: .copy-code}