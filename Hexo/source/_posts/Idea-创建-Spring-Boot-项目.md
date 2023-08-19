---
title: Idea 创建 Spring Boot 项目
date: 2023-08-19 10:44:18
tags: [Java, Spring Boot]
---

# Context

这篇文章介绍通过 Idea 快速创建 Spring Boot 项目

# 创建过程

## 1. create new project

![create new project](https://img.inkseeker.com/i/36455e8b242381e0214a3b8a4d25ae6c/64e02d97111e1.webp)

## 2. 设置项目名称

- Name: 一个对于用户更为友好的项目名称, 项目创建后本地项目文件夹名就是这个 name
- Location: 本地存储目录
- Type: 用哪种构建工具, Gradle or Maven
- Group: 企业或团队的唯一标识，定义了项目属于哪个组/团队, `域.企业or团队名`
- Artifact: 项目的唯一的标识符，实际对应项目的名称，就是项目根目录的名称

举例:

Apache 公司的 Tomcat 项目：这个项目的 groupId 是 org.apache，它的域是 org，公司名称是 apache，artifactId 是 tomcat。


![设置项目名称](https://img.inkseeker.com/i/007bf3f856d0a1627b08ef0c44396b7d/64e02e1f90d4e.webp)

## 3. 选择项目依赖, 并创建

在 Dependencies 列表中选择项目依赖, 这里以 Web 开发后端为例, 选择了下面的依赖:

- Spring Boot DevTools: 可以实现本地更新后热加载
- Lombok: 建华 Java 对象 (POJO) 的使用, 不需要写一对 getter, setter 等样板式代码
- Spring Web: 简化 Web 开发, 比如集成 Web MVC, RESTful Web 服务等
- Spring DataJPA: 简化对数据库的访问和操作
- MySQL Driver: MySQL 官方 JDBC 驱动程序，连接 Java 程序和 MySQL 数据库

![Alt text](https://img.inkseeker.com/i/8b4ee64449b9da285abfd9aac9762a37/64e02e2d307ae.webp)

依赖选择完之后, 点击创建就 Okay 啦!
