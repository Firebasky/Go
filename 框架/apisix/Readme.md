# apisix

Apache APISIX 是一个动态、实时、高性能的API 网关， 提供负载均衡、动态上游、灰度发布、服务熔断、身份认证、可观测性等丰富的流量管理功能。 你可以使用Apache APISIX 来处理传统的南北向流量，以及服务间的东西向流量， 也可以当做k8s ingress controller 来使用

出现的漏洞是apisix-dashboard的未授权漏洞，并且可以rce

## 漏洞原理

[CVE-2021-45232 Apache APISIX Dashboard 认证绕过漏洞分析](https://www.wangan.com/p/7fy747719a2907e1)

**实际上就是/apisix/admin/migrate/export,/apisix/admin/migrate/import 没有做权限验证**

## rce


## 思考

![image](https://user-images.githubusercontent.com/63966847/148041426-1a419562-5583-4c07-bbe2-887059392e5a.png)
