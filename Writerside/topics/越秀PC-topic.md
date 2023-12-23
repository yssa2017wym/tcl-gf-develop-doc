#越秀PC

## 订单系统

**XkFundYxClient**

- obtainPlanCode
- surveySubmit



```java
@PostMapping({"/survey/submit"})
@ApiOperation("踏勘进件")
RespDTO<SurveyResp> surveySubmit(@RequestBody @Validated BaseQueryConditionReq baseReqDto);
```

```java
@PostMapping({"/online/contract/plan/code"})
@ApiOperation("获取产品编号详情")
RespDTO<List<YXPrjProjectProductDTO>> obtainPlanCodeYXPC(@RequestBody @Validated BaseQueryConditionDTO baseReqDto);
```



## 越秀PC资方前置子系统



```java
xk-fund-yxpc:

1、接口：/online/contract/plan/code
   合同系统获取产品配置信息
  
2、踏勘提交时校验产品配置信息是否存在，若存在传给资方
  
3、踏勘通过后回调订单系统  pagekey:approveInfo
   内容：审批单价、fundVersion、农户收益(json)
   回调接口：/order/save
```







