---
date: " 2024-08-30"
tags:
  - 运维
  - 技术
description: 
title: 
draft: false
---
```bash
docker run -d --privileged=true -p 6379:6379 --restart always -v /home/theblind/redis/conf/redis.conf:/etc/redis/redis.conf -v /home/theblind/redis/data:/data --name myredis redis redis-server /etc/redis/redis.conf --appendonly yes
```


```
docker run -d --privileged=true -p 6379:6379 --restart always -v /home/beavers2/redis/conf/redis.conf:/etc/redis/redis.conf -v /home/beavers2/redis/data:/data --name myredis redis redis-server /etc/redis/redis.conf --appendonly yes
```