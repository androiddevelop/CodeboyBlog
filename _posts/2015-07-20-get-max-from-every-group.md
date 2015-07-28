---
layout: post
title: '获取所有分组中某列最大的行'
date: '2015-07-20'
header-img: "img/home-bg.jpg"
tags:
     - database
author: 'Codeboy'
---

**怎么获取所有分组中某列最大的行？**下面用一个例子来说明下：

一共公司有若干员工，每个员工有各自的id, group_id(部门), salary(工资).现在的问题转变为
	
	求公司各部门最高工资的员工
	
首先明确一个问题，一个部门的若干个员工可能同时拥有最高的工资，需要都列举出来。

看一下员工的数据库表结构(只包含有用的列):


| Field    | Type    | Null | Key | Default | Extra |
|----------|---------|------|-----|---------|-------|
| id       | int(11) | NO   | PRI | NULL    |       |
| group_id | int(11) | YES  |     | NULL    |       |
| salary   | int(11) | YES  |     | NULL    |       |

添加的测试数据如下:

| id | group_id | salary |
|----|----------|--------|
|  1 |    1     |    100 |
|  2 |    1     |    200 |
|  3 |    1     |    200 |
|  4 |    2     |    200 |
|  5 |    2     |    300 |

我们需要做的步骤如下:

- 1. 获取各个部门最高的工资
- 2. 查找各个部门工资等于最高工资的员工

#### 获取各个部门最高的工资
	
	select group_id, max(salary) as max_salary from employee group by group_id ;

执行后的结果:

| group_id | max_salary |
|----------|------------|
|        1 |        200 |
|        2 |        300 |

#### 查找各个部门工资等于最高工资的员工

	select a.id, a.group_id, a.salary from employee as a, b where a.group_id=b.group_id and a.salary=b.max_salary ;
	
>假设第一执行后的数据存在表b中。

这样就得到了最终的结果:

| id | group_id | salary |
|----|----------|--------|
|  2 |        1 |    200 |
|  3 |        1 |    200 |
|  5 |        2 |    300 |

我们可以简单的将**获取各个部门最高的工资**的代码替换b即可，组合后的语句如下:

	select a.id, a.group_id, a.salary from employee as a, (select group_id, max(salary) as max_salary from employee group by group_id) as b where a.group_id=b.group_id and a.salary=b.max_salary ;

执行后的结果相同。

#### 总结
	我们首先按照部门进行分组，获取每组最大的工资(表b); 之后将表a(原表)与表b做一下笛卡尔积，筛选出我们需要的数据即可。

> 如有任何知识产权、版权问题或理论错误，还请指正。
>
> 转载请注明原作者及以上信息。
