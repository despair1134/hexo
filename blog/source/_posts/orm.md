
---
title: ORM简介
author: despair
avatar: http://127.0.0.1:8000/static/upload/touxiang.jpg
authorLink: despair1134
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
date: 2020-4-5 11:21
comments: true
tags: 
 - web
 - orm
keywords: Sakura
description: ORM
---


<video src='http://q9hs8ny3b.bkt.clouddn.com/e81199b057d1b3484a87d6544cd49376.mp4' type='video/mp4' controls='controls'  width='100%' height='100%'>
</video>



<h2>orm(对象关系映射)


 - ORM（Object Relational Mapping）框架采用元数据来描述对象一关系映射细节，元数据一般采用XML格式，并且存放在专门的对象一映射文件中。

 - 只要提供了持久化类与表的映射关系，ORM框架在运行时就能参照映射文件的信息，把对象持久化到数据库中。当前ORM框架主要有五种：Hibernate(Nhibernate)，iBATIS，mybatis，EclipseLink，JFinal。

 - ORM是通过使用描述对象和数据库之间映射的元数据,在我们想到描述的时候自然就想到了xml和特性(Attribute).目前的ORM框架中,Hibernate就是典型的使用xml文件作为描述实体对象的映射框架,而大名鼎鼎的Linq则是使用特性(Attribute)来描述的。


<h2>什么是ORM



<h3>即Object-Relationl Mapping，它的作用是在关系型数据库和对象之间作一个映射，这样，我们在具体的操作数据库的时候，就不需要再去和复杂的SQL语句打交道，只要像平时操作对象一样操作它就可以了 。

<h2>为什么要做持久化和ORM设计(重要)


在目前的企业应用系统设计中，MVC，即 Model（模型）- View（视图）- Control（控制）为主要的系统架构模式。MVC 中的 Model 包含了复杂的业务逻辑和数据逻辑，以及数据存取机制（如 JDBC的连接、SQL生成和Statement创建、还有ResultSet结果集的读取等）等。将这些复杂的业务逻辑和数据逻辑分离，以将系统的紧耦 合关系转化为松耦合关系（即解耦合），是降低系统耦合度迫切要做的，也是持久化要做的工作。MVC 模式实现了架构上将表现层（即View）和数据处理层（即Model）分离的解耦合，而持久化的设计则实现了数据处理层内部的业务逻辑和数据逻辑分离的解耦合。 而 ORM 作为持久化设计中的最重要也最复杂的技术，也是目前业界热点技术。



简单来说，按通常的系统设计，使用 JDBC 操作数据库，业务处理逻辑和数据存取逻辑是混杂在一起的。
一般基本都是如下几个步骤：

 - 1、建立数据库连接，获得 Connection 对象。

 - 2、根据用户的输入组装查询 SQL 语句。

 - 3、根据 SQL 语句建立 Statement 对象 或者 PreparedStatement 对象。

 - 4、用 Connection 对象执行 SQL语句，获得结果集 ResultSet 对象。

 - 5、然后一条一条读取结果集 ResultSet 对象中的数据。

 - 6、根据读取到的数据，按特定的业务逻辑进行计算。

 - 7、根据计算得到的结果再组装更新 SQL 语句。

 - 8、再使用 Connection 对象执行更新 SQL 语句，以更新数据库中的数据。

 - 7、最后依次关闭各个 Statement 对象和 Connection 对象。

 - 举例来说，比如要完成一个购物打折促销的程序，用 ORM 思想将如下实现,业务逻辑如下：

```

public Double calcAmount(String customerid, double amount) 
{
    // 根据客户ID获得客户记录
    Customer customer = CustomerManager.getCustomer(custmerid); 
    // 根据客户等级获得打折规则
    Promotion promotion = PromotionManager.getPromotion(customer.getLevel()); 
    // 累积客户总消费额，并保存累计结果
    customer.setSumAmount(customer.getSumAmount().add(amount); 
    CustomerManager.save(customer); 
    // 返回打折后的金额
    return amount.multiply(protomtion.getRatio()); 
}

```


