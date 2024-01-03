# 合同系统

此处介绍一些走单过程种，合同服务常见的问题和解决方案。

> **Highlight important information**
>
> You can change the element to *tip* or *warning* by renaming the style attribute below.
>
{style="note"}

## 签署印章与主体不匹配

说明：这个错误信息是e签宝第三方平台返回的，项目公司的印章和项目公司主体不匹配就会抛出！

常见原因:
- 项目公司的印章id配置错误
- 测试环境对接的是e签宝测试环境，而项目公司印章是生产环境的
- 项目公司没有配置印章id
- 项目公司信息错误
- 合同服务模板表配置的印章id错误

### 如何解决？

测试环境项目公司印章映射关系表



|      |      |      |      |      |
| ---- | ---- | ---- | ---- | ---- |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |
|      |      |      |      |      |








1. 查询项目公司表的印章id

   ```SQL
   SELECT pc.id,
          pc.company_name,
          pc.seal_id AS e签宝印章id
   FROM product_company pc
   WHERE pc.is_delete = 0
     AND pc.company_name = ?
     AND pc.id = ?;
   ```

2. 查询订单对应的项目公司信息（合同签署传的是订单的项目公司的信息）


   - **注意：产品系统2.0升级后，订单绑定的项目公司信息是有版本的。**

   ```sql
   SELECT opc.order_no,
          opc.product_id         产品id,
          opc.control_oneself    是否自持产品,
          opc.project_company_id 项目公司id,
          opc.project_company AS 项目公司json串
   FROM `xk-order`.order_product_config opc
   WHERE opc.is_delete = 0
     AND opc.order_no = ?;
   ```

3. Step with a [link](https://www.jetbrains.com)

   

4. Step with a list.
   - List item
   - List item
   - List item