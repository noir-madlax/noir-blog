---
layout:     post
title:      "Data Authority"
subtitle:   "企业系统中的数据行权限"
# date:       2015-01-29 12:00:00
author:     "Noir"
tags:
    - java
---

> 最近项目中需要对行级别的数据做权限控制，于是就有了这篇文章。

### 需求： ###
根据登录用户的相关信息以及设定好的匹配规则，筛选出所需要的数据。


### 设计思路： ###

> 一开始设计时，思路一直停留在：什么谁能看？对返回的结果集进行二次过滤，效率方面有大问题。
>
> 然后把思路换成：谁能看什么之后，问题就很简单了

1.	程序启动时，缓存hb_row_expression表中的信息
2.	业务service保存http请求中的用户信息至线程变量（RowRightParameter）
3.	Mybatis拦截器根据线程变量判断是否需要拦截Sql
4.	获取调用方法所配置的所有表达式
5.	根据表达式与线程变量中保存的信息，按照顺序，逐一匹配表达式
6.	为第一条匹配的表达式拼接对应的sql语句
7.	执行sql并返回

表hb_row_expression

字段名称 | 含义 | 类型
----|------|----
Id | 权限id（序号）  | Int
Description | 说明  | Varchar
service_name | 类名  | Varchar
method_name	 | 方法名	 | Varchar（100）	
expression	 | 表达式	 | Varchar（2000）	
Sql	Where  | 条件	 | Varchar（2000）	
Sort | 排序	 | Int	

RowRightParameter对象中包含的信息：
1.	类名
2.	方法名
3.	用户id：userId
4.	部门id：userDepartmentid
5.	职位id：userPositionid
6.	角色id（数组）：roleId


### 问题、优化点： ###

在某方法内调用行权限服务初始化线程变量后，在该方法被释放前，拦截器会对该线程下所有的sql语句进行拦截，如有不需要进行拦截的语句，会出现错误。