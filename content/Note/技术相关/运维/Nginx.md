---
dg-publish: true
title: nginx
dg-created: " 2024/08/29 14:56:25"
tags: 
dg-path: Note/技术相关/运维/20240829145625
dg-note-icon: "1"
uid: 20240829145625
dg-permalink: 20240829145625
---
# 启动和重启
进入sbin目录，运行nginx文件即可。

重启（不中断进行，重新载入配置文件）

`nginx -s reload -c nginx.conf`

