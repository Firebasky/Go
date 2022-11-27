# Go安全

> Go语言本来就比较安全hhh,介绍一些go相关的安全知识和安全开发，学习中ing 
> 跟着大哥atao学习。
## Go 学习

+ [Go语言的IO库那么多，我该怎么选？](https://mp.weixin.qq.com/s/TtN6NZ8hQ2AIf0C8wVzkjA)

## Go web
简单的了解了gin框架,小类型的demo

[bubble](https://github.com/Q1mi/bubble)

## 代码审计
- [Go语言代码安全审计分享](https://www.freebuf.com/articles/web/224363.html)

```
路径穿越写计划任务/etc/cron.hourly/shell getshell
```
- [Go语言代码审计实战](https://mp.weixin.qq.com/s/u0lfUbemtXpgy-EQsqa5tw)

## 框架漏洞
- [Grafana 任意文件读取](./框架/Grafana/)
- [APISIX Dashboard 未授权](./框架/apisix/)
- [CVE-2021-21287: 容器与云的碰撞——一次对MinIO的测试](https://www.leavesongs.com/PENETRATION/the-collision-of-containers-and-the-cloud-pentesting-a-MinIO.html) **其中对ssrf处理非常好 (go中的302/307)ssrf->rce**

- [The arbitrary file upload vulnerability caused by path traversal is on github.com/flipped-aurora/gin-vue-admin](https://github.com/flipped-aurora/gin-vue-admin/security/advisories/GHSA-wrmq-4v4c-gxp2)

## 安全工具

+ [Golang爬虫框架初探](https://www.sec-in.com/article/1819) **可能之后写工具有用**

自己写的rmi利用防止反置问题 [GoRmi](https://github.com/Firebasky/GoRmi)



