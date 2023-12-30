# 产品配置表历史数据初始化

## 1. 目前生产环境的电站方案

| 设计方案    | 技术方案                       | 产品    | 备注                      |
|---------|----------------------------|-------|-------------------------|
| 阵列式     | 阵列式-普通方案、阵列式-双坡方案、阵列式-底梁方案 |       |                         |
| 阳光房     | 暂时没有技术方案（默认：**阳光房-普通方案**）  |       |                         |
| 庭院房     | 庭院房-普通方案、庭院房-大桁架           |       |                         |
| 阵列式+庭院房 | 取设计方案下的技术方案集合              | 5号    | 踏勘和施工阶段需要分摊计算审批单价、工程款单价 |
| 阵列式+阳光房 | 取设计方案下的技术方案集合              | 5号、8号 | 踏勘和施工阶段需要分摊计算审批单价、工程款单价 |

## 2. 历史数据处理流程图

- 有技术方案 农户收益和价格全部从 `exhibition_area_config`表的`power_design_scheme`字段中取
- 无技术方案 农户收益从`each_earnings_price`字段中取，价格从`project_config`取，并补齐技术方案模式值：xxx-普通方案
- 工程款结算单价阶梯数据会进行初始化，默认值为0-50000
- 非自持产品无农户收益
- 自持产品预审阶段确定设计方案和技术方案
- 非自持产品预审阶段确定设计方案，踏勘阶段确定技术方案
- 从拓展表获取相关字段赋值到`order_product_config`表

<note>
本质上就是把order、order_expand、exhibition_area_config表数据合并成order_product_config表数据
</note>



<img src="order_product_config_his_convert.png" alt="Convert table to XML" width="706" border-effect="line"/>

| order           | order_expand                                     | exhibition_area_config                                                                       | order_product_config                           | 优先级   | 备注                                                                       |
|-----------------|--------------------------------------------------|----------------------------------------------------------------------------------------------|------------------------------------------------|-------|--------------------------------------------------------------------------|
| order_no        |                                                  |                                                                                              | order_no                                       |       |                                                                          |
| fund_id         |                                                  |                                                                                              | product_id （产品id）                              |       |                                                                          |
| control_oneself |                                                  |                                                                                              | control_oneself  （1-自持 0-非自持）                  |       |                                                                          |
|                 |                                                  |                                                                                              | install_capacity （安装容量）                        |       |                                                                          |
| roof_type       | roof_type                                        |                                                                                              | ps_design_scheme   （电站设计方案）                    | order |                                                                          |
|                 | power_design_scheme                              |                                                                                              | ps_technical_scheme     （电站技术方案）               |       |                                                                          |
|                 | power_design_scheme_mixed、power_design_scheme    |                                                                                              | ps_technical_schemes    （电站技术方案集合json串）        |       |                                                                          |
| age_scheme      | earnings_deadline                                |                                                                                              | age_scheme      （年限方案）                         |       |                                                                          |
|                 | payment_cycle     (支付周期)                         |                                                                                              | age_scheme      （年限方案）                         |       |                                                                          |
|                 | payment_date                (支付日期)               |                                                                                              | age_scheme      （年限方案）                         |       |                                                                          |
|                 | each_earnings_price               (每期收益单价(元/块/期) |                                                                                              | age_scheme      （年限方案）                         |       |                                                                          |
|                 | power_rate_foot_method                           |                                                                                              | fee_settle_method     （结算方式）                   |       |                                                                          |
|                 |                                                  |                                                                                              | is_mixed          （设计方案是否混装 0-非混装 1-混装）        |       |                                                                          |
| approve_price   | approve_price                                    |                                                                                              | approve_price       （审批单价）                     | order |                                                                          |
| approve_price   | net_price                                        |                                                                                              | net_price                 （造电价）                | order |                                                                          |
| approve_price   | power_station_financing                          |                                                                                              | power_station_financing          （电站融资单价(元/W)） | order |                                                                          |
|                 | equipment_suit                                   |                                                                                              | equipment_suit      （集采设备套装）                   |       |                                                                          |
|                 | project_clearn_price                             |                                                                                              | project_clearn_price                 （工程款结算单价） |       |                                                                          |
|                 | project_warranty_price                           |                                                                                              | project_warranty_price     （工程款质保金额）           |       |                                                                          |
|                 | product_company_name                             | product_company_id                                                                           | project_company_id （项目公司id）                    |       |                                                                          |
|                 |                                                  |                                                                                              | project_company （项目公司信息json)                   |       | 根据product_company_id查询product_company                                    |
|                 |                                                  |                                                                                              | bank_config             (资方产品配置信息json)         |       | 2.0新增字段，历史数据不做处理，新增资方交互订单会有该字段                                           |
|                 |                                                  | project_config、power_design_scheme、power_rate_foot_method、each_earnings_price、equipment_suit | designing_scheme   (产品配置信息json)                |       | 1.由这5个字段合并成2.0的新结构。2.无技术方案补充-xxx-普通方案。3.有技术方案不需要用到each_earnings_price字段。 |

### 2.1 涉及库表和相关字段

#### 2.1.1 **订单产品配置表**

```sql
SELECT *
FROM order_product_config opc
WHERE opc.is_delete = 0;
```

#### 2.1.2 **产品配置表** （1.0）

```sql
SELECT *
FROM exhibition_area_config eac
WHERE eac.is_delete = 0
```

#### 2.1.3 **订单拓展表**

```sql
SELECT id,
       order_no,
       net_price,
       roof_type,
       project_config,
       power_rate_foot_method,
       project_clearn_price,
       project_warranty_price,
       power_station_financing,
       earnings_deadline,
       payment_cycle,
       payment_date,
       each_earnings_price,
       equipment_suit,
       product_id,
       approve_price,
       power_design_scheme,
       product_company_name,
       power_design_scheme_mixed
FROM order_expand oe
WHERE oe.is_delete = 0;
```

#### 2.1.4 订单表

```sql
SELECT id,
       order_no,
       order_stage,
       order_scene,
       order_status,
       fund_name,
       fund_id,
       product_code,
       approve_price,
       unit_price,
       control_oneself,
       exhibition_area_config_id,
       roof_type,
       age_scheme
FROM `order` o
WHERE o.is_delete = 0;
```

## 3. 接口初始化指定订单产品配置数据

- 不存在则插入
- 存在则更新

### 3.1 接口信息

POST

http://10.74.183.21:30019/order/product/config/handleHis

Body

```json
{
  "orderNoList": [
    "GF231216174055000897"
  ]
}
```

<note>
10.74.183.21:30019 是测试3环境订单服务ip和端口
<p>注意：订单号必传</p>
</note>

## CURL调用

<tabs>
    <tab title="linux">
        <code-block lang="plain text">
curl --location --request POST 'http://10.74.183.21:30019/order/product/config/handleHis' \
--header 'User-Agent: Apifox/1.0.0 (https://apifox.com)' \
--header 'Content-Type: application/json' \
--header 'Accept: */*' \
--header 'Host: 10.74.183.21:30019' \
--header 'Connection: keep-alive' \
--data-raw '{
    "orderNoList": [
        "GF231216174055000897"
    ]
}'
        </code-block>
    </tab>
    <tab title="windows">
        <code-block lang="xml">
      curl --location --request POST "http://10.74.183.21:30019/order/product/config/handleHis" ^
--header "User-Agent: Apifox/1.0.0 (https://apifox.com)" ^
--header "Content-Type: application/json" ^
--header "Accept: */*" ^
--header "Host: 10.74.183.21:30019" ^
--header "Connection: keep-alive" ^
--data-raw "{    \"orderNoList\": [        \"GF231216174055000897\"    ]}"
    </code-block>
    </tab>
</tabs>
