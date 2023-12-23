
# 光富宝-同裕


### SQL

```SQL
SELECT l.id,
       l.order_no                      订单号,
       l.approve_price,
       l.age_scheme,
       l.fund_id,
       l.control_oneself,
       l.roof_type                     电站设计方案,
       l.fund_name                     产品名称,
       l.order_status                  订单状态,
       l.create_time                   订单创建时间,
       l.update_time                   订单修改时间,
       oe.approve_price,
       oe.net_price,
       oe.power_design_scheme,
       oe.equipment_suit,
       oe.earnings_deadline,
       oe.payment_date,
       oe.payment_cycle,
       oe.each_earnings_price,
       oe.project_clearn_price,
       oe.project_warranty_price,
       oe.power_station_financing,
       oe.power_design_scheme       AS '订单电站技术方案',
       oe.power_design_scheme_mixed AS '订单电站混装技术方案',
       eac.id                          产品配置ID,
       eac.version                     产品配置version,
       eac.power_design_scheme         产品配置技术方案json,
       eac.project_config              产品配置设计方案json,
       eac.each_earnings_price         产品配置农户收益json
FROM `xk-order`.`order` l
         INNER JOIN `xk-order`.order_expand oe ON l.order_no = oe.order_no
         INNER JOIN `xk-order`.exhibition_area_config eac ON l.exhibition_area_config_id = eac.id
WHERE l.is_delete = 0
and l.fund_id = 130
  AND l.control_oneself = 0
ORDER BY l.id DESC 

```


## 电站设计方案
eac.project_config
```json
[
  {
    "ageScheme": "",
    "checked": false,
    "designType": "阵列式",
    "projectClearnPrice": ""
  },
  {
    "ageScheme": "25",
    "checked": true,
    "designType": "阳光房",
    "projectClearnPrice": "1.43"
  },
  {
    "ageScheme": "",
    "checked": false,
    "designType": "庭院房",
    "projectClearnPrice": ""
  }
]
```


## 电站技术方案
eac.power_design_scheme
```json
[
  {
    "checked": false,
    "designType": "阵列式-普通方案",
    "projectClearnPrice": ""
  },
  {
    "checked": false,
    "designType": "阵列式-底梁方案",
    "projectClearnPrice": ""
  }
]
```



## 农户收益
不存在农户收益有值的数据