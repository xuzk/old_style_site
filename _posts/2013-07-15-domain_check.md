---
layout: post
title: 从多级域名中提取主域并验证有效性
---

# 起源

需求起源来自于一个用户自定义域名绑定bucket空间的自动化过程。

整个自定义域名绑定bucket空间的流程：

    客户发起自定义域名(例如img.coaku.com)和bucket空间coaku的绑定操作
    ->
    后台管理系统收到绑定请求，记录需要绑定的域名信息
    ->
    人工从img.coaku.com中提取出主域`coaku.com`和泛二级域`.coaku.com`，提交给CDN进行缓存设置
    ->
    CDN缓存设置完成后，回复设置成功的邮件
    ->
    利用alibench.com对缓存设置进行测试，测试全国各地对coaku.com和.coaku.com的可访问性
    ->
    从CDN回复的邮件中获取别名切换的链接，将其通知给客户
    ->
    客户将coaku.com的CNAME指向CDN返回的别名，设置完成


经过以上的设置之后，域名img.coaku.com的解析流程将会变成这样：

    dns query `img.coaku.com`: get CNAME to `coaku.qiniudn.com.`

    dns query `cname.domain.in.cdn`: get CNAME to `coaku.qiniudn.com.`
