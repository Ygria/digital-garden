---
date: ' 2026-04-21'
tags: 
description: 
title: 
draft: false
keywords: []
---
1. Docker 查看容器 pid

```bash
docker inspect --format '{{.State.Pid}}' <容器名或ID>
```


```shell
docker inspect --format '{{.State.Pid}}' aihis-tps-ppt-render-service
```



配置触发逻辑：

**在阿里云的** Webhook 配置中进行配置 ：

```
http://47.94.248.181:18085/jenkins-webhook/tps-server
```

以及：
```
http://47.94.248.181:18085/jenkins-webhook/tps-ui
```


Token 填写： `tps-ui-test-deploy`


Optional filter 处填写：

Expression:
```
$ref
```

Text

```
^refs/heads/dev-tps$
```