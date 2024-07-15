## Hive/Spark funtions

spark方言特有语法

hive 方言特有语法

从属于哪些类型的函数。

流程 

SQL => 







json_tuple



json_tuple 是表生成函数，虽然一个json展开是 **多列的一行** json，但是，hive 不允许 在select 命令多个列，需要配合lateral view 使用。例如SQL以下SQL只能在Spark运行

```
SELECT
    user_id,
    json_tuple(event_params,'camId', 'round','adsId') as (cam_id, round_num,ads_id),
    event_action,
    log_create_ts
FROM dwd.web_normalize_fact_inc
WHERE
    dt = '2024-07-08'
    and event_module = 'payinto_cam'
limit 100
```









