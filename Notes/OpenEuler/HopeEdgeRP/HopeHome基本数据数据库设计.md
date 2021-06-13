# HopeHome

## Cloud 基本数据数据库设计

### 数据表设计

#### 1. 设备信息表(t_device)

表名: t_device

描述: 端节点设备

| 字段名      | 类型      | 长度 | 允许空否 | 说明                                          |
| ----------- | --------- | ---- | -------- | --------------------------------------------- |
| id          | int       | 11   | NOT NULL | 设备id，主键，自增                            |
| d_no        | varchar   | 255  | NOT NULL | 设备号，设备唯一硬件编号，不允许重复          |
| alias       | varchar   | 255  | NOT NULL | 设备别名，用于判断设备信息，例如 大厅，卧室等 |
| type        | varchar   | 255  | NOT NULL | 设备类型，主要有[采集,控制]类                 |
| isonline    | tinyint   | 1    | NOT NULL | 在线状态，1为在线，0为离线，默认为0           |
| carete_time | TIMESTAMP | 14   | NOT NULL | 创建时间                                      |
| update_time | TIMESTAMP | 14   | NOT NULL | 修改时间                                      |

#### 2. 温度表(t_temp)

表名:t_temp

描述:采集类设备的温度，如果设备为控制类，则表示该设备控制的目标温度，修改一次就记录一次

| 字段名      | 类型      | 长度  | 允许空否 | 说明                                                         |
| ----------- | --------- | ----- | -------- | ------------------------------------------------------------ |
| id          | int       | 11    | NOT NULL | 温度id，主键，自增                                           |
| d_no        | varchar   | 255   | NOT NULL | 设备号，温度所属设备号                                       |
| alias       | varchar   | 255   | NULL     | 设备别名，用于判断设备信息，例如 大厅，卧室等                |
| type        | varchar   | 255   | NOT NULL | 设备类型，主要有[采集,控制]类                                |
| temp        | DECIMAL   | (5,2) | NULL     | 温度，设备获取到的温度 例如 123.34℃，如果为控制类，则表示当前目标温度 |
| carete_time | TIMESTAMP | 14    | NOT NULL | 创建时间                                                     |
| update_time | TIMESTAMP | 14    | NOT NULL | 修改时间                                                     |

### 索引设计

除去 MySQL innoDB 默认的主键索引之外，还要对常用查询的字段添加索引。

1. t_temp 表中的 temp 字段