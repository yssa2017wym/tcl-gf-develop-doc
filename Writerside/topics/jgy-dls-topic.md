# 极光云涉及产品配置

## 1. 代理商详情-查询展业区域

![jgy-dls-1.png](jgy-dls-1.png)

#### 1.1  /get/product/exhibition/data
```HTTP

https://sit2-jgyy-pv.tcl.com/api/web/product/config/get/product/exhibition/data

```
```json
{
  "exhibitionStatus": 0
}
```

`get/product/exhibition/data` 会通过`xk-web-server` 转发到  `/product/config/select/product/company/exhibition`

```java
    @PostMapping("get/product/exhibition/data")
    @ApiOperation("根据产品和项目公司查询展业区域配置详情")
    public RespDTO<Map<Long, List<ExhibitionAreaDTO>>> getProductCompExhibit(@RequestBody ExhibitionAreaDTO exhibitionCondition) {
        Predicate<ExhibitionAreaDTO> filter = condition -> {
            if (Objects.isNull(exhibitionCondition)) {
                return true;
            }
            boolean test = true;
            if (Objects.nonNull(exhibitionCondition.getExhibitionStatus())) {
                test = Objects.nonNull(condition.getExhibitionStatus()) && exhibitionCondition.getExhibitionStatus().equals(condition.getExhibitionStatus());
            }
            return test;
        };
        return RespDTO.success(DataDealUtil.listToMapList(xkProductClient.selectProductCompExhib(null, null)
                .pollDataElseThrow(), ExhibitionAreaDTO::getProductId, filter));
    }
```









#### 1.2 /admin/dealers/660/exhibition/area

```HTTP

https://sit2-jgyy-pv.tcl.com/api/web/admin/dealers/660/exhibition/area

```
```json
{
  "exhibitionStatus": 0
}
```


## 2. 新增/编辑展业区域

数据会落到代理商的库

#### 2.1 admin/dealers/472/exhibition/area
```HTTP

https://sit2-jgyy-pv.tcl.com/api/webadmin/dealers/472/exhibition/area

```






