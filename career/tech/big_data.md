# iot
## flink
数据采集、数据清洗、数据计算、结果入库

## flink_sql
基于calcite实现标准sql的解析和校验->SqlNode
物理执行计划会按照node类型的不同将node转换成dataset或datastream的API。
最终把SQL转换成DataSet或DataStream的API。
