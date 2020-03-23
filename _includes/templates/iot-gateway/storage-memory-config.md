|---
| **参数**            | **默认值**                            | **描述**                                                |
|:-|:-|-
| type                     | **memory**                                   | Storage type (Saving data to RAM, no save to hard drive).      |
| read_records_count       | **10**                                       | Count of messages to get from storage and send to ThingsBoard. |
| max_records_count *      | **100**                                      | Maximum count of data in storage before send to ThingsBoard.   |
|---


\* -- 如果已计算存储空间（如该参数所述）时接收数据，则新数据将丢失。

配置文件的存储部分将如下所示：

```yaml
storage:
  type: memory
  read_records_count: 10
  max_records_count: 1000
```