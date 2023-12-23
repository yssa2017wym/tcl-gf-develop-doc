# 浦银

## 订单系统

**XkFundPyClient**

- getElcPriceByOrderNo
- getElcPrice
- check
- getElcPrice



```java
@GetMapping({"/puyin/construction/getElcPriceByOrderNo"})
@ApiOperation("根据订单号获取城市电价信息")
RespDTO<List<CityProductDTO>> getElcPriceByOrderNo(@RequestParam("orderNo") String orderNo);
```

```java
@GetMapping({"/puyin/construction/getElcPrice/{cityCode}"})
@ApiOperation("获取城市电站相关单价")
RespDTO<CityNetPriceDTO> getElcPrice(@PathVariable("cityCode") String cityCode, @RequestParam("roofType") String roofType, @RequestParam(value = "fundVersion",required = false) String fundVersion);
```

```java
@PostMapping({"/puyin/trade/check"})
@ApiOperation("订单资方状态是否为已结清")
RespDTO<List<OrderTradeInfoRespDto>> check(@RequestBody TradeSubmitReqDto tradeSubmitReqDto);
```

```java
@PostMapping({"/puyin/trade/getElcPrice"})
@ApiOperation("批量查询资方最新融资单价")
RespDTO<List<OrderTradeInfoRespDto>> getElcPrice(@RequestBody TradeSubmitReqDto tradeSubmitReqDto);
```



## 浦银资方前置子系统



```java
xk-fund-puyin:

1、接口：/puyin/construction/getElcPrice/{cityCode}
   订单系统预审阶段查询融资单价计算是否超额

2、接口：/puyin/construction/getElcPriceByOrderNo
   订单系统获取产品配置信息（供结算系统使用）

3、接口：/puyin/online/contract/plan/code
   合同系统获取产品配置信息

4、踏勘通过后回调订单系统保存承租人收益
   回调接口：/fund/order/product/detail/save

5、踏勘通过后回调订单系统  pagekey:approveInfo
   内容：审批单价、fundVersion、农户收益(json)
   回调接口：/order/save

6、踏勘提交时校验融资单价是否存在，若存在传给资方netPrice字段

7、回购订单发起重推之前订单会获取最新融资单价进行更新 // 待定
   接口：/puyin/trade/getElcPrice

8、回购订单重推踏勘阶段时校验融资单价是否存在，若存在传给资方netPrice字段

9、预审通过后回调订单系统保存承租人收益
   回调接口：/fund/order/product/detail/save
```







