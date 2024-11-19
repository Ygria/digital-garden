---
date: " 2024-11-13"
tags: 
description: 
title: 
draft: false
---

Linux内核通过内核配置参数`kernel.pid_max`限制进程的数量，当运行的服务的总进程数超出`kernel.pid_max`的值时，再创建新进程时系统会报错`task: Cannot allocate memory`。

如何调整（ref：https://help.aliyun.com/zh/ecs/support/start-a-service-in-the-linux-system-prompt-task-always-the-allocate-memory-what-should-i-do）

1. 远程连接ECS实例。
    
    具体操作，请参见[连接方式概述](https://help.aliyun.com/zh/ecs/user-guide/connection-methods#concept-tmr-pgx-wdb)。
    
2. 执行以下命令，查看系统当前已运行的进程数是否大于最大进程数。
    
    - 查看系统当前已运行的进程数。
        
        ```1c
        ps -eLf | wc -l
        ```
        
    - 查看系统的最大进程数。
        
        ```nginx
        sysctl kernel.pid_max
        ```
        
    
    如果系统已运行的进程数大于最大进程数，请继续执行步骤[3](https://help.aliyun.com/zh/ecs/support/start-a-service-in-the-linux-system-prompt-task-always-the-allocate-memory-what-should-i-do#step-mfk-mql-0vi)。
    
3. 将命令中的`XXXX`修改为期望值，来调高`kernel.pid_max`的值。
    
    - 执行以下任意一条命令，临时设置（重启实例后会失效，需要重新设置）
        - `sysctl -w kernel.pid_max=XXXX`
        - `echo XXXX> /proc/sys/kernel/pid_max`
    - 永久设置（不受实例状态影响，一直有效）
        
        ```bash
        echo "kernel.pid_max=XXXX" >> /etc/sysctl.conf
        sysctl -p
        ```
        
    
    调高后，系统立即生效。


ps -eo pid,cmd,nlwp --sort=-nlwp | head -n 11