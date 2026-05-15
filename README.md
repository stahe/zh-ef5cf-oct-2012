# 从 NHibernate 迁移至 Entity Framework 5（2012 年 10 月）

本文介绍了一种**灵活且可扩展的 ASP.NET 应用程序架构**，
并演示了如何在不修改应用程序层的情况下，
将 ORM **NHibernate** 替换为 **Entity Framework 5**。

🌐 相关网站可通过以下地址访问：https://stahe.github.io/zh-ef5cf-oct-2012/

---

## 背景信息

**Entity Framework** 是一种 ORM（对象关系映射器），最初由微软创建，
并于 2012 年 7 月开源。

在 ASP.NET 课程中，本文基于分层架构，
该架构允许在不影响应用程序的情况下更新技术（ORM、DBMS）。

---

## 总体架构

下图展示了该应用程序采用的架构：

![基于 NHibernate 和 Spring.NET 的 ASP.NET 架构](https://stahe.github.io/ef5cf-oct-2012/images/10000000000007D200000183315F4E40.png)

![基于 Entity Framework 5 和 Spring.NET 的 ASP.NET 架构](https://stahe.github.io/ef5cf-oct-2012/images/10000000000007D7000001825B1CF7DD.png)

### 层级描述

- **ASP.NET 应用程序**  
  表示层和业务逻辑层。

- **DAO（数据访问对象）**  
  应用程序使用的数据访问接口。

- **ORM（NHibernate / Entity Framework）**  
  负责生成 SQL 语句并与 ADO.NET 进行通信。

- **ADO.NET**  
  数据库管理系统（DBMS）连接器。

- **DBMS**  
  数据库管理系统。

- **Spring.NET**  
  确保各层级集成与依赖注入。

---

## 为什么要使用 ORM？

将 DAO 层直接连接到 ADO.NET 会导致应用程序依赖于 DBMS：
- 数据类型差异；
- 专有 SQL；
- 特定于 DBMS 的库。

使用 ORM 时，更换 DBMS 实质上等同于 **修改 ORM 的配置**。
DAO 层保持不变。

---

## Spring.NET 的作用

Spring.NET 允许：

- ASP.NET 应用程序获取对 DAO 层的引用；
- 通过配置文件创建该层；
- **无需修改代码**即可用另一种 DAO 实现替换现有实现，
  前提是接口保持不变。

---

## 本文目的

通过实践证明该架构：

- **能够适应 DBMS 的变更**；
- **能够适应 ORM 的变更**；
- **允许将 NHibernate 替换为 Entity Framework 5**，
  且无需修改 ASP.NET 应用程序层。

---

## 采用的方法

迁移分几个阶段进行：

1. 探索 **Entity Framework 5** 与不同 DBMS 的兼容性；
2. 创建新的数据访问层（**DAO2**）；
3. 将现有的 ASP.NET 应用程序连接到这个新的 DAO 层。

---

## 目标受众
- ASP.NET 开发人员
- 软件架构领域的学生和教师
- 任何对解耦和可扩展架构感兴趣的人士
---
## 许可与使用
本教学文档旨在用于教学和演示可扩展的应用程序架构。
