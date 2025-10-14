---
date: ' 2025-10-09'
tags: 
description: 
title: 
draft: false
keywords: []
---
```sql
SELECT d.department_name, AVG(e.salary) AS avg_salary FROM employees e JOIN departments d ON e.department_id = d.department_id WHERE e.hire_date > '2022-01-01' GROUP BY d.department_name HAVING COUNT(e.employee_id) > 5 ORDER BY avg_salary DESC LIMIT 10;
```

### Step 1：`FROM` 和 `JOIN` 

第一步，执行器将所有关联的表数据和联出的表数据，在内存或临时文件中，形成一张巨大的虚拟表（笛卡尔积），这个表包含了 `employees` 表和 `departments` 表的所有

制约瓶颈：
1. 索引
2. 连接条件

**如果联表查出的数据量过大，会导致后续所有操作的计算量都指数增长。**

## Step 2： 

