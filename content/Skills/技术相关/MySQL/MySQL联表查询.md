---
title: MySQL联表查询
date: 2024-08-29T12:02:00
tags:
  - MySQL
---

`JOIN` 和 `LEFT JOIN` 都是用于将多张表的数据结合在一起的 SQL 语句，但它们之间存在一些关键区别。
### 1. `JOIN`（也叫 `INNER JOIN`）

- `JOIN` 或 `INNER JOIN` 会返回两张表中**匹配的行**。
- 只有当连接条件（通常是相等条件）匹配时，才会返回结果。
- 如果某一张表中的某一行没有在另一张表中找到匹配的行，该行将不会出现在结果集中。

**示例：**

假设有两个表 `A` 和 `B`：
```sql
SELECT *  FROM A  JOIN B ON A.id = B.a_id;
```
结果：只返回在 `A` 表和 `B` 表中都有匹配 `id` 的行。
### 2. `LEFT JOIN`（也叫 `LEFT OUTER JOIN`）

- `LEFT JOIN` 会返回左表（`A` 表）中的**所有行**，即使右表（`B` 表）中没有匹配的行。
- 如果右表中没有匹配的行，则结果集中右表的列将显示为 `NULL`。

**示例：**

```sql
SELECT *  FROM A  LEFT JOIN B ON A.id = B.a_id;
```

结果：返回 `A` 表中的所有行。如果 `B` 表中有与 `A` 表匹配的行，则显示这些行；如果没有匹配，则显示 `NULL`。

### 主要区别总结：

- **`JOIN`（`INNER JOIN`）**: 只返回两张表中都有匹配数据的行。
- **`LEFT JOIN`（`LEFT OUTER JOIN`）**: 返回左表中的所有行，即使右表中没有匹配的行（未匹配的部分用 `NULL` 填充）。

选择哪种 JOIN 取决于你的业务需求：如果你只需要匹配的数据，使用 `JOIN`；如果你需要左表的所有数据，并且包含右表中的匹配项，使用 `LEFT JOIN`。

联表可能会导致出现重复记录，使用[[MySQL distinct语句]]去重。
