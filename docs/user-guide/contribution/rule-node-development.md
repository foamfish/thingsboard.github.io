---
layout: docwithnav
title: 规则节点开发指南
description: 创建自定义规则节点

---

* TOC
{:toc}

## 概述

在本教程中您将学习如何创建自定义节点：

 - [**过滤节点**](https://github.com/thingsboard/rule-node-examples/blob/master/src/main/java/org/thingsboard/rule/engine/node/filter/TbKeyFilterNode.java)检查入站消息的遥测数据中是否存在**key**。如果所选密钥存在-通过**True**链发送消息，否则使用**False**链。
 
 ![image](/images/user-guide/contribution/customization/check-key-node.png)
 
 ![image](/images/user-guide/contribution/customization/add-check-key-node.png)
 
 - [**富集节点**](https://github.com/thingsboard/rule-node-examples/blob/master/src/main/java/org/thingsboard/rule/engine/node/enrichment/TbGetSumIntoMetadata.java)当字段以指定的**Input Key**开头时，将为每个设备分别计算遥测数据的总和，然后使用**Output Key**将结果添加到消息元数据中。
 
 ![image](/images/user-guide/contribution/customization/get-sum-in-metadata-node.png)
 
 ![image](/images/user-guide/contribution/customization/add-get-sum-into-matadata-node.png)
 
 - [**转换节点**](https://github.com/thingsboard/rule-node-examples/blob/master/src/main/java/org/thingsboard/rule/engine/node/transform/TbCalculateSumNode.java)当字段以指定的**Input Key**开头时，将为每个设备分别计算遥测数据的总和，然后使用**Output Key**将总和添加到新的消息payload中。
 
 ![image](/images/user-guide/contribution/customization/calculate-sum-node.png)
 
 ![image](/images/user-guide/contribution/customization/add-calculate-sum-node.png)


## 先决条件

我们假设您已完成以下指南并查看了以下文章：

  * [入门](/docs/getting-started-guides/helloworld/) guide.
  * [规则引擎概述](/docs/user-guide/rule-engine-2-0/overview/) article.
  * [规则引擎架构](/docs/user-guide/rule-engine-2-0/architecture/) article.

## 定制

为了创建新的规则节点，您应该实现[TbNode](https://github.com/thingsboard/thingsboard/blob/release-2.0/rule-engine/rule-engine-api/src/main/java/org/thingsboard/rule/engine/api/TbNode.java) 接口:

```java
package org.thingsboard.rule.engine.api;

...

public interface TbNode {

    void init(TbContext ctx, TbNodeConfiguration configuration) throws TbNodeException;

    void onMsg(TbContext ctx, TbMsg msg) throws ExecutionException, InterruptedException, TbNodeException;

    void destroy();

}
```

并使用以下引用运行时的[多值注释](https://github.com/thingsboard/thingsboard/blob/release-2.0/rule-engine/rule-engine-api/src/main/java/org/thingsboard/rule/engine/api/RuleNode.java)来注释您的实现：

```java
org.thingsboard.rule.engine.api.RuleNode 
```

每个规则节点也可以具有实现[NodeConfiguration](https://github.com/thingsboard/thingsboard/blob/release-2.0/rule-engine/rule-engine-api/src/main/java/org/thingsboard/rule/engine/api/NodeConfiguration.java)接口的配置类。

```java
package org.thingsboard.rule.engine.api;

public interface NodeConfiguration<T extends NodeConfiguration> {

    T defaultConfiguration();

}
```
- [TbKeyFilterNodeConfiguration](https://github.com/thingsboard/rule-node-examples/blob/master/src/main/java/org/thingsboard/rule/engine/node/filter/TbKeyFilterNodeConfiguration.java) 类:

{% highlight java %}
    package org.thingsboard.rule.engine.node.filter;
    
    import lombok.Data;
    import org.thingsboard.rule.engine.api.NodeConfiguration;
    
    @Data
    public class TbKeyFilterNodeConfiguration implements NodeConfiguration<TbKeyFilterNodeConfiguration> {
    
        private String key;
    
        @Override
        public TbKeyFilterNodeConfiguration defaultConfiguration() {
            TbKeyFilterNodeConfiguration configuration = new TbKeyFilterNodeConfiguration();
            configuration.setKey(null);
            return configuration;
        }
    }{% endhighlight %}
    
- [TbCalculateSumNodeConfiguration](https://github.com/thingsboard/rule-node-examples/blob/master/src/main/java/org/thingsboard/rule/engine/node/transform/TbCalculateSumNodeConfiguration.java) 类:
{% highlight java %}
    package org.thingsboard.rule.engine.node.transform;
    
    import lombok.Data;
    import org.thingsboard.rule.engine.api.NodeConfiguration;
    
    
    @Data
    public class TbCalculateSumNodeConfiguration implements NodeConfiguration<TbCalculateSumNodeConfiguration> {
    
        private String inputKey;
        private String outputKey;
    
        @Override
        public TbCalculateSumNodeConfiguration defaultConfiguration() {
            TbCalculateSumNodeConfiguration configuration = new TbCalculateSumNodeConfiguration();
            configuration.setInputKey("temperature");
            configuration.setOutputKey("TemperatureSum");
            return configuration;
        }
    }{% endhighlight %}    

- [TbGetSumIntoMetadataConfiguration](https://github.com/thingsboard/rule-node-examples/blob/master/src/main/java/org/thingsboard/rule/engine/node/enrichment/TbGetSumIntoMetadataConfiguration.java) 类:
{% highlight java %}
    package org.thingsboard.rule.engine.node.enrichment;
    
    import lombok.Data;
    import org.thingsboard.rule.engine.api.NodeConfiguration;
    
    @Data
    public class TbGetSumIntoMetadataConfiguration implements NodeConfiguration<TbGetSumIntoMetadataConfiguration> {
    
        private String inputKey;
        private String outputKey;
    
    
        @Override
        public TbGetSumIntoMetadataConfiguration defaultConfiguration() {
            TbGetSumIntoMetadataConfiguration configuration = new TbGetSumIntoMetadataConfiguration();
            configuration.setInputKey("temperature");
            configuration.setOutputKey("TemperatureSum");
            return configuration;
        }
    }{% endhighlight %}        


配置类在规则节点类中定义。

### 方法流程

在本节中介绍**TbNode**接口实现每个方法的目的：

#### init方法

{% highlight java %}void init(TbContext ctx, TbNodeConfiguration configuration) throws TbNodeException;{% endhighlight %}
 
创建规则节点后对其进行初始化的方法。上述每个规则节点的**init**方法的内容全部一样。我们将传入的JSON配置转换为特定的[NodeConfiguration](https://github.com/thingsboard/thingsboard/blob/release-2.0/rule-engine/rule-engine-api/src/main/java/org/thingsboard/rule/engine/api/NodeConfiguration.java)实现。

 - [**检查key**](https://github.com/thingsboard/rule-node-examples/blob/master/src/main/java/org/thingsboard/rule/engine/node/filter/TbKeyFilterNode.java):
 
{% highlight java %}
    private TbKeyFilterNodeConfiguration config;
    private String key;
            
    @Override
    public void init(TbContext tbContext, TbNodeConfiguration configuration) throws TbNodeException {
        this.config = TbNodeUtils.convert(configuration, TbKeyFilterNodeConfiguration.class);
        key = config.getKey();
    }{% endhighlight %}
    
 - [**求和计算**](https://github.com/thingsboard/rule-node-examples/blob/master/src/main/java/org/thingsboard/rule/engine/node/transform/TbCalculateSumNode.java):
 
{% highlight java %}
    private TbCalculateSumConfiguration config;
    private String inputKey;
    private String outputKey;
    
    @Override
    public void init(TbContext ctx, TbNodeConfiguration configuration) throws TbNodeException {
        this.config = TbNodeUtils.convert(configuration, TbCalculateSumConfiguration.class);
        inputKey = config.getInputKey();
        outputKey = config.getOutputKey();
    }{% endhighlight %}    

 - [**求和放入元数据**](https://github.com/thingsboard/rule-node-examples/blob/master/src/main/java/org/thingsboard/rule/engine/node/enrichment/TbGetSumIntoMetadata.java):
 
{% highlight java %}
    private TbGetSumIntoMetadataConfiguration config;
    private String inputKey;
    private String outputKey;
    
    @Override
    public void init(TbContext ctx, TbNodeConfiguration configuration) throws TbNodeException {
        this.config = TbNodeUtils.convert(configuration, TbGetSumIntoMetadataConfiguration.class);
        inputKey = config.getInputKey();
        outputKey = config.getOutputKey();
    }{% endhighlight %}    
仅在创建或更新规则节点后才调用它，并接受两个输入参数：
 
 - [**TbContext**](https://github.com/thingsboard/thingsboard/blob/release-2.0/rule-engine/rule-engine-api/src/main/java/org/thingsboard/rule/engine/api/TbContext.java)是一个接口，可让规则节点访问大多数服务，例如，将遥测保存到数据库，并通过WebSockets通知实体数据更改的所有订阅：
    
   {% highlight java %}ctx.getTelemetryService().saveAndNotify(msg.getOriginator(), tsKvEntryList, ttl, new TelemetryNodeCallback(ctx, msg));{% endhighlight %}
               
 - [**TbNodeConfiguration**](https://github.com/thingsboard/thingsboard/blob/release-2.0/rule-engine/rule-engine-api/src/main/java/org/thingsboard/rule/engine/api/TbNodeConfiguration.java) TbNodeConfiguration是一个简单的类，只有一个在规则节点Web UI上处理的字段：{% highlight java %}private final JsonNode data; {% endhighlight %}

#### onMsg方法

{% highlight java %}void onMsg(TbContext ctx, TbMsg msg) throws ExecutionException, InterruptedException, TbNodeException;{% endhighlight %}
 
处理接收到消息的方法。每当消息到达节点时都会调用它。并且还接受两个输入参数：

- [**TbMsg**](https://github.com/thingsboard/thingsboard/blob/release-2.0/common/message/src/main/java/org/thingsboard/server/common/msg/TbMsg.java)是最终的序列化类，它使您可以访问消息中的字段，并且还允许您复制消息，将消息转换为ByteBuffer以及进行其他操作。
        
   可用字段: 
   
     {% highlight java %}
          msg.getData();
          msg.getMetaData();
          msg.getOriginator();
          msg.getType();
          msg.getId();
          msg.getRuleNodeId();
          msg.getRuleChainId();
          msg.getClusterPartition();
          msg.getDataType();{% endhighlight %}
     
   复制消息: 
    {% highlight java %}
        TbMsg copy = msg.copy(UUIDs.timeBased(), entityId, targetId, DEFAULT_CLUSTER_PARTITION);
        
    **×注意**×在群集模式下使用。 {% endhighlight %} 
 
- [**TbContext**](https://github.com/thingsboard/thingsboard/blob/release-2.0/rule-engine/rule-engine-api/src/main/java/org/thingsboard/rule/engine/api/TbContext.java)接口还具有在链上路由出站消息的方法，例如

![image](/images/user-guide/contribution/customization/check-key-config.png)  ![image](/images/user-guide/contribution/customization/relations.png)  

true & false: 
    {% highlight java %}
        ctx.tellNext(msg, mapper.readTree(msg.getData()).has(key) ? "True" : "False");{% endhighlight %}  
          
failure:
    {% highlight java %}
        ctx.tellFailure(msg, e);
        
        ctx.tellNext(msg, FAILURE, new Exception());{% endhighlight %}
  
tellSelf和updateSelf方法:
    {% highlight java %}
        ctx.tellSelf(tickMsg, curDelay);
        
        ctx.updateSelf(ruleNode);{% endhighlight %}
        
        
{% highlight java %}***注意***tellSelf()方法在基于特定延迟的规则节点中使用。每个更新规则节点上使用的updateSelf()方法。{% endhighlight %}
      
        
此处, [**TbContext**](https://github.com/thingsboard/thingsboard/blob/release-2.0/rule-engine/rule-engine-api/src/main/java/org/thingsboard/rule/engine/api/TbContext.java)允许创建新消息： 
     {% highlight java %}
        String data = "{temperature:20, humidity:30}"
     
        ctx.newMsg(msg.getType(), msg.getOriginator(), msg.getMetaData(), data);{% endhighlight %}
      
并转换消息:     
     {% highlight java %}
     ctx.transformMsg(origMsg, origMsg.getType(), origMsg.getOriginator(), metaData, origMsg.getData());{% endhighlight %} 

{% highlight java %}
***注意***这些方法之间的区别在于：
    
       TbMsg的newMsg方法使用新的messageId创建一条新消息；
    
       TbMsg的transformMsg方法转换已存在的消息；{% endhighlight %}   

#### destroy方法
           
{% highlight java %}void destroy();{% endhighlight %}  

仅在规则节点停止或更新并且没有输入参数之后才调用的此方法。

### 构建

 - 克隆规则节点示例项目：
  
```
git clone git@github.com:thingsboard/rule-node-examples.git
```

 - 从rule-node-examples文件夹执行以下命令以构建项目:
 
    - **注意** 您需要先从Thingsboard文件夹执行此命令 。
 
```
mvn clean install
``` 

### 将可执行的jar文件导入到ThingsBoard实例中

将jar文件作为依赖库导入到Thingsboard项目中，应该在这里:
 
```
./target/rule-engine-1.0.0-custom-nodes.jar
```

#### 使用IDE的Thingsboard:

 - 请参阅[IDEA](https://www.jetbrains.com/help/idea/library.html#add-library-to-module-dependencies)和[Eclipse](https://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jst.j2ee.doc.user%2Ftopics%2Ftjimpapp.html)相关说明.
 
重新启动ThingsBoard服务器端容器。请参考以下链接以查看如何执行相关操作: [服务器端容器运行](/docs/user-guide/contribution/how-to-contribute/#running-server-side-container). 
 
```
 **一旦ThingsBoard重新启动，您需要清除浏览器缓存并刷新网页以重新加载规则节点的用户界面**
``` 
 
#### Thingsboard服务:
 
 - 您需要先执行以下命令将jar文件移动到Thingsboard扩展:
   
```
sudo mv rule-engine-1.0.0-custom-nodes.jar /usr/share/thingsboard/extensions/
```

 - 接下来执行以下操作以将权限更改为Thingsboard:

```
sudo chown thingsboard:thingsboard /usr/share/thingsboard/extensions/*
```

重启Thingsboard服务:

```
sudo service thingsboard restart

**一旦ThingsBoard重新启动，您需要清除浏览器缓存并刷新网页以重新加载规则节点的用户界面**
```
  
### UI configuration

ThingsBoard规则节点UI在官方[github仓库](https://github.com/thingsboard/rule-node-examples-ui)中配置了另一个项目。请参阅以下[连接](https://github.com/thingsboard/thingsboard-rule-config-ui#thingsboard-rule-config-ui)查看相关说明。

#### 在热部署模式下运行Rule Node UI容器

要以热重新部署方式运行Rule Node UI容器：

 - 您需要先在**server.js**文件中将常量**ruleNodeUiforwardPort**从8080更改为5000，应在此处：
    
```
cd ${TB_WORK_DIR}/ui/server.js
```
     
 - 其次您需要在热部署模式下运行UI容器。请参考以下链接以了解如何执行此操作：[以热重新部署模式运行UI容器](/docs/user-guide/contribution/how-to-contribute/#running-ui-container-in-hot-redeploy-mode)。
 
 - 接下来您需要将**server.js**文件中的常量**forwardPort**从**8080**更改为**3000**，应在此处：
  
```
cd ${TB_RULE_NODE_UI_WORK_DIR}/ui/server.js
```
   
  - 最后一步是从本地目录**TB_RULE_NODE_UI_WORK_DIR**执行如下命令:
    
    ```
    npm start
    ```

这会将规则节点UI请求转发到在**3000**端口上侦听的服务器。

## 下一步
 
 {% assign currentGuide = "Contribution" %}{% include templates/guides-banner.md %}