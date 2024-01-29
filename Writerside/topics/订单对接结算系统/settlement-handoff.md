# 订单对接结算系统

# 1. 结算调用订单接口

[结算调用订单接口 - 信息技术部 - Confluence (tclpv.net)](https://wiki.tclpv.net/pages/viewpage.action?pageId=12650572)

## 2. 订单提供的接口

| 代码                                                                                      | MQ                             | 备注                                      |
|-----------------------------------------------------------------------------------------|--------------------------------|-----------------------------------------|
| com.dycjr.xiakuan.order.service.order.OrderStatusChangeListener#orderStatusChangeHandle | MQSendEnum.ORDER_STATUS_CHANGE | 订单状态发生变更包含订单取消通知                        |
| com.dycjr.xiakuan.order.controller.OrderAuxiliaryController#pushOrderCancelToAssert     | MQSendEnum.ORDER_CANCEL_ASSERT | 手动推送已取消订单至结算系统                          |
| com.dycjr.xiakuan.order.service.OrderAuxiliaryService#pushOrderStatusToAssert           | MQSendEnum.ASSET_HISTORY_ORDER | 历史订单推送给结算系统生成农户租金                       |
| com.dycjr.xiakuan.order.controller.OrderAuxiliaryController#handleProjectProduct        |                                | 历史订单生成农户收益                              |
| com.dycjr.xiakuan.order.service.order.listener.OrderLoanListener#rentDateUpdateReceive  |                                | 起租日更新 （129光鑫宝-共富、131光瑞宝、133公共屋顶、134光悦宝） |
| com.dycjr.xiakuan.order.service.rentdate.RentDateService#handle                         |                                | 起租日更新 （115光鑫宝、128光盈宝）                   |
| com.dycjr.xiakuan.order.service.OrderService#queryOrderDetailToYX                       |                                | 起租日更新  （112光富宝、130光富宝-同裕）               |

## 3. 结算系统消费的MQ

https://wiki.tclpv.net/pages/viewpage.action?pageId=12651029