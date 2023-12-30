# 资方产品历史数据处理

# 1. 展业的产品

```SQL
SELECT product_id,
       product_name,
       CASE type
           WHEN 1 THEN '自持'
           ELSE '非自持' END
                     AS '是否自持',
       channel_name,
       new_fund_name AS '资方名称',
       CASE product_id
           WHEN 128 THEN '4号'
           WHEN 115 THEN '3号'
           WHEN 130 THEN '6号'
           WHEN 112 THEN '2号'
           WHEN 133 THEN '10号'
           WHEN 132 THEN '9号'
           WHEN 134 THEN '8号'
           WHEN 131 THEN '7号'
           WHEN 129 THEN '5号'
           END       AS '产品序号'
FROM fund_info fi
WHERE fi.is_delete = 0
  AND fi.status = 0
ORDER BY type, new_fund_name;
```
![fundinfo.png](fundinfo.png)

# 2. 光富宝-2号-越秀资方



## 2.1 获取2号产品历史订单产品配置数据



```sql
SELECT *
FROM exhibition_area_config eac
WHERE eac.is_delete = 0
  AND eac.product_id = 112;
```



### 2.1.1 异常订单（产品配置无设计方案）

- <u>296笔异常订单</u>

```sql
SELECT o.order_no,
       o.order_stage,
       o.order_status,
       o.exhibition_area_config_id,
       o.roof_type,
       o.approve_price,
       o.create_time,
       o.update_time
FROM `xk-order`.`order` o
WHERE o.exhibition_area_config_id IN (SELECT eac.id
                                      FROM exhibition_area_config eac
                                      WHERE eac.is_delete = 0
                                          AND eac.product_id = 112
                                          AND eac.project_config IS NULL
                                         OR eac.project_config = '')
  AND o.order_status NOT IN ('取消', '已放款');
```



```sql
SELECT COUNT(*)
FROM `xk-order`.`order` o
WHERE o.exhibition_area_config_id IN (SELECT eac.id
                                      FROM exhibition_area_config eac
                                      WHERE eac.is_delete = 0
                                          AND eac.product_id = 112
                                          AND eac.project_config IS NULL
                                         OR eac.project_config = '')
  AND o.order_status NOT IN ('取消', '已放款');
```



























