# TCL 金融

## 1. 获取所有未生成农户租金的订单

总计：54 条

```sql
SELECT o.order_no,
       o.order_stage,
       o.order_status,
       o.fund_name,
       o.age_scheme,
       o.fund_id,
       o.exhibition_area_config_id,
       od.eq_num,
       od.roof_type_num,
       od.roof_type_name,
       oe.power_design_scheme,
       oe.power_design_scheme_mixed,
       oe.product_type,
       oe.rent_date,
       eac.project_config      eacProjectConfig,
       eac.power_design_scheme eacPowerDesignScheme,
       eac.each_earnings_price eachEarningsPrice,
       opc.designing_scheme,
       opc.ps_technical_schemes,
       opc.ps_design_scheme
FROM `xk-order`.`order` o
         LEFT JOIN `xk-order`.order_devices od ON o.order_no = od.order_no AND eq_type = '光伏组件'
         LEFT JOIN `xk-order`.order_product_config opc ON o.order_no = opc.order_no AND opc.is_delete = 0
         LEFT JOIN `xk-order`.exhibition_area_config eac ON o.exhibition_area_config_id = eac.id
         LEFT JOIN `xk-order`.order_expand oe ON o.order_no = oe.order_no AND oe.is_delete = 0
         INNER JOIN (SELECT n.order_no
                     FROM `xk-asset`.pv_message_compensation n
                              LEFT JOIN `xk-asset`.pv_settle_order r ON n.order_no = r.order_no
                     WHERE r.product_name = 'TCL金融'
                       AND n.processed_flag = '0'
                       AND n.retry_scene = '农户租金'
                       AND n.is_delete = 0) t ON o.order_no = t.order_no
WHERE o.is_delete = 0;
```



### 1.1 无法生成农户租金的原因

- 产品系统 2.0 统一了农户租金结构，Tcl 金融属于产品 1.0 及之前，农户收益由前置系统维护，需要另外写一套解析逻辑。
- 无起租日

### 1.2 解决方案

1. 结算系统编写解析产品 2.0 之前的农户租金数据结构代码。
2. 订单系统准备 CURL 和相关接口通过导入的形式推送订单数据。
3. 无起租日的数据需要通知产品吴伊琳，由产品找业务给出具体的起租日期。

























