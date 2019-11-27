# 第一章、Spring的基本应用

## 1.1 Spring概述

### 1.1.1 什么是Spring

Spring是一个分层的 JavaSE/JavaEE full-stack（一站式）轻量级开源框架。以：

- Ioc （Inversion of Control，控制反转）
- AOP（Aspect Oriented Programming，面向切面编程）

为内核。

### 1.1.2 Spring 优点

- 非侵入式设计
- 方便解耦，简化开发
- 支持AOP
- 支持声明式事务处理
- 方便程序的测试
- 方便集成各种优秀框架
- 降低 Java EE API 的使用难度

### 1.1.3 Spring 的体系结构

介绍以下 3 模块（层）：

#### 1.Core Container （核心容器层）

Core Container（核心容器）是其他模块建立的基础，它主要由以下 5 种模块组成：

##### 1.Beans 模块

###### 2. Core 核心模块

##### 3. Context 上下文模块

##### 4.Context-support 模块

##### 5. SpEL 模块

#### 2. Data Access/Integration （数据访问/集成层）

数据访问/集成层包括以下 5 个模块：

##### 1.JDBC 模块

##### 2. ORM模块

##### 3.OXM模块

##### 4.JMS模块

##### 5. Transactions 事务模块

#### 3.Web层

Web 层主要包括以下 4 个模块：

##### 1.WebSocket 模块

##### 2. Servlet 模块

##### 3. Web模块

##### 4. Portlet 模块

#### 4. 其他模块

除了上面 3 大模块，Spring 还有以下模块

- AOP 模块
- Aspects 模块
- Instrumentation 模块
- Messaging 模块
- Test 模块

###  1.1.4 Spring 的下载及目录结构

（本文使用Spring 版本为：Spring 4.3.6）

Spring 开发所需 Jar 包分为两部分：

#### 1. Spring 框架包

我们下载 Spring 4.3.6 版本的框架压缩包后可以看到目录如下：

- docs 文件夹中包含 Spring 的 API 文档和开发规范
- libs 文件夹中包含开发所需要的 JAR 包和源码 
- schema 文件夹中包含开发所需要的 schema 文件，这些文件定义了 Spring 相关文件的约束。

其中在libs目录下， Jar包分为3类 （共60 个 JAR，每种20个）：

- RELEASE.jar 结尾的：是Spring框架class文件的 Jar 包。 
- RELEASE-javadoc.jar 结尾的：是Spring 框架 API 文档的压缩包
- RELEASE-sources.jar 结尾的：是 Spring 框架源文件的压缩包

其中有 `4 个 Spring 基础包`。分别对应 Spring 核心容器的四个模块：

1. spring-core-*.RELEASE.jar : 包含 Spring 核心工具类，是其他组件的基本核心， Spring本身其他组件也都要用到这个包里的类。
2. spring-beans-*.RELEASE.jar : 所有应用都要用到的 JAR 包，包含 访问配置文件、创建和管理 Bean 以及进行 IoC 或 DI操作相关的所有类。 
3. spring-context-*.RELEASE.jar : 提供各种扩展服务以及各种视图层框架的封装
4. spring-expression-*.RELEASE.jar : 定义 Spring 的表达式语言

#### 2. 第三方依赖包

除了自带的 4 个基础包外，还需要依赖一个 `commons.logging ` JAR 包。



> 因此搭建一个基础的 Spring 程序只要 `5` 个 JAR 包就可以完成，其中 4 个 Spring 基础包和一个 第三方依赖包。

## 1.2 Spring 的核心容器

 