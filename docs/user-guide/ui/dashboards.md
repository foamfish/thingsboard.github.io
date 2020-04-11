---
layout: docwithnav
assignees:
- ashvayka
title: 面板
description: ThingsBoard IoT 面板

---

* TOC
{:toc}

## 视频教程

<div id="video">  
    <div id="video_wrapper">
        <iframe src="https://www.youtube.com/embed/L_geyNzS7tM" frameborder="0" allowfullscreen></iframe>
    </div>
</div>

## 用户默认的IoT仪表板

从ThingsBoard 1.2开始，你现在可以通过两个简单步骤为用户定义默认的IoT仪表板：

#### 步骤1.将IoT仪表板分配给客户

请参阅上面的嵌入式视频教程，以获取有关如何执行此操作的提示。

#### 步骤2.打开客户用户详细信息

导航至 "**Customers** -> 你的客户 -> **Customer Users**" 然后使用屏幕右上角的“铅笔”按钮切换编辑模式。

#### 步骤3.选择IoT仪表板

从列表中选择IoT仪表板并应用更改。请注意，你还可以检查“始终全屏”模式，以防止用户导航到其他仪表板/屏幕。

![image](/images/user-guide/ui/default-dashboard.png)

## IoT仪表板导入/导出

#### 仪表板导出

你可以将仪表板导出为JSON格式，然后将其导入相同或另一个ThingsBoard实例。

为了导出仪表盘，您应该导航到**Dashboards**页面，然后单击位于特定仪表盘卡上的导出按钮。
 
![image](/images/user-guide/ui/export-dashboard.png)

#### 仪表板导入

同样，要导入仪表板，您应该导航至**Dashboards**页面，然后单击屏幕右下角的大“ +”按钮，然后单击导入按钮。

![image](/images/user-guide/ui/import-dashboard.png)

仪表板导入窗口应弹出，并提示您上传json文件。

![image](/images/user-guide/ui/import-dashboard-window.png)

单击“导入”按钮后，您将需要指定设备别名。基本上这使您可以设置与仪表板别名对应的设备。

![image](/images/user-guide/ui/import-dashboard-aliases.png)

