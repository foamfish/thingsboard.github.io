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

<<<<<<< HEAD
所有与部件相关的代码都位于[JavaScript部分](#javascript-section)。
还提供了对部件实例的引用的内置变量**self**。
每个部件函数都应定义为**self**变量的属性。
**self**变量具有**ctx**属性-引用具有小部件实例使用的所有必要API和数据的小部件上下文。
以下是窗口部件上下文属性的简要说明：
=======
All widget related code is located in the [JavaScript section](#javascript-section).
The built-in variable **self** that is a reference to the widget instance is also available.
Each widget function should be defined as a property of the **self** variable.
**self** variable has property **ctx** of type [WidgetContext](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/modules/home/models/widget-component.models.ts#L83) - a reference to widget context that has all necessary API and data used by widget instance.
Below is brief description of widget context properties:
>>>>>>> master
 
| **属性**                     | **类型**           |  **描述**                                                |
|----------------------------------|--------------------|-----------------------------------------------------------------|
<<<<<<< HEAD
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
=======
| $container                       | jQuery Object      | Container element of the widget. Can be used to dynamically access or modify widget DOM using jQuery API. |
| $scope                           | [IDynamicWidgetComponent](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/modules/home/models/widget-component.models.ts#L274")             | Reference to the current widget component. Can be used to access/modify component properties when widget is built using Angular approach. |
| width                            | Number             | Current width of widget container in pixels.                     |
| height                           | Number             | Current height of widget container in pixels.                    |
| isEdit                           | Boolean            | Indicates whether the dashboard is in in the view or editing state.|
| isMobile                         | Boolean            | Indicates whether the dashboard view is less then 960px width (default mobile breakpoint). |
| widgetConfig                     | [WidgetConfig](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/shared/models/widget.models.ts#L341)             | Common widget configuration containing properties such as **color** (text color), **backgroundColor** (widget background color), etc. |
| settings                         | Object             | Widget settings containing widget specific properties according to the defined [settings json schema](#settings-schema-section) |
| datasources                      | Array<[Datasource](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/shared/models/widget.models.ts#L250)>  | Array of resolved widget datasources. See [Subscription object](#subscription-object). |
| data                             | Array<[DatasourceData](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/shared/models/widget.models.ts#L275)>  | Array of latest datasources data. See [Subscription object](#subscription-object). |
| timeWindow                       | [WidgetTimewindow](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/shared/models/time/time.models.ts#L104)   | Current widget timewindow (applicable for timeseries widgets). Holds information about current timewindow bounds. **minTime** - minimum time in UTC milliseconds, **maxTime** - maximum time in UTC milliseconds, **interval** - current aggregation interval in milliseconds.|
| units                            | String             | Optional property defining units text of values displayed by widget. Useful for simple widgets like cards or gauges. |
| decimals                         | Number             | Optional property defining how many positions should be used to display decimal part of the value number.  |
| hideTitlePanel                   | Boolean            | Manages visibility of widget title panel. Useful for widget with custom title panels or different states. **updateWidgetParams()** function must be called after this property change. |
| widgetTitle                      | String             | If set, will override configured widget title text. **updateWidgetParams()** function must be called after this property change. |
| detectChanges()                  | Function           | Trigger change detection for current widget. Must be invoked when widget HTML template bindings should be updated due to widget data changes. |
| updateWidgetParams()             | Function           | Updates widget with runtime set properties such as **widgetTitle**, **hideTitlePanel**, etc. Must be invoked in order these properties changes take effect. |
| defaultSubscription              | [IWidgetSubscription](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/core/api/widget-api.models.ts#L220")             | Default widget subscription object contains all subscription information, including current data, according to the widget type. See [Subscription object](#subscription-object). |
| timewindowFunctions              | [TimewindowFunctions](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/core/api/widget-api.models.ts#L45)             | Object with timewindow functions used to manage widget data time frame. Can by used by Time-series or Alarm widgets. See [Timewindow functions](#timewindow-functions). |
| controlApi                       | [RpcApi](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/core/api/widget-api.models.ts#L58)             | Object that provides API functions for RPC (Control) widgets. See [Control API](#control-api). | 
| actionsApi                       | [WidgetActionsApi](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/core/api/widget-api.models.ts#L67)             | Set of API functions to work with user defined actions. See [Actions API](#actions-api). |
| stateController                  | [IStateController](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/core/api/widget-api.models.ts#L121)             | Reference to Dashboard state controller, providing API to manage current dashboard state. See [State Controller](#state-controller). |

In order to implement a new widget, the following JavaScript functions should be defined *(Note: each function is optional and can be implemented according to  widget specific behaviour):*
>>>>>>> master
   
| **函数**                       | **描述**                                                                        |
|------------------------------------|----------------------------------------------------------------------------------------|
<<<<<<< HEAD
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
=======
| ``` onInit() ```                   | The first function which is called when widget is ready for initialization. Should be used to prepare widget DOM, process widget settings and initial subscription information. |
| ``` onDataUpdated() ```            | Called when the new data is available from the widget subscription. Latest data can be accessed from the [**defaultSubscription** object](#subscription-object) of widget context (**ctx**). |
| ``` onResize() ```                 | Called when widget container is resized. Latest width and height can be obtained from widget context (**ctx**).             |
| ``` onEditModeChanged() ```        | Called when dashboard editing mode is changed. Latest mode is handled by isEdit property of **ctx**.             |
| ``` onMobileModeChanged() ```      | Called when dashboard view width crosses mobile breakpoint. Latest state is handled by isMobile property of **ctx**.                 |
| ``` onDestroy() ```                | Called when widget element is destroyed. Should be used to cleanup all resources if necessary.            |
| ``` getSettingsSchema() ```        | Optional function returning widget settings schema json as alternative to **Settings tab** of [Settings schema section](#settings-schema-section).             |
| ``` getDataKeySettingsSchema() ``` | Optional function returning particular data key settings schema json as alternative to **Data key settings schema** tab of [Settings schema section](#settings-schema-section).             |
| ``` typeParameters() ```           | Returns [WidgetTypeParameters](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/shared/models/widget.models.ts#L146) object describing widget datasource parameters. See [Type parameters object](#type-parameters-object). |            |
| ``` actionSources() ```            | Returns map describing available widget action sources ([WidgetActionSource](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/shared/models/widget.models.ts#L118)) used to define user actions. See [Action sources object](#action-sources-object). |
>>>>>>> master

窗口部件订阅对象包含所有订阅信息包括根据[部件类型](/docs/user-guide/ui/widget-library/#widget-types)的当前数据。

根据窗口部件类型，订阅对象提供不同的数据结构。

<<<<<<< HEAD
对于[最新值](/docs/user-guide/ui/widget-library/#latest-values)和[时间序列](/docs/user-guide/ui/widget-library/#time-series)部件类型，它提供以下属性：

 - **datasources** - 此订阅使用的数据源数组，具有以下结构:
=======
The widget subscription object is instance of [IWidgetSubscription](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/core/api/widget-api.models.ts#L220") and contains all subscription information, including current data, according to the [widget type](/docs/user-guide/ui/widget-library/#widget-types).
Depending on widget type, subscription object provides different data structures.
For [Latest values](/docs/user-guide/ui/widget-library/#latest-values) and [Time-series](/docs/user-guide/ui/widget-library/#time-series) widget types, it provides the following properties:

 - **datasources** - array of datasources (Array<[Datasource](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/shared/models/widget.models.ts#L250)>) used by this subscription, using the following structure:
>>>>>>> master

```javascript
    datasources = [
        {  // datasource
           type: 'entity',// type of the datasource. Can be "function" or "entity"
           name: 'name', // name of the datasource (in case of "entity" usually Entity name)
           aliasName: 'aliasName', // name of the alias used to resolve this particular datasource Entity
           entityName: 'entityName', // name of the Entity used as datasource
           entityType: 'DEVICE', // datasource Entity type (for ex. "DEVICE", "ASSET", "TENANT", etc.)
           entityId: '943b8cd0-576a-11e7-824c-0b1cb331ec92', // entity identificator presented as string uuid. 
           dataKeys: [ //  array of keys (Array<DataKey>) (attributes or timeseries) of the entity used to fetch data 
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
    
<<<<<<< HEAD
  - **data** - 在此订阅范围内接收到的最新数据的数组，其结构如下：
=======
  - **data** - array of latest data (Array<[DatasourceData](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/shared/models/widget.models.ts#L275)>) received in scope of this subscription, using the following structure:
>>>>>>> master
  
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
 
<<<<<<< HEAD
 - **alarmSource** - 有关为其获取警报的实体的信息，并具有以下结构: 
=======
 - **alarmSource** - ([Datasource](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/shared/models/widget.models.ts#L250)) information about entity for which alarms are fetched, using the following structure: 
>>>>>>> master

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

<<<<<<< HEAD
  - **alarms** - 在此订阅范围内收到的警报数组，其结构如下：
=======
  - **alarms** - array of alarms (Array<[Alarm](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/shared/models/alarm.models.ts#L88)>) received in scope of this subscription, using the following structure:
>>>>>>> master

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

<<<<<<< HEAD
具有时间窗口功能的对象，用于管理窗口小部件数据时间范围。可以由[时间序列](/docs/user-guide/ui/widget-library/#time-series)或[警报](/docs/user-guide/ui/widget-library/#alarm-widget)部件。
=======
Object with timewindow functions ([TimewindowFunctions](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/core/api/widget-api.models.ts#L45)) used to manage widget data time frame. Can by used by [Time-series](/docs/user-guide/ui/widget-library/#time-series) or [Alarm](/docs/user-guide/ui/widget-library/#alarm-widget) widgets.
>>>>>>> master

| **函数**                                        | **描述**                                                                        |
|-----------------------------------------------------|----------------------------------------------------------------------------------------|
| ``` onUpdateTimewindow(startTimeMs, endTimeMs) ```  | 此功能可用于将当前订阅时间范围更新为由**startTimeMs**和**endTimeMs**参数标识的历史时间范围。 |
| ``` onResetTimewindow() ```                         | 根据窗口部件设置将订阅时间范围重置为由窗口部件时间窗口组件或仪表板时间窗口定义的默认时间范围。 |


#### Control API

<<<<<<< HEAD
提供[RPC (Control)](/docs/user-guide/ui/widget-library/#rpc-control-widget)部件的API函数的对象。
=======
Object that provides API functions ([RpcApi](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/core/api/widget-api.models.ts#L58)) for [RPC (Control)](/docs/user-guide/ui/widget-library/#rpc-control-widget) widgets.
>>>>>>> master

| **函数**                                        | **描述**                                                                        |
|-----------------------------------------------------|----------------------------------------------------------------------------------------|
| ``` sendOneWayCommand(method, params, timeout) ```  | 向设备发送一种方法（无响应）RPC命令。返回命令执行承诺。**method** -RPC方法名称，字符串，**params** -RPC方法参数，自定义json对象，**timeout** -等待接收到响应/确认之前的最大延迟（以毫秒为单位）。 |
| ``` sendTwoWayCommand(method, params, timeout) ```  | 设备发送两种方式（带有响应）的RPC命令。在成功回调中返回带有响应主体的命令执行承诺。 |


#### Actions API

<<<<<<< HEAD
API函数集，可与用户定义的操作一起使用。
=======
Set of API functions ([WidgetActionsApi](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/core/api/widget-api.models.ts#L67)) to work with user defined actions.
>>>>>>> master

| **函数**                                                          | **描述**                                                                        |
|-----------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| ``` getActionDescriptors(actionSourceId) ```                          | 返回提供的**actionSourceId**的动作描述符的列表                 |
| ``` handleWidgetAction($event, descriptor, entityId, entityName) ```  | 处理特定动作源产生的动作。**$event** -与Action相关的事件对象**descriptor**action描述符， **entityId**和**entityName** -当前实体ID和名称（如果可用）由动作源提供。 |


#### State Controller

<<<<<<< HEAD
引用仪表板状态控制器，提供用于管理当前仪表板状态的API。
=======
Reference to Dashboard state controller ([IStateController](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/core/api/widget-api.models.ts#L121)), providing API to manage current dashboard state.
>>>>>>> master

| **函数**                                        | **描述**                                                                        |
|-----------------------------------------------------|----------------------------------------------------------------------------------------|
| ``` openState(id, params, openRightLayout) ```      | 导航到新的仪表板状态。**id** -目标仪表盘状态的ID，**params** -与状态参数对象被新的状态下使用，**openRightLayout** -可选布尔参数强行打开右仪表盘布局如果存在于移动视图模式。 |
| ``` updateState(id, params, openRightLayout) ```    | 更新当前仪表板状态。**id** -目标仪表盘状态的可选id替换当前的状态ID，**params** -与状态参数对象更新当前状态参数，**openRightLayout** -可选布尔参数强行打开右仪表盘布局如果存在于移动视图模式。 |
| ``` getStateId() ```                                | 返回当前的仪表板状态ID。 |
| ``` getStateParams() ```                            | 返回当前仪表板状态参数。 |
| ``` getStateParamsByStateId(id) ```                 | 返回由**id**标识的特定仪表板状态的状态参数。 |


#### 类型参数对象

<<<<<<< HEAD
描述小部件数据源参数的对象。它具有以下属性:
=======
Object [WidgetTypeParameters](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/shared/models/widget.models.ts#L146) describing widget datasource parameters. It has the following properties:
>>>>>>> master

```javascript
    return {
        maxDatasources: -1, // Maximum allowed datasources for this widget, -1 - unlimited
        maxDataKeys: -1, //Maximum allowed data keys for this widget, -1 - unlimited
        dataKeysOptional: false //Whether this widget can be configured with datasources without data keys
    }
```

#### 对象操作来源

<<<<<<< HEAD
映射描述了可以向其分配用户操作的可用窗口小部件操作源。它具有以下结构：
=======
Map describing available widget action sources ([WidgetActionSource](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/shared/models/widget.models.ts#L118)) to which user actions can be assigned. It has the following structure:
>>>>>>> master

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
  {% raw  %}<div fxFlex fxLayout="column" style="height: 100%;" fxLayoutAlign="center stretch">
    <div>My first latest values widget.</div>
    <div fxFlex fxLayout="row" *ngFor="let dataKeyData of data" fxLayoutAlign="space-around center">
        <div>{{dataKeyData.dataKey.label}}:</div>
        <div>{{(dataKeyData.data[0] && dataKeyData.data[0][0]) | date : 'yyyy-MM-dd HH:mm:ss' }}</div>
        <div>{{dataKeyData.data[0] && dataKeyData.data[0][1]}}</div>
    </div>
  </div>{% endraw %}
```

 - 将以下JavaScript代码放入“JavaScript”部分中：
 
```javascript
    self.onInit = function() {
       self.ctx.$scope.data = self.ctx.defaultSubscription.data;
    }
        
    self.onDataUpdated = function() {
        self.ctx.detectChanges();
    }
```

 - 单击**部件编辑器工具栏**中的**运行**按钮，以便在**部件预览**部分中查看结果。
 
![image](/images/user-guide/contribution/widgets/latest-values-widget-sample.png) 

<<<<<<< HEAD
在此示例中，[subscription](#subscription-object)的**data**属性被分配给 **$scope** 并且可以在HTML模板中访问。

在HTML内部，使用特殊的**ng-repeat**角度指令来迭代可用的dataKeys数据点并使用其时间戳呈现相应的最新值。

#### 时间序列部件

在**部件捆绑包**视图中，单击屏幕右下角的“+”大按钮，然后单击**创建部件类型**按钮。
=======
In this example, the **data** property of [subscription](#subscription-object) is assigned to the **$scope** and becomes accessible within the HTML template.
Inside the HTML, a special [***ngFor**](https://angular.io/api/common/NgForOf) structural angular directive is used in order to iterate over available dataKeys & datapoints then render latest values with their corresponding timestamps. 
>>>>>>> master

在**选择部件类型**弹出窗口中单击**时间系列**按钮。

将打开**部件编辑器**其中预填充了默认的**时间系列**模板小部件的内容。

<<<<<<< HEAD
 - 清除“资源”部分CSS标签的内容。
 - 将以下HTML代码放入“资源”部分的HTML标签中：
=======
 - Replace content of the CSS tab in "Resources" section with the following one:

```css
.my-data-table th {
    text-align: left;
}
``` 
 
 - Put the following HTML code inside the HTML tab of "Resources" section:
>>>>>>> master

```html
  {% raw  %}<mat-tab-group style="height: 100%;">
      <mat-tab *ngFor="let datasource of datasources; let $dsIndex = index" label="{{datasource.name}}">
          <table class="my-data-table" style="width: 100%;">
              <thead>
                  <tr>
                      <th>Timestamp</th>
                      <th *ngFor="let dataKeyData of datasourceData[$dsIndex]">{{dataKeyData.dataKey.label}}</th>
                  <tr>          
              </thead>
              <tbody>
                  <tr *ngFor="let data of datasourceData[$dsIndex][0].data; let $dataIndex = index">
                      <td>{{data[0] | date : 'yyyy-MM-dd HH:mm:ss'}}</td>
                      <td *ngFor="let dataKeyData of datasourceData[$dsIndex]">{{dataKeyData.data[$dataIndex] && dataKeyData.data[$dataIndex][1]}}</td>
                  </tr>      
              </tbody>          
          </table>          
      </mat-tab>       
  </mat-tab-group>{% endraw %}
```

 - 将以下JavaScript代码放入“JavaScript”部分中：
 
```javascript
self.onInit = function() {
    self.ctx.widgetTitle = 'My first Time-Series widget';
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
    self.ctx.updateWidgetParams();

}

self.onDataUpdated = function() {
    self.ctx.detectChanges();
}
```

 - 单击**部件编辑器工具栏**中的**运行**按钮以便在**部件预览**部分中查看结果。

![image](/images/user-guide/contribution/widgets/timeseries-widget-sample.png) 

<<<<<<< HEAD
在此示例中，[subscription](#subscription-object)的**datasources**和**data**属性被分配给**$scope**并且可以在HTML模板中访问。

引入了**datasourceData**范围属性，以通过数据源索引映射数据源特定的dataKeys数据以便在HTML模板中进行灵活访问。

在HTML内部，使用特殊的**ng-repeat**角度指令来迭代可用的数据源并呈现相应的选项卡。

在每个选项卡内，使用从datasource索引访问的**datasourceData**范围属性获取的dataKeys数据呈现表。

每个表都通过遍历所有**dataKeyData**对象来呈现列，并通过遍历每个**dataKeyData**的**data**数组以呈现时间戳和值来呈现所有可用的数据点。

请注意此代码中的**onDataUpdated**函数是通过调用有角度的 **$digest** 函数实现的，该函数对于接收新数据时执行新的渲染周期是必需的。

#### RPC(控制)部件

在**部件捆绑包**视图中单击屏幕右下角的“+”大按钮然后单击“创建部件类型”按钮。
=======
In this example, the [subscription](#subscription-object) **datasources** and **data** properties are assigned to **$scope** and become accessible within the HTML template.
The **$scope.datasourceData** property is introduced to map datasource specific dataKeys data by datasource index for flexible access within the HTML template.
Inside the HTML, a special [***ngFor**](https://angular.io/api/common/NgForOf) structural angular directive is used in order to iterate over available datasources and render corresponding tabs.
Inside each tab, the table is rendered using dataKeys obtained from **datasourceData** scope property accessed by datasource index.
Each table renders columns by iterating over all **dataKeyData** objects and renders all available datapoints by iterating over **data** array of each **dataKeyData** to render timestamps and values.
Note that in this code, **onDataUpdated** function is implemented with a call to **detectChanges** function necessary to perform new change detection cycle when new data is received.   
>>>>>>> master

在Z**选择部件类型**弹出窗口中单击**控制部件**按钮。

将打开**部件编辑器**并预先填充默认的**控制**模板小部件的内容。

 - 清除“资源”部分CSS标签的内容。
 - 将以下HTML代码放入“资源”部分的HTML标签中：

```html
    {% raw  %}<form #rpcForm="ngForm" (submit)="sendCommand()">
      <div class="mat-content mat-padding" fxLayout="column">
        <mat-form-field class="mat-block">
          <mat-label>RPC method</mat-label>
          <input matInput required name="rpcMethod" #rpcMethodField="ngModel" [(ngModel)]="rpcMethod"/>
          <mat-error *ngIf="rpcMethodField.hasError('required')">
            RPC method name is required.
          </mat-error>
        </mat-form-field>
        <mat-form-field class="mat-block">
          <mat-label>RPC params</mat-label>
          <input matInput required name="rpcParams" #rpcParamsField="ngModel" [(ngModel)]="rpcParams"/>
          <mat-error *ngIf="rpcParamsField.hasError('required')">
            RPC params is required.
          </mat-error>
        </mat-form-field>
        <button [disabled]="rpcForm.invalid || !rpcForm.dirty" mat-raised-button color="primary" type="submit" >
          Send RPC command
        </button>
        <div>
          <label>RPC command response</label>
          <div style="width: 100%; height: 100px; border: solid 2px gray" [innerHTML]="rpcCommandResponse">
          </div>
        </div>
      </div>
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

        var commandObservable;
        if (oneWayElseTwoWay) {
            commandObservable = self.ctx.controlApi.sendOneWayCommand(rpcMethod, rpcParams, timeout);
        } else {
            commandObservable = self.ctx.controlApi.sendTwoWayCommand(rpcMethod, rpcParams, timeout);
        }
        commandObservable.subscribe(
            function (response) {
                if (oneWayElseTwoWay) {
                    self.ctx.$scope.rpcCommandResponse = "Command was successfully received by device.<br> No response body because of one way command mode.";
                } else {
                    self.ctx.$scope.rpcCommandResponse = "Response from device:<br>";                    
                    self.ctx.$scope.rpcCommandResponse += JSON.stringify(response, undefined, 2);
                }
                self.ctx.detectChanges();
            },
            function (rejection) {
                self.ctx.$scope.rpcCommandResponse = "Failed to send command to the device:<br>"
                self.ctx.$scope.rpcCommandResponse += "Status: " + rejection.status + "<br>";
                self.ctx.$scope.rpcCommandResponse += "Status text: '" + rejection.statusText + "'";
                self.ctx.detectChanges();
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
  
<<<<<<< HEAD
在此示例中**controlApi**用于发送RPC命令。此外，引入了自定义窗口小部件设置以配置RPC命令模式和RPC请求超时。
=======
In this example, **controlApi** is used to send RPC commands. Additionally, custom widget settings were introduced in order to configure RPC command mode and RPC request timeout.
The response from the device is handled by **commandObservable**.  It has success and failed callbacks with corresponding response, or rejection objects containing information about request execution result.     
>>>>>>> master

设备的响应由具有成功和失败回调的**commandPromise**进行处理，并带有包含有关请求执行结果信息的相应响应或拒绝对象。

#### 警报小部件

<<<<<<< HEAD
在**部件捆绑**视图中单击屏幕右下角的“+”按钮然后单击**创建部件类型**按钮。

在**选择部件类型**弹出窗口中单击**警报部件**按钮。

使用默认的**Alarm**模板小部件的内容预填充打开**部件编辑器**。

 - 将以下HTML代码放入“资源”部分的HTML标签中：
=======
- Replace content of the CSS tab in "Resources" section with the following one:

```css
.my-alarm-table th {
    text-align: left;
}
``` 

 - Put the following HTML code inside the HTML tab of "Resources" section:
>>>>>>> master

```html
  {% raw  %}<div fxFlex fxLayout="column" style="height: 100%;">
      <div>My first Alarm widget.</div>
      <table class="my-alarm-table" style="width: 100%;">
          <thead>
              <tr>
                  <th *ngFor="let dataKey of alarmSource?.dataKeys">{{dataKey.label}}</th> 
              <tr>          
          </thead>
          <tbody>
              <tr *ngFor="let alarm of alarms">
                  <td *ngFor="let dataKey of alarmSource?.dataKeys" 
                      [ngStyle]="getAlarmCellStyle(alarm, dataKey)">
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
    
    var alarmSeverityColorFunctionBody = self.ctx.settings.alarmSeverityColorFunction;
    if (typeof alarmSeverityColorFunctionBody === 'undefined' || !alarmSeverityColorFunctionBody.length) {
        alarmSeverityColorFunctionBody = "if(severity == 'CRITICAL') {return 'red';} else if (severity == 'MAJOR') {return 'orange';} else return 'green';";
    }
    
    var alarmSeverityColorFunction = null;
    try {
        alarmSeverityColorFunction = new Function('severity', alarmSeverityColorFunctionBody);
    } catch (e) {
        alarmSeverityColorFunction = null;
    }

    self.ctx.$scope.getAlarmValue = function(alarm, dataKey) {
        var alarmKey = dataKey.name;
        if (alarmKey === 'originator') {
            alarmKey = 'originatorName';
        }
        var value = alarm[alarmKey];
        if (alarmKey === 'createdTime') {
            return self.ctx.date.transform(value, 'yyyy-MM-dd HH:mm:ss');
        } else {
            return value;
        }
    }
    
    self.ctx.$scope.getAlarmCellStyle = function(alarm, dataKey) {
        var alarmKey = dataKey.name;
        if (alarmKey === 'severity' && alarmSeverityColorFunction) {
            var severity = alarm[alarmKey];
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
    self.ctx.detectChanges();
}
```

 - 单击“部件编辑器工具栏”中的“运行”按钮，以便在“部件预览”部分中查看结果。

![image](/images/user-guide/contribution/widgets/alarm-widget-sample.png)

<<<<<<< HEAD
在此示例中[订阅](#subscription-object)的**alarmSource**和**alarms**属性被分配给**$scope**并且可以在HTML模板中访问。

在HTML内部使用angular的**ng-repeat**指令进行遍历**alarmSource**可用警报的**dataKeys**并呈现相应的列。

在表中通过遍历**alarms**数组，显示对应的单元格**dataKeys**。

函数**getAlarmValue**是使用特殊的AlarmFields常量用来获取警报值该常量是从ThingsBoard UI的**types**中获取的并且可以通过Angular**$injector**进行访问。
=======
In this example, the **alarmSource** and **alarms** properties of [subscription](#subscription-object) are assigned to **$scope** and become accessible within HTML template.
Inside the HTML, a special [***ngFor**](https://angular.io/api/common/NgForOf) structural angular directive is used in order to iterate over available alarm **dataKeys** of **alarmSource** and render corresponding columns.
The table rows are rendered by iterating over **alarms** array and corresponding cells rendered by iterating over **dataKeys**.
The function **getAlarmValue** is fetching alarm value and formatting **createdTime** alarm property using a [DatePipe](https://angular.io/api/common/DatePipe) angular pipe accessible via **date** property of **ctx**.
The function **getAlarmCellStyle** is used to assign custom cell styles for each alarm cell. In this example, we introduced new settings property called **alarmSeverityColorFunction** that contains function body returning color depending on alarm severity.
Inside the **getAlarmCellStyle** function there is corresponding invocation of **alarmSeverityColorFunction** with severity value in order to get color for alarm severity cell. 
Note that in this code **onDataUpdated** function is implemented in order to update **alarms** property with latest alarms from subscription and invoke change detection using **detectChanges()** function.   
>>>>>>> master

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
  {% raw  %}<div fxFlex fxLayout="column" style="height: 100%;" fxLayoutAlign="space-around stretch">
    <h3 style="text-align: center;">My first static widget.</h3>
    <button mat-raised-button color="primary" (click)="showAlert()">Click me</button>
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

<<<<<<< HEAD
在**部件捆绑**视图中单击屏幕右下角的“+”大按钮，然后单击**创建部件类型**按钮。
=======
#### Latest Values Example

In this example, **Latest Values** gauge widget will be created using external [gauge.js](http://bernii.github.io/gauge.js/) library.
>>>>>>> master

在**选择部件类型**弹出窗口中单击**Latest Values**按钮。

将打开**部件编辑器**并预先填充默认的**Latest Values**模板小部件的内容。

 - 打开**资源**标签，然后单击“添加”，然后插入以下链接：

```  
https://bernii.github.io/gauge.js/dist/gauge.min.js  
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

<<<<<<< HEAD
为了使此代码可在窗口部件中访问，您需要注册相应的Angular模块或将JavaScript类注入到全局变量（例如，对于window对象）。

一些ThingsBoard小部件已经使用这种方法。看看[widget.service.js](https://github.com/thingsboard/thingsboard/tree/master/ui/src/app/api/widget.service.js)。

在这里，您可以找到如何注册一些捆绑的类或模块以供以后在ThingsBoard小部件中使用。

例如**Timeseries-Flot**小部件（来自**Charts**部件捆绑包）使用[**TbFlot**](https://github.com/thingsboard/thingsboard/tree/master/ui/src/app/widget/lib/flot-widget.js)JavaScript类，将其作为窗口属性注入**widget.service.js**中：
=======
#### Time-Series Example

In this example, **Time-Series** line chart widget will be created using external [Chart.js](https://www.chartjs.org/) library.

In the **Widgets Bundle** view, click the big “+” button at the bottom-right part of the screen, then click the “Create new widget type” button.
Click the **Time-Series** button on the **Select widget type** popup.
The **Widget Editor** will be opened, pre-populated with the content of default **Time-Series** template widget.

 - Open **Resources** tab and click "Add" then insert the following link:

```  
https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.9.3/Chart.min.js
```

 - Clear content of the CSS tab of "Resources" section.
 - Put the following HTML code inside the HTML tab of "Resources" section:
 
```html
  {% raw  %}<canvas id="myChart"></canvas>{% endraw %}
```
>>>>>>> master

 - Put the following JavaScript code inside the "JavaScript" section:
 
```javascript
var myChart;

self.onInit = function() {
    
    var chartData = {
        datasets: []
    };

    for (var i=0; i < self.ctx.data.length; i++) {
        var dataKey = self.ctx.data[i].dataKey;
        var dataset = {
            label: dataKey.label,
            data: [],
            borderColor: dataKey.color,
            fill: false
        };
        chartData.datasets.push(dataset);
    }
    
    var options = {
        maintainAspectRatio: false,
        legend: {
            display: false
        },
        scales: {
        xAxes: [{
            type: 'time',
            ticks: {
                maxRotation: 0,
                autoSkipPadding: 30
            }
        }]
    }
    };
    
    var canvasElement = $('#myChart', self.ctx.$container)[0];
    var canvasCtx = canvasElement.getContext('2d');
    myChart = new Chart(canvasCtx, {
        type: 'line',
        data: chartData,
        options: options
    });
    self.onResize();
}

self.onResize = function() {
    myChart.resize();
}

self.onDataUpdated = function() {
    for (var i = 0; i < self.ctx.data.length; i++) {
        var datasourceData = self.ctx.data[i];
        var dataSet = datasourceData.data;
        myChart.data.datasets[i].data.length = 0;
        var data = myChart.data.datasets[i].data;
        for (var d = 0; d < dataSet.length; d++) {
            var tsValuePair = dataSet[d];
            var ts = tsValuePair[0];
            var value = tsValuePair[1];
            data.push({t: ts, y: value});
        }
    }
    myChart.options.scales.xAxes[0].ticks.min = self.ctx.timeWindow.minTime;
    myChart.options.scales.xAxes[0].ticks.max = self.ctx.timeWindow.maxTime;
    myChart.update();
}
```

 - Click the **Run** button on the **Widget Editor Toolbar** in order to see the result in **Widget preview** section.

![image](/images/user-guide/contribution/widgets/external-js-timeseries-widget-sample.png)

In this example, the external JS library API was used that becomes available after injecting the corresponding URL in **Resources** section.
Initially chart datasets prepared using configured dataKeys from **data** property of **ctx**.
In the **onDataUpdated** function datasources data converted to Chart.js line chart format and pushed to chart datasets.
Please note that xAxis (time axis) is limited to current timewindow bounds obtained from **timeWindow** property of **ctx**.  

### Using existing JavaScript code

Another approach of creating widgets is to use existing bundled JavaScript code.
In this case, you can create own TypeScript class or Angular component and bundle it into the ThingsBoard UI code.
In order to make this code accessible within the widget, you need to register corresponding Angular module or inject TypeScript class to a global variable (for ex. window object).
Some of the ThingsBoard widgets already use this approach. Take a look at the [polyfills.ts](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/polyfills.ts#L106)
or [widget-components.module.ts](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/modules/home/components/widget/widget-components.module.ts#L44).
Here you can find how some bundled classes or components are registered for later use in ThingsBoard widgets.
For example "Timeseries - Flot" widget (from "Charts" Widgets Bundle) uses [**TbFlot**](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/modules/home/components/widget/lib/flot-widget.ts#L63) TypeScript class which is injected as window property inside **polyfills.ts**:

```typescript
...

import { TbFlot } from '@home/components/widget/lib/flot-widget';
...

    (window as any).TbFlot = TbFlot;
...

```

<<<<<<< HEAD
另一个示例是使用“Angular”指令[**tb-timeseries-table-widget**](https://github.com/thingsboard/thingsboard/tree/master/ui/src/app/widget/lib/timeseries-table-widget.js)，该文件已注册为**thingsboard.api.widget**的Angular模块依赖 **widget.service.js**中的Angular模块的依赖项。

因此，此指令可在小部件模板HTML中使用。
=======
Another example is "Timeseries table" widget (from "Cards" Widgets Bundle) that uses Angular component [**tb-timeseries-table-widget**](https://github.com/thingsboard/thingsboard/blob/13e6b10b7ab830e64d31b99614a9d95a1a25928a/ui-ngx/src/app/modules/home/components/widget/lib/timeseries-table-widget.component.ts#L99) which is registered as dependency of **WidgetComponentsModule** Angular module inside **widget-components.module.ts**.
Thereby this component becomes available for use inside the widget template HTML. 
>>>>>>> master

```typescript
...

import { TimeseriesTableWidgetComponent } from '@home/components/widget/lib/timeseries-table-widget.component';

...

@NgModule({
  declarations:
    [
...
      TimeseriesTableWidgetComponent,
...
    ],
...
  exports: [
...
      TimeseriesTableWidgetComponent,
...
  ],
...    
})
export class WidgetComponentsModule { }
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