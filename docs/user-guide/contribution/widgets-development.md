---
layout: docwithnav
assignees:
- ikulikov
title: 部件开发指南

---

* TOC
{:toc}

## 介绍

**ThingsBoard小部件**属于UI模块可以与任何[IoT仪表板](/docs/user-guide/ui/dashboards/)集成并提供最终功能给用户，例如数据可视化，远程设备控制，警报管理和显示静态自定义html内容。

根据提供的功能每个部件定义代表特定的[部件类型](/docs/user-guide/ui/widget-library/#widget-types)。

## 创建新的部件定义

为了创建新的部件定义请导航“部件库”。

然后打开现有的“部件捆绑”或创建一个新的。

在“部件捆绑”视图中，单击屏幕右下角的大“ +”按钮，然后单击“创建新的窗口小部件类型”按钮。

![image](/images/user-guide/contribution/widgets/create-new-widget-type.png)

**选择窗口部件类型**会弹出窗口会，提示您选择要开发的相应[部件类型](/docs/user-guide/ui/widget-library/#widget-types)。

![image](/images/user-guide/contribution/widgets/select-widget-type.png)

之后，将根据先前选择的窗口小部件类型打开“窗口部件编辑器”页面，该页面预填充了启动器窗口部件模板。

![image](/images/user-guide/contribution/widgets/widget-editor.png)

### 部件编辑器概述

它由[工具栏](#widget-editor-toolbar)和四个主要部分组成：

 - [HTML和CSS资源](#resourceshtmlcss-section)    
 - [JavaScript](#javascript-section) 
 - [设置](#settings-schema-section) 
 - [部件介绍](#widget-preview-section)

#### 部件工具栏编辑

![image](/images/user-guide/contribution/widgets/widget-editor-toolbar.png)

窗口部件编辑器工具栏包含以下各项：

 - **部件标题**字段 - 用于指定小部件定义的标题
 - **部件类型**选择器 - 用于指定部件定义的[类型](/docs/user-guide/ui/widget-library/#widget-types)
 - **运行**按钮 - 用于运行部件代码并在“部件预览”部分中查看结果
 - **撤消** 按钮 - 将所有编辑器部分恢复为最新保存状态
 - **保存**按钮 - 保存部件定义
 - **另存为**按钮 - 通过指定新的部件类型名称和目标**部件捆绑**可以保存新的部件定义副本
 
#### HTML和CSS资源部分

本节包含三个选项卡。第一个**资源**选项卡用于指定窗口部件使用的外部JavaScript/CSS资源。

![image](/images/user-guide/contribution/widgets/widget-editor-resources.png)

第二个**HTML**选项卡包含部件html代码（注意：某些部件会动态创建html内容，而初始html内容可以为空）。

![image](/images/user-guide/contribution/widgets/widget-editor-html.png)

第三个**CSS**选项卡包含特定于部件的CSS样式定义。

![image](/images/user-guide/contribution/widgets/widget-editor-css.png)

#### JavaScript部分

本节包含根据[部件API](#basic-widget-api)的所有与窗口部件相关的JavaScript代码。

![image](/images/user-guide/contribution/widgets/widget-editor-javascript.png)

#### 设置部分

本节包含两个选项卡。

第一个**设置模式**标签用于指定部件设置的json模式，以便使用react-schema-form [捆绑](http://networknt.github.io/react-schema-form/)。

在生成的UI表单上显示部件设置的**高级**选项卡。

设置序列化对象存储部件然后从部件Javascript代码访问。

![image](/images/user-guide/contribution/widgets/widget-editor-settings-schema.png)
 
第二个**数据键设置**选项卡用于指定数据键为json模式，以便使用react-schema-form [捆绑](http://networknt.github.io/react-schema-form/)。

在生成的UI表单上显示部件设置的**数据键配置**选项卡。

设置序列化对象存储部件数据源的特定数据键。

这些设置可从小部件JavaScript代码访问。

![image](/images/user-guide/contribution/widgets/widget-editor-datakey-settings-schema.png)

#### 部件预览部分

本部分用于预览和测试窗口小部件定义。

它以迷你仪表板的形式显示，其中包含一个从当前窗口小部件定义实例化的窗口小部件。

它具有通常的ThingsBoard仪表板提供的几乎所有功能，但有一些限制。

例如，出于调试目的，在窗口小部件数据源部分中只能选择“功能”作为数据源类型。

![image](/images/user-guide/contribution/widgets/widget-editor-preview.png)

### 基础小部件API

所有与部件相关的代码都位于[JavaScript部分](#javascript-section)。
还提供了对部件实例的引用的内置变量**self**。
每个部件函数都应定义为**self**变量的属性。
**self**变量具有**ctx**属性-引用具有小部件实例使用的所有必要API和数据的小部件上下文。
以下是窗口部件上下文属性的简要说明：
 
| **属性**                     | **类型**           |  **描述**                                                |
|----------------------------------|--------------------|-----------------------------------------------------------------|
| $container                       | jQuery Object      | 部件的容器元素。可用于使用jQuery API动态访问或修改部件DOM。 |
| $scope                           | Object             | 当前部件元素的角度范围对象。使用Angular方法构建窗口小部件时，可用于访问/修改范围属性。 |
| width                            | Number             | 部件容器的当前宽度（以像素为单位）。                    |
| height                           | Number             | 部件容器的当前高度（以像素为单位）。                    |
| isEdit                           | Boolean            | 指示仪表板是处于视图状态还是处于编辑状态。|
| isMobile                         | Boolean            | 指示仪表板视图是否小于960px宽度（默认移动断点）。 |
| widgetConfig                     | Object             | 常见的窗口小部件配置，其中包含诸如颜色（文本颜色），backgroundColor（小部件背景颜色）等属性。 |
| settings                         | Object             | 根据定义的[json模式](#settings-schema-section)包含小部件特定属性的小部件设置 |
| units                            | String             | 定义窗口小部件显示的值的单位文本的可选属性。对于简单的小部件（如卡片或仪表）很有用。 |
| decimals                         | Number             | 可选属性，用于定义应使用多少个位置来显示数值的小数部分。  |
| hideTitlePanel                   | Boolean            | 管理窗口小部件标题面板的可见性。对于具有自定义标题面板或不同状态的小部件很有用。 |
| defaultSubscription              | Object             | 请参阅[对象订阅](#subscription-object) |
| timewindowFunctions              | Object             | 请参阅[Timewindow功能](#timewindow-functions) |
| controlApi                       | Object             | 请参阅[Control API](#control-api) | 
| actionsApi                       | Object             | 请参阅[Actions API](#actions-api) |
| stateController                  | Object             | 请参阅[状态Controller](#state-controller) |

为了实现新的窗口部件，应定义以下JavaScript函数（注意：每个函数都是可选的，可以根据窗口部件的特定/行为来实现）：
   
| **函数**                       | **描述**                                                                        |
|------------------------------------|----------------------------------------------------------------------------------------|
| ``` onInit() ```                   | 当widget准备好初始化时调用的第一个函数。应该用于准备小部件DOM，处理小部件设置和初始订阅信息。	 |
| ``` onDataUpdated() ```            | 在部件订阅中有新数据可用时调用。可以从窗口部件上下文（**ctx**）的[**defaultSubscription** object](#subscription-object)访问最新数据。|
| ``` onResize() ```                 | 调整窗口小部件容器的大小时调用。可以从窗口小部件上下文（**ctx**）获得最新的宽度和高度。             |
| ``` onEditModeChanged() ```        | 更改仪表板编辑模式时调用。最新模式由**ctx**的isEdit属性处理。             |
| ``` onMobileModeChanged() ```      | 当仪表板视图宽度超过移动断点时调用。最新状态由**ctx**的isMobile属性处理。                |
| ``` onDestroy() ```                | 当部件元素被销毁时调用。如有必要，应使用它来清理所有资源。            |
| ``` getSettingsSchema() ```        | 返回窗口小部件设置架构json的可选函数以替代[设置部分](#settings-schema-section)的**设置标签**。             |
| ``` getDataKeySettingsSchema() ``` | 返回特定数据密钥设置方案json的可选函数，替代**设置部分**[Settings schema section](#settings-schema-section)的数据密钥设置方案标签。             |
| ``` typeParameters() ```           | 检索描述窗口小部件数据源参数的对象。请参阅[类型参数对象](#type-parameters-object)类型参数对象。|            |
| ``` actionSources() ```            | 调用对象，该对象描述用于定义用户操作的可用窗口小部件操作源。请参阅[操作源对象](#action-sources-object)。 |


#### 订阅对象

窗口部件订阅对象包含所有订阅信息包括根据[部件类型](/docs/user-guide/ui/widget-library/#widget-types)的当前数据。

根据窗口部件类型，订阅对象提供不同的数据结构。

对于[最新值](/docs/user-guide/ui/widget-library/#latest-values)和[时间序列](/docs/user-guide/ui/widget-library/#time-series)部件类型，它提供以下属性：

 - **datasources** - 此订阅使用的数据源数组，具有以下结构:

```javascript
    datasources = [
        {  // datasource
           type: 'entity',// type of the datasource. Can be "function" or "entity"
           name: 'name', // name of the datasource (in case of "entity" usually Entity name)
           aliasName: 'aliasName', // name of the alias used to resolve this particular datasource Entity
           entityName: 'entityName', // name of the Entity used as datasource
           entityType: 'DEVICE', // datasource Entity type (for ex. "DEVICE", "ASSET", "TENANT", etc.)
           entityId: '943b8cd0-576a-11e7-824c-0b1cb331ec92', // entity identificator presented as string uuid. 
           dataKeys: [ // array of keys (attributes or timeseries) of the entity used to fetch data 
               { // dataKey
                    name: 'name', // the name of the particular entity attribute/timeseries 
                    type: 'timeseries', // type of the dataKey. Can be "timeseries", "attribute" or "function" 
                    label: 'Sin', // label of the dataKey. Used as display value (for ex. in the widget legend section) 
                    color: '#ffffff', // color of the key. Can be used by widget to set color of the key data (for ex. lines in line chart or segments in the pie chart).  
                    funcBody: "", // only applicable for datasource with type "function" and "function" key type. Defines body of the function to generate simulated data.
                    settings: {} // dataKey specific settings with structure according to the defined Data key settings json schema. See "Settings schema section".
               },
               //...
           ]
        },
        //...
    ]
```
    
  - **data** - 在此订阅范围内接收到的最新数据的数组，其结构如下：
  
```javascript
    data = [
        {
            datasource: {}, // datasource object of this data. See datasource structure above.
            dataKey: {}, // dataKey for which the data is held. See dataKey structure above.
            data: [ // array of data points
                [   // data point
                    1498150092317, // unix timestamp of datapoint in milliseconds
                    1, // value, can be either string, numeric or boolean  
                ],
                //...
            ]  
        },
        //...
    ]     
```

对于[Alarm小部件](/docs/user-guide/ui/widget-library/#alarm-widget)它提供以下属性：
 
 - **alarmSource** - 有关为其获取警报的实体的信息，并具有以下结构: 

```javascript
    alarmSource = {
         type: 'entity',// type of the alarm source. Can be "function" or "entity"
         name: 'name', // name of the alarm source (in case of "entity" usually Entity name)
         aliasName: 'aliasName', // name of the alias used to resolve this particular alarm source Entity
         entityName: 'entityName', // name of the Entity used as alarm source
         entityType: 'DEVICE', // alarm source Entity type (for ex. "DEVICE", "ASSET", "TENANT", etc.)
         entityId: '943b8cd0-576a-11e7-824c-0b1cb331ec92', // entity identificator presented as string uuid. 
         dataKeys: [ // array of keys indicating alarm fields used to display alarms data 
            { // dataKey
                 name: 'name', // the name of the particular alarm field 
                 type: 'alarm', // type of the dataKey. Only "alarm" in this case. 
                 label: 'Severity', // label of the dataKey. Used as display value (for ex. as a column title in the Alarms table) 
                 color: '#ffffff', // color of the key. Can be used by widget to set color of the key data.  
                 settings: {} // dataKey specific settings with structure according to the defined Data key settings json schema. See "Settings schema section".
            },
            //...
          ] 
    }
```

  - **alarms** - 在此订阅范围内收到的警报数组，其结构如下：

```javascript
    alarms = [
        { // alarm
            id: { // alarm id 
                entityType: "ALARM", 
                id: "943b8cd0-576a-11e7-824c-0b1cb331ec92"
            },
            createdTime: 1498150092317, // Alarm created time (unix timestamp)
            startTs: 1498150092316, // Alarm started time (unix timestamp)
            endTs: 1498563899065, // Alarm end time (unix timestamp)
            ackTs: 0, // Time of alarm aknowledgment (unix timestamp)
            clearTs: 0, // Time of alarm clear (unix timestamp)
            originator: { // Originator - id of entity produced this alarm 
                entityType: "ASSET", 
                id: "ceb16a30-4142-11e7-8b30-d5d66714ea5a"
            },
            originatorName: "Originator Name", // Name of originator entity
            type: "Temperature", // Type of the alarm            
            severity: "CRITICAL", // Severity of the alarm ("CRITICAL", "MAJOR", "MINOR", "WARNING", "INDETERMINATE") 
            status: "ACTIVE_UNACK", // Status of the alarm 
                                    // ("ACTIVE_UNACK" - active unacknowledged, 
                                    // "ACTIVE_ACK" - active acknowledged, 
                                    // "CLEARED_UNACK" - cleared unacknowledged, 
                                    // "CLEARED_ACK" - cleared acknowledged)
            details: {} // Alarm details object derived from alarm details json.
        }
    ]               
```

对于其他部件类型，例如[RPC](/docs/user-guide/ui/widget-library/#rpc-control-widget)或[Static](/docs/user-guide/ui/widget-library/#static)订阅对象是可选的，不包含必要的信息。

#### Timewindow函数

具有时间窗口功能的对象，用于管理窗口小部件数据时间范围。可以由[时间序列](/docs/user-guide/ui/widget-library/#time-series)或[警报](/docs/user-guide/ui/widget-library/#alarm-widget)部件。

| **函数**                                        | **描述**                                                                        |
|-----------------------------------------------------|----------------------------------------------------------------------------------------|
| ``` onUpdateTimewindow(startTimeMs, endTimeMs) ```  | 此功能可用于将当前订阅时间范围更新为由**startTimeMs**和**endTimeMs**参数标识的历史时间范围。 |
| ``` onResetTimewindow() ```                         | 根据窗口部件设置将订阅时间范围重置为由窗口部件时间窗口组件或仪表板时间窗口定义的默认时间范围。 |


#### Control API

提供[RPC (Control)](/docs/user-guide/ui/widget-library/#rpc-control-widget)部件的API函数的对象。

| **函数**                                        | **描述**                                                                        |
|-----------------------------------------------------|----------------------------------------------------------------------------------------|
| ``` sendOneWayCommand(method, params, timeout) ```  | 向设备发送一种方法（无响应）RPC命令。返回命令执行承诺。**method** -RPC方法名称，字符串，**params** -RPC方法参数，自定义json对象，**timeout** -等待接收到响应/确认之前的最大延迟（以毫秒为单位）。 |
| ``` sendTwoWayCommand(method, params, timeout) ```  | 设备发送两种方式（带有响应）的RPC命令。在成功回调中返回带有响应主体的命令执行承诺。 |


#### Actions API

API函数集，可与用户定义的操作一起使用。

| **函数**                                                          | **描述**                                                                        |
|-----------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| ``` getActionDescriptors(actionSourceId) ```                          | 返回提供的**actionSourceId**的动作描述符的列表                 |
| ``` handleWidgetAction($event, descriptor, entityId, entityName) ```  | 处理特定动作源产生的动作。**$event** -与Action相关的事件对象**descriptor**action描述符， **entityId**和**entityName** -当前实体ID和名称（如果可用）由动作源提供。 |


#### State Controller

引用仪表板状态控制器，提供用于管理当前仪表板状态的API。

| **函数**                                        | **描述**                                                                        |
|-----------------------------------------------------|----------------------------------------------------------------------------------------|
| ``` openState(id, params, openRightLayout) ```      | 导航到新的仪表板状态。**id** -目标仪表盘状态的ID，**params** -与状态参数对象被新的状态下使用，**openRightLayout** -可选布尔参数强行打开右仪表盘布局如果存在于移动视图模式。 |
| ``` updateState(id, params, openRightLayout) ```    | 更新当前仪表板状态。**id** -目标仪表盘状态的可选id替换当前的状态ID，**params** -与状态参数对象更新当前状态参数，**openRightLayout** -可选布尔参数强行打开右仪表盘布局如果存在于移动视图模式。 |
| ``` getStateId() ```                                | 返回当前的仪表板状态ID。 |
| ``` getStateParams() ```                            | 返回当前仪表板状态参数。 |
| ``` getStateParamsByStateId(id) ```                 | 返回由**id**标识的特定仪表板状态的状态参数。 |


#### 类型参数对象

描述小部件数据源参数的对象。它具有以下属性:

```javascript
    return {
        maxDatasources: -1, // Maximum allowed datasources for this widget, -1 - unlimited
        maxDataKeys: -1 //Maximum allowed data keys for this widget, -1 - unlimited
    }
```

#### 对象操作来源

映射描述了可以向其分配用户操作的可用窗口小部件操作源。它具有以下结构：

```javascript
   return {
        'headerButton': { // Action source Id (unique action source identificator)
           name: 'Header button', // Display name of action source, used in widget settings ('Actions' tab).
           multiple: true // Boolean property indicating if this action source supports multiple action definitions (for ex. multiple buttons in one cell, or only one action can by assigned on table row click.)
        }
    };   
```

### 创建简单部件 

以下是一组简单的教程，介绍如何创建每种类型的最小部件。

使用Angular框架将减少ThingsBoard UI代码量。

顺便说一句您始终可以在部件代码中使用纯JavaScript或jQuery API。

#### 最新值部件

在**件捆绑包**视图中单击屏幕右下角的“+”大按钮，然后单击**创建部件类型**按钮。

在**选择部件类型**弹出窗口中单击“新值”按钮。

打开**部件编辑器**”，并预先填充默认的**新值**模板部件的内容。

 - 清除“资源”部分CSS标签的内容。
 - 将以下HTML代码放入“资源”部分的HTML标签中：
  
```html
  {% raw  %}<div flex layout="column" style="height: 100%;" layout-align="center stretch">
      <div>My first latest values widget.</div>
      <div flex layout="row" ng-repeat="dataKeyData in data" layout-align="space-around center">
          <div>{{dataKeyData.dataKey.label}}:</div>
          <div>{{dataKeyData.data[0][0] | date : 'yyyy-MM-dd HH:mm:ss'}}</div>
          <div>{{dataKeyData.data[0][1]}}</div>
      </div>
  </div>{% endraw %}
```

 - 将以下JavaScript代码放入“JavaScript”部分中：
 
```javascript
    self.onInit = function() {
        
        self.ctx.$scope.data = self.ctx.defaultSubscription.data;
    
    }
```

 - 单击**部件编辑器工具栏**中的**运行**按钮，以便在**部件预览**部分中查看结果。
 
![image](/images/user-guide/contribution/widgets/latest-values-widget-sample.png) 

在此示例中，[subscription](#subscription-object)的**data**属性被分配给 **$scope** 并且可以在HTML模板中访问。

在HTML内部，使用特殊的**ng-repeat**角度指令来迭代可用的dataKeys数据点并使用其时间戳呈现相应的最新值。

#### 时间序列部件

在**部件捆绑包**视图中，单击屏幕右下角的“+”大按钮，然后单击**创建部件类型**按钮。

在**选择部件类型**弹出窗口中单击**时间系列**按钮。

将打开**部件编辑器**其中预填充了默认的**时间系列**模板小部件的内容。

 - 清除“资源”部分CSS标签的内容。
 - 将以下HTML代码放入“资源”部分的HTML标签中：

```html
  {% raw  %}<div flex layout="column" style="height: 100%;">
      <div>My first Time-Series widget.</div>
      <md-tabs md-border-bottom>
          <md-tab ng-repeat="datasource in datasources track by $index" label="{{datasource.name}}">
              <table style="width: 100%;">
                  <thead>
                      <tr>
                          <th>Timestamp</th>
                          <th ng-repeat="dataKeyData in datasourceData[$index]">{{dataKeyData.dataKey.label}}</th>
                      <tr>          
                  </thead>
                  <tbody>
                      <tr ng-repeat="data in datasourceData[$index][0].data track by $index">
                          <td>{{data[0] | date : 'yyyy-MM-dd HH:mm:ss'}}</td>
                          <td ng-repeat="dataKeyData in datasourceData[$parent.$index]">{{dataKeyData.data[$parent.$index][1]}}</td>
                      </tr>      
                  </tbody>          
              </table>          
          </md-tab>       
      </md-tabs>      
  </div>{% endraw %}
```

 - 将以下JavaScript代码放入“JavaScript”部分中：
 
```javascript
self.onInit = function() {
    self.ctx.$scope.datasources = self.ctx.defaultSubscription.datasources;
    self.ctx.$scope.data = self.ctx.defaultSubscription.data;
    
    self.ctx.$scope.datasourceData = [];
    
    var currentDatasource = null;
    var currentDatasourceIndex = -1;
    
    for (var i=0;i<self.ctx.$scope.data.length;i++) {
        var dataKeyData = self.ctx.$scope.data[i];
        if (dataKeyData.datasource != currentDatasource) {
            currentDatasource = dataKeyData.datasource
            currentDatasourceIndex++;
            self.ctx.$scope.datasourceData[currentDatasourceIndex] = [];
            
        } 
        self.ctx.$scope.datasourceData[currentDatasourceIndex].push(dataKeyData);
    }

}

self.onDataUpdated = function() {
    self.ctx.$scope.$digest();
}
```

 - 单击**部件编辑器工具栏**中的**运行**按钮以便在**部件预览**部分中查看结果。

![image](/images/user-guide/contribution/widgets/timeseries-widget-sample.png) 

在此示例中，[subscription](#subscription-object)的**datasources**和**data**属性被分配给**$scope**并且可以在HTML模板中访问。

引入了**datasourceData**范围属性，以通过数据源索引映射数据源特定的dataKeys数据以便在HTML模板中进行灵活访问。

在HTML内部，使用特殊的**ng-repeat**角度指令来迭代可用的数据源并呈现相应的选项卡。

在每个选项卡内，使用从datasource索引访问的**datasourceData**范围属性获取的dataKeys数据呈现表。

每个表都通过遍历所有**dataKeyData**对象来呈现列，并通过遍历每个**dataKeyData**的**data**数组以呈现时间戳和值来呈现所有可用的数据点。

请注意此代码中的**onDataUpdated**函数是通过调用有角度的 **$digest** 函数实现的，该函数对于接收新数据时执行新的渲染周期是必需的。

#### RPC(控制)部件

在**部件捆绑包**视图中单击屏幕右下角的“+”大按钮然后单击“创建部件类型”按钮。

在Z**选择部件类型**弹出窗口中单击**控制部件**按钮。

将打开**部件编辑器**并预先填充默认的**控制**模板小部件的内容。

 - 清除“资源”部分CSS标签的内容。
 - 将以下HTML代码放入“资源”部分的HTML标签中：

```html
  {% raw  %}<form name="rpcForm" ng-submit="sendCommand()">
    <md-content class="md-padding" layout="column">
        <md-input-container>
          <label>RPC method</label>  
          <input required name="rpcMethod" ng-model="rpcMethod"/>
          <div ng-messages="rpcForm.rpcMethod.$error">
            <div ng-message="required">RPC method name is required.</div>
          </div>
        </md-input-container>    
        <md-input-container>
          <label>RPC params</label>  
          <input required name="rpcParams" ng-model="rpcParams"/>
          <div ng-messages="rpcForm.rpcParams.$error">
            <div ng-message="required">RPC params is required.</div>
          </div>
        </md-input-container>    
        <md-button ng-disabled="rpcForm.$invalid || !rpcForm.$dirty" type="submit"
                   class="md-raised md-primary">
            Send RPC command
        </md-button>
        <div>
           <label>RPC command response</label>
           <div style="width: 100%; height: 100px; border: solid 2px gray" ng-bind-html="rpcCommandResponse">
           </div>       
        </div>
    </md-content>
  </form>{% endraw %}
```

 - 将以下JSON内容放入**设置架构**标签中：
 
```json
{
    "schema": {
        "type": "object",
        "title": "Settings",
        "properties": {
            "oneWayElseTwoWay": {
                "title": "Is One Way Command",
                "type": "boolean",
                "default": true
            },
            "requestTimeout": {
                "title": "RPC request timeout",
                "type": "number",
                "default": 500
            }
        },
        "required": []
    },
    "form": [
        "oneWayElseTwoWay",
        "requestTimeout"
    ]
} 
```

 - 将以下JavaScript代码放入“ JavaScript”部分中：
 
```javascript
self.onInit = function() {
    
    self.ctx.$scope.sendCommand = function() {
        var rpcMethod = self.ctx.$scope.rpcMethod;
        var rpcParams = self.ctx.$scope.rpcParams;
        var timeout = self.ctx.settings.requestTimeout;
        var oneWayElseTwoWay = self.ctx.settings.oneWayElseTwoWay ? true : false;
        
        var commandPromise;
        if (oneWayElseTwoWay) {
            commandPromise = self.ctx.controlApi.sendOneWayCommand(rpcMethod, rpcParams, timeout);
        } else {
            commandPromise = self.ctx.controlApi.sendTwoWayCommand(rpcMethod, rpcParams, timeout);
        }
        commandPromise.then(
            function success(response) {
                if (oneWayElseTwoWay) {
                    self.ctx.$scope.rpcCommandResponse = "Command was successfully received by device.<br/> No response body because of one way command mode.";
                } else {
                    self.ctx.$scope.rpcCommandResponse = "Response from device:<br/>";                    
                    self.ctx.$scope.rpcCommandResponse += angular.toJson(response);
                }
            },
            function fail(rejection) {
                self.ctx.$scope.rpcCommandResponse = "Failed to send command to the device:<br/>"
                self.ctx.$scope.rpcCommandResponse += "Status: " + rejection.status + "<br/>";
                self.ctx.$scope.rpcCommandResponse += "Status text: '" + rejection.statusText + "'";
            }
            
        );
    }
    
}
```

 - 用小部件类型名称填充**部件标题**字段，例如。 “我的第一个控件”。
 - 单击**部件编辑器工具栏**中的**运行**按钮以便在**部件预览**部分中查看结果。
 - 现在点击预览部分中的信息中心编辑按钮然后更改生成的小部件的大小然后单击仪表板应用按钮最终的小部件应如下图所示。
 
![image](/images/user-guide/contribution/widgets/control-widget-sample.png)
   
 - 单击**部件编辑器工具栏**中的**保存**按钮以保存小部件类型。
   
为了测试此窗口小部件如何执行RPC命令，我们需要将该窗口小部件放置在某些仪表板上并绑定到使用RPC命令的某些设备。为此，请执行以下步骤：
 
 - 以租户管理员身份登录。
 - 导航至**设备**，并使用某些名称创建新设备，例如。 “我的RPC设备”。
 - 打开设备详细信息，然后单击“复制访问令牌”按钮，以将设备访问令牌复制到剪贴板。
 - 下载[mqtt-js-rpc-from-server.sh](/docs/reference/resources/mqtt-js-rpc-from-server.sh)和[mqtt-js-rpc-from-server.js](/docs/reference/resources/mqtt-js-rpc-from-server.js)。将这些文件放在某个文件夹中。
 - 编辑**mqtt-js-rpc-from-server.sh**-用剪贴板中的设备访问令牌替换 **$ACCESS_TOKEN**。
 - 运行**mqtt-js-rpc-from-server.sh**脚本。如果一切正常，您应该在控制台中看到“已连接”消息。
 - 导航至**仪表板**并创建一个具有某些名称的新仪表板，例如。 “我的第一个控制仪表板”。打开此仪表板。
 - 点击信息中心的**修改**按钮。在仪表板编辑模式下，单击位于仪表板工具栏上的“实体别名”按钮。

![image](/images/user-guide/contribution/widgets/dashboard-toolbar-entity-aliases.png)
 
 - 在**实体别名**弹出窗口中，单击“添加别名”。
 - 填写“别名”字段，例如。 “我的RPC设备别名”。
 - 在“过滤器类型”字段中选择“实体列表”。
 - 在“类型”字段中选择“设备”。
 - 在“实体列表”字段中选择您的设备。在此示例中，“我的RPC设备”。
 
![image](/images/user-guide/contribution/widgets/add-rpc-device-alias.png)

 - 在**实体别名**中单击“添加”，然后单击“保存”。
 - 点击信息中心的“+”按钮，然后点击“创建部件”按钮。

![image](/images/user-guide/contribution/widgets/dashboard-create-new-widget-button.png)

 - 然后选择**部件捆绑包** RPC小部件的保存位置。选择“控件小部件”选项卡。
 - 点击小部件。在此示例中，“我的第一个控件”。
 - 在**添加小部件**弹出窗口中，在**目标设备**部分中选择您的设备别名。在此示例中，“我的RPC设备别名”。
 - 点击**添加**。您的控件小部件将出现在仪表板中。单击仪表板“应用更改”按钮以保存仪表板并退出编辑模式。
 - 在**RPC方法**名称字段中填充RPC方法名称。对于前。 “测试方法”。
 - 用**RPC参数**字段“{param1：'value1'}”。
 - 点击**发送RPC命令**​​按钮。您应该在小部件中看到以下响应。
 
![image](/images/user-guide/contribution/widgets/control-widget-sample-response-one-way.png)  
  
  以下输出应在设备控制台中打印：
  
```bash   
  request.topic: v1/devices/me/rpc/request/0
  request.body: {"method":"TestMethod","params":"{ param1: \"value1\" }"}
```

为了测试“Two way"” RPC命令模式，我们需要更改相应的窗口小部件设置属性。为此，请执行以下步骤：

 - 点击信息中心的**修改**按钮.在仪表板编辑模式下，单击“控制”小部件标题中的“编辑小部件”按钮。
 - 在小部件详细信息视图中，选择“高级”标签，然后取消选中“Is One Way Command”复选框。

![image](/images/user-guide/contribution/widgets/control-widget-sample-settings.png)   

 - 点击部件详细信息标题中的**应用更改**按钮。关闭详细信息，然后点击信息中心**应用更改**按钮。
 - 与前面的步骤一样，用RPC方法名称和参数填充部件字段。
 点击**发送RPC命令**​​按钮。您应该在小部件中看到以下响应。
 
![image](/images/user-guide/contribution/widgets/control-widget-sample-response-two-way.png)
  
 - 停止**mqtt-js-rpc-from-server.sh**脚本。
 点击**发送RPC命令**​​按钮。您应该在小部件中看到以下响应。
  
![image](/images/user-guide/contribution/widgets/control-widget-sample-response-timeout.png)  
  
在此示例中**controlApi**用于发送RPC命令。此外，引入了自定义窗口小部件设置以配置RPC命令模式和RPC请求超时。

设备的响应由具有成功和失败回调的**commandPromise**进行处理，并带有包含有关请求执行结果信息的相应响应或拒绝对象。

#### 警报小部件

在**部件捆绑**视图中单击屏幕右下角的“+”按钮然后单击**创建部件类型**按钮。

在**选择部件类型**弹出窗口中单击**警报部件**按钮。

使用默认的**Alarm**模板小部件的内容预填充打开**部件编辑器**。

 - 将以下HTML代码放入“资源”部分的HTML标签中：

```html
  {% raw  %}<div flex layout="column" style="height: 100%;">
      <div>My first Alarm widget.</div>
      <table style="width: 100%;">
          <thead>
              <tr>
                  <th ng-repeat="dataKey in alarmSource.dataKeys">{{dataKey.label}}</th> 
              <tr>          
          </thead>
          <tbody>
              <tr ng-repeat="alarm in alarms">
                  <td ng-repeat="dataKey in alarmSource.dataKeys" 
                      ng-style="getAlarmCellStyle(alarm, dataKey)">
                      {{getAlarmValue(alarm, dataKey)}}
                  </td>
              </tr>      
          </tbody>          
      </table>          
  </div>{% endraw %}
```**

 - 将以下JSON内容放入“设置”部分的**设置架构**标签中：
 
```json
{
    "schema": {
        "type": "object",
        "title": "AlarmTableSettings",
        "properties": {
            "alarmSeverityColorFunction": {
                "title": "Alarm severity color function: f(severity)",
                "type": "string",
                "default": "if(severity == 'CRITICAL') {return 'red';} else if (severity == 'MAJOR') {return 'orange';} else return 'green'; "
            }
        },
        "required": []
    },
    "form": [
        {
            "key": "alarmSeverityColorFunction",
            "type": "javascript"
        }
    ]
}
```

 - 将以下JavaScript代码放入“ JavaScript”部分中：
 
```javascript
self.onInit = function() {
    self.ctx.$scope.alarmSource = self.ctx.defaultSubscription.alarmSource;
    
    var alarmFields = self.ctx.$scope.$injector.get('types').alarmFields;
    var $filter = self.ctx.$scope.$injector.get('$filter');
    
    var alarmSeverityColorFunctionBody = self.ctx.settings.alarmSeverityColorFunction;
    if (angular.isUndefined(alarmSeverityColorFunctionBody) || !alarmSeverityColorFunctionBody.length) {
        alarmSeverityColorFunctionBody = "if(severity == 'CRITICAL') {return 'red';} else if (severity == 'MAJOR') {return 'orange';} else return 'green';";
    }
    
    var alarmSeverityColorFunction = null;
    try {
        alarmSeverityColorFunction = new Function('severity', alarmSeverityColorFunctionBody);
    } catch (e) {
        alarmSeverityColorFunction = null;
    }

    self.ctx.$scope.getAlarmValue = function(alarm, dataKey) {
        var alarmField = alarmFields[dataKey.name];
        if (alarmField) {
            var value = alarm[alarmField.value];
            if (alarmField.time) {
                return $filter('date')(value, 'yyyy-MM-dd HH:mm:ss');
            } else {
                return value;
            }
        } else {
            return alarm[dataKey.name];
        }
    }
    
    self.ctx.$scope.getAlarmCellStyle = function(alarm, dataKey) {
        var alarmField = alarmFields[dataKey.name];
        if (alarmField && alarmField == alarmFields.severity && alarmSeverityColorFunction) {
            var severity = alarm[alarmField.value];
            var color = alarmSeverityColorFunction(severity);
            return {
                color: color  
            };
        }
        return {};
    }
}

self.onDataUpdated = function() {
    self.ctx.$scope.alarms = self.ctx.defaultSubscription.alarms;
}
```

 - 单击“部件编辑器工具栏”中的“运行”按钮，以便在“部件预览”部分中查看结果。

![image](/images/user-guide/contribution/widgets/alarm-widget-sample.png)

在此示例中[订阅](#subscription-object)的**alarmSource**和**alarms**属性被分配给**$scope**并且可以在HTML模板中访问。

在HTML内部使用angular的**ng-repeat**指令进行遍历**alarmSource**可用警报的**dataKeys**并呈现相应的列。

在表中通过遍历**alarms**数组，显示对应的单元格**dataKeys**。

函数**getAlarmValue**是使用特殊的AlarmFields常量用来获取警报值该常量是从ThingsBoard UI的**types**中获取的并且可以通过Angular**$injector**进行访问。

函数**getAlarmCellStyle**用于为每个报警单元分配自定义样式。
在此示例中，我们引入了新的名为**alarmSeverityColorFunction**的设置属性该属性包含根据警报严重性返回颜色的函数主体。

在**getAlarmCellStyle**函数内部有一个带有严重性值的**alarmSeverityColorFunction**相应的调用，以获取警报严重性单元格的颜色。

请注意，在此代码中，实现了**onDataUpdated**函数，以便使用来自订阅的最新警报来更新**alarms**属性。

#### Static部件

在**部件捆绑**视图中单击屏幕右下角的“+”按钮然后单击**创建部件类型**按钮。

在**选择部件类型**弹出窗口中单击**Static部件**按钮。

打开**部件编辑器**并预先填充默认的***Static*模板小部件的内容。

 - 将以下HTML代码放入“资源”部分的HTML标签中：

```html
  {% raw  %}<div flex layout="column" style="height: 100%;" layout-align="space-around stretch">
      <h3 style="text-align: center;">My first static widget.</h3>
      <md-button class="md-primary md-raised" ng-click="showAlert()">Click me</md-button>
  </div>{% endraw %}
```

 - 将以下JSON内容放入“设置”的**设置架构**标签中：
 
```json
{
    "schema": {
        "type": "object",
        "title": "Settings",
        "properties": {
            "alertContent": {
                "title": "Alert content",
                "type": "string",
                "default": "Content derived from alertContent property of widget settings."
            }
        }
    },
    "form": [
        "alertContent"
    ]
}
``` 

 - 将以下JavaScript代码放入“ JavaScript”部分中：
 
```javascript
self.onInit = function() {

    self.ctx.$scope.showAlert = function() {
        var alertContent = self.ctx.settings.alertContent;
        if (!alertContent) {
            alertContent = "Content derived from alertContent property of widget settings.";
        }
        window.alert(alertContent);  
    };

}
```

 - 单击**部件编辑器工具栏**中的**运行**按钮，以便在**部件预览**部分中查看结果。

![image](/images/user-guide/contribution/widgets/static-widget-sample.png)

This is just a static HTML widget so there is no subscription data or special widget API was used.
这只是一个静态HTML小部件，因此没有订阅数据或使用了特殊的小部件API。

仅实现了自定义**showAlert**函数该函数显示具有小部件设置的**alertContent**属性内容的警报。

您可以在**部件预览**部分切换到仪表盘编辑模式，并通过在小部件详细信息的“高级”选项卡中更改小部件设置来更改**alertContent**的值。

然后您可以看到显示了新的警报内容。

## 集成现有代码创建部件定义

以下是一些示例，这些示例演示如何重用/集成外部JavaScript库或现有代码以创建新的小部件。
 
### 使用外部JavaScript库

在此示例中，将使用外部[gauge.js](http://bernii.github.io/gauge.js/)库创建**Latest Values**仪表小部件。

在**部件捆绑**视图中单击屏幕右下角的“+”大按钮，然后单击**创建部件类型**按钮。

在**选择部件类型**弹出窗口中单击**Latest Values**按钮。

将打开**部件编辑器**并预先填充默认的**Latest Values**模板小部件的内容。

 - 打开**资源**标签，然后单击“添加”，然后插入以下链接：

```  
http://bernii.github.io/gauge.js/dist/gauge.min.js  
```

 - 清除“资源”部分CSS标签的内容。
 - 将以下HTML代码放入“资源”部分的HTML标签中：
 
```html
  {% raw  %}<canvas id="my-gauge"></canvas>{% endraw %}
```

 - 将以下JavaScript代码放入“ JavaScript”部分中：
 
```javascript
var canvasElement;
var gauge;

self.onInit = function() {
    canvasElement = $('#my-gauge', self.ctx.$container)[0];
    gauge = new Gauge(canvasElement);
    gauge.minValue = -1000; 
    gauge.maxValue = 1000; 
    gauge.animationSpeed = 16; 
    self.onResize();
}

self.onResize = function() {
    canvasElement.width = self.ctx.width;
    canvasElement.height = self.ctx.height;
    gauge.update(true);
    gauge.render();
}

self.onDataUpdated = function() {
    var value = self.ctx.defaultSubscription.data[0].data[0][1];
    gauge.set(value);
}
```

 - 单击**部件编辑器工具栏**中的**运行**按钮以便在**部件预览**部分中查看结果。

![image](/images/user-guide/contribution/widgets/external-js-widget-sample.png)

在此示例中，使用了外部JS库的API，该API在**Resources**部分中注入相应的URL后才可用。

显示的值是从第一个dataKey的[subscription](#subscription-object) **data**属性获得的。

### 使用现有的JavaScript代码

创建窗口部件的另一种方法是使用现有的捆绑JavaScript代码。

在这种情况下，您可以创建自己的JavaScript类或Angular指令并将其捆绑到ThingsBoard UI代码中。

为了使此代码可在窗口部件中访问，您需要注册相应的Angular模块或将JavaScript类注入到全局变量（例如，对于window对象）。

一些ThingsBoard小部件已经使用这种方法。看看[widget.service.js](https://github.com/thingsboard/thingsboard/tree/master/ui/src/app/api/widget.service.js)。

在这里，您可以找到如何注册一些捆绑的类或模块以供以后在ThingsBoard小部件中使用。

例如**Timeseries-Flot**小部件（来自**Charts**部件捆绑包）使用[**TbFlot**](https://github.com/thingsboard/thingsboard/tree/master/ui/src/app/widget/lib/flot-widget.js)JavaScript类，将其作为窗口属性注入**widget.service.js**中：

```javascript
...

import TbFlot from '../widget/lib/flot-widget';
...

    $window.TbFlot = TbFlot;
...

```

另一个示例是使用“Angular”指令[**tb-timeseries-table-widget**](https://github.com/thingsboard/thingsboard/tree/master/ui/src/app/widget/lib/timeseries-table-widget.js)，该文件已注册为**thingsboard.api.widget**的Angular模块依赖 **widget.service.js**中的Angular模块的依赖项。

因此，此指令可在小部件模板HTML中使用。

```javascript
...

import thingsboardTimeseriesTableWidget from '../widget/lib/timeseries-table-widget';

...

export default angular.module('thingsboard.api.widget', ['oc.lazyLoad', thingsboardLedLight, thingsboardTimeseriesTableWidget,

...

```

## 小部件代码调试技巧

调试的最简单方法是Web控制台输出。

只需将[**console.log（...）**](https://developer.mozilla.org/en-US/docs/Web/API/Console/log) 函数放在小部件JavaScript代码的任何部分内即可。

然后单击**运行**按钮以重新启动小部件代码，并在Web控制台中观察调试信息。

另一种最有效的调试方法是调用浏览器调试器。

将[**debugger;**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger)语句放入您感兴趣的小部件代码的位置，然后单击**运行**按钮重新启动小部件代码。

浏览器调试器（如果启用）将自动在调试器语句处暂停代码执行，您将能够使用浏览器调试工具来分析脚本执行。

## Next steps

{% assign currentGuide = "Contribution" %}{% include templates/guides-banner.md %}