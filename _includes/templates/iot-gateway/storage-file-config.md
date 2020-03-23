|---
| **参数**            | **默认值**                            | **描述**                                                |
|:-|:-|-
| type                     | **file**                                     | Storage type (Saving data to hard drive)                       |
| data_folder_path         | **./data/**                                  | Path to folder, that will contains data (Relative or Absolute).|
| max_file_count           | **5**                                        | Maximum count of file that will be saved.                      |
| max_read_records_count * | **6**                                        | Count of messages to get from storage and send to ThingsBoard. |
| max_records_per_file     | **14**                                       | Maximum count of records that will be stored in one file.      |
|---


\* -- 如果已计算存储空间（如该参数所述）时接收数据，则新数据将丢失。

配置文件的“存储”部分将如下所示：

```yaml
storage
  type: file
  data_folder_path: ./data/
  max_file_count: 5
  max_read_records_count: 6
  max_records_per_file: 14
```
