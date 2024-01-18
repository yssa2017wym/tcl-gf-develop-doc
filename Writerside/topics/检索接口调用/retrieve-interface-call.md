# 检索接口调用

![image_13.png](image_13.png)



## 1. 全局检索日志

[搜索 | Splunk 8.0.5 (tclpv.com)](https://log.tclpv.com/zh-CN/app/search/search)

账号：pvlog

密码：pvlog



**示例指令**：

```shell
index="java" sourcetype="xk-order2" /fundInfo/queryByFundNameAndTenantKey
```


![image_14.png](image_14.png)


## 2. 登录服务检索日志



### 2.1 查看项目部署的服务节点

[Spring Boot Admin (tclpv.com)](http://admin.pv.tclpv.com/#/applications)

账号：admin

密码：admin123



### 2.2 登录云服务器堡垒机

[云堡垒机-华为 (tclpv.com)](https://ops.tclpv.com/#/desktop)



### 2.3 检索日志

**示例指令**：
```shell
cat /opt/deploy/xk-order2/logs/access/access_log.log |grep '/fundInfo/queryInfoByOrderNo' | head -n 5
```

<img src="image_15.png" alt="Alt text" />





