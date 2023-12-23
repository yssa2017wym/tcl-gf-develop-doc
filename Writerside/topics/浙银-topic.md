# 浙银


## 1. 订单系统

**XkFundZyClient**

- getInfo

```java
@PostMapping({"/city/product/getInfo"})
@ApiOperation("获取城市产品配置")
RespDTO<CityProductResp> getInfo(@RequestBody CityProductReq baseReqDto);
```

```java
@PostMapping({"/online/contract/plan/code"})
@ApiOperation("获取产品编号详情")
RespDTO<List<YXPrjProjectProductDTO>> obtainPlanCodeYXPC(@RequestBody @Validated BaseQueryConditionDTO baseReqDto);
```



## 2. 浙银资方前置子系统



```java
xk-fund-zy:
1、检查进件参数与历史进件参数是否一致 netPrice
   接口：/checkParams/checkSurveyParams
  
2、安装地址整改校验合同是否需要重签 netPrice
  
3、踏勘通过后回调订单系统  pagekey:approveInfo
   内容：审批单价、fundVersion、农户收益(json)
   回调接口：/order/save
  
4、踏勘通过后回调订单系统保存产品信息
   回调接口：/fund/order/product/detail/save
  
6、整改结果查询返回产品配置信息
   接口：/rars/result
  
7、踏勘提交时校验产品配置信息是否存在，若存在传给资方
  
8、接口：/city/product/getInfo
   获取产品配置信息
```



## 3. 浙银子系统的交互问题

### 3.1 预审阶段

1. WAIT_PRE_SUBMIT("待预审提交")
2. WAIT_FUND_PRE_APPROVAL("待资方预审审核"),
3. WAIT_FUND_PRE_APPROVAL_BACK("资方预审退回"),



说明：**待资方预审审核**节点会把**农户三要素**传递给资方进行审核

**农户三要素**：

- 身份证
- 手机号
- 姓名



**订单系统**：

​	**待资方预审审核**

​	调 `xk-channel-adapter` 

**浙银子系统**：

​	**农户三要素**传递给资方进行审核



### 3.2 踏勘阶段

1. 

2. ```
   WAIT_SUR_RECEIVE("待踏勘领单")
   WAIT_SUR_DISPATCH("待踏勘派单", 5)
   WAIT_SUR_SUBMIT("待踏勘提交", 7)
   WAIT_SUR_CHECK("待踏勘检查", 8)
   ```





### 3.3 物流阶段



### 3.4 施工阶段



### 3.5 并网阶段
