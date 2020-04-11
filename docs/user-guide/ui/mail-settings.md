---
layout: docwithnav
assignees:
- ashvayka
title: 邮件设置
description: ThingsBoard IoT平台邮件设置

---

ThingsBoard系统管理员可以配置与SMTP服务器相关信息，该服务器将用于向用户发送激活和密码重置电子邮件。
在生产环境中需要此配置步骤。如果你正在学习了解本平台在大多数用例中预配置的[**演示帐户**](/docs/samples/demo-account/#demo-tenant)就足够了。
  
**注意**系统邮件设置仅在用户创建和密码重置过程中使用，并由系统管理员控制。
租户管理员可以[**设置电子邮件规则节点**](/docs/user-guide/rule-engine-2-0/tutorials/send-email/)以分发[**规则引擎**](/docs/user-guide/rule-engine-2-0/re-getting-started/)产生的警报。 

* TOC
{:toc}

需要执行以下步骤来配置系统邮件设置。

#### 步骤1.以系统管理员身份登录

使用默认[**帐户**](/docs/samples/demo-account/#system-administrator)作为系统管理员登录到ThingsBoard实例WEB UI。

#### 步骤2.更改管理员电子邮件地址 

右键单击WEB UI右上角的汉堡，然后选择配置文件。将“sysadmin@thingsboard.org” 更改为您的电子邮件地址。现在再次以管理员身份重新登录。

#### 步骤3.打开“外发邮件”并填充SMTP服务器设置

导航至 **System Settings -> Outgoing Mail** 然后填写表格。点击“发送测试电子邮件”按钮。测试电子邮件将发送到您在“步骤2”中指定的电子邮件地址。如果配置错误，您应该会收到一个带有错误日志的弹出窗口。

##### 步骤3.1 Sendgrid配置示例

SendGrid配置非常简单明了。首先，您需要创建[SendGrid](https://sendgrid.com/)帐户。

创建帐户后，您将被转到欢迎页面。现在，您可以配置SMTP中继凭据。请参见下面的屏幕截图。

{:refdef: style="text-align: center;"}
![image](/images/user-guide/ui/mail/sendgrid-welcome.png)
{: refdef}

请在下一页选择SMTP中继。

{:refdef: style="text-align: center;"}
![image](/images/user-guide/ui/mail/sendgrid-smtp-relay.png)
{: refdef}

填充并生成API密钥名称后，您就可以将设置从屏幕复制粘贴到ThingsBoard邮件设置表单中。

{:refdef: style="text-align: center;"}
![image](/images/user-guide/ui/mail/sendgrid-token.png)
{: refdef}

复制并粘贴设置，更新“Mail From”字段，然后单击“Send Test Mail”按钮。

{:refdef: style="text-align: center;"}
![image](/images/user-guide/ui/mail/sendgrid-settings.png)
{: refdef}

收到有关成功测试的通知后，保存填充的数据。您也可以在SendGrid网站上完成验证。

{:refdef: style="text-align: center;"}
![image](/images/user-guide/ui/mail/sendgrid-it-works.png)
{: refdef}

##### 步骤3.2 Gmail配置示例

为了使用G-mail，你将需要执行两个额外的步骤。首先，你需要 [**允许安全性较低的应用程序**](https://support.google.com/accounts/answer/6010255?hl=en)。其次，您需要启用两步验证并生成[**应用密码**](https://support.google.com/accounts/answer/185833?hl=en)。尽管第二步不是强制性的，但强烈建议您这样做。

{:refdef: style="text-align: center;"}
![image](/images/user-guide/ui/mail/app-password.png)
{: refdef}

准备就绪后，您应该可以使用以下信息设置Gmail帐户

{:refdef: style="text-align: center;"}
![image](/images/user-guide/ui/mail/gmail-settings.png)
{: refdef}

类似的设置可用于G-suite帐户，但是，您可能需要与系统管理员联系以启用安全性较低的应用程序等。请注意，您还可以使用复选框启用/禁用TLS。

{:refdef: style="text-align: center;"}
![image](/images/user-guide/ui/mail/gsuite-settings.png)
{: refdef}


#### 步骤4.保存配置

收到测试电子邮件后，您可以保存SMTP服务器配置。
