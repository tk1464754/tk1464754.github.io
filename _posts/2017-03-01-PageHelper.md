---
layout: post
title:  "Pagehelper"
categories: java
tags:  java
author: zz
---



（1）在pom.xml文件中引入依赖库

<!-- 分页插件 -->
		<dependency>
			<groupId>com.github.pagehelper</groupId>
			<artifactId>pagehelper-spring-boot-starter</artifactId>
			<version>1.2.3</version>
		</dependency>
--------------------- 
（2）在application.properties中添加分页配置

# 配置pageHelper分页插件的内容
pagehelper.helper-dialect=mysql
pagehelper.reasonable=true
pagehelper.support-methods-arguments=true
pagehelper.params=count=countSql

或者在application.yml文件中添加分页配置
pagehelper:
 helperDialect: mysql
 reasonable: true
 supportMethodsArguments: true
 params: count=countSql
--------------------- 
（3）进行使用。（可以在controller层或者service层使用即可）

/**
     * 查询所有的person内容
     * @return
     */
    @RequestMapping(value = "/list")
    public String jumpJsp(Map<String, Object> result){
        PageHelper.startPage(3 , 3);
        List<Person> personList = personService.findPerson();
        //得到分页的结果对象
        PageInfo<Person> personPageInfo = new PageInfo<>(personList);
        //得到分页中的person条目对象
        List<Person> pageList = personPageInfo.getList();
        //将结果存入map进行传送
        result.put("pageInfo" , pageList);
        return "person_list";
    }
--------------------- 
解析：

（1）PageHelper.startPage（pageNum ， pageSize），这个方法就是类似我们数据库操作的limit start ， count

（2）得到的对象PageInfo里面包含很多的字段信息，这个可以自己看源码，非常详细

（3）如果我们只想得到分页处理之后我们的实体对象的结果，那么就调用PageInfo对象的getList（）方法即可。

（4）这种配置使用的方式是最通用的方式，也就是对于环境搭建不同方式都可以利用这种使用方法。

问题：如果运行时出现，org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'com.github.pagehelper.autoconfigure.PageHelperAutoConfiguration': 这个错误。

解决办法：这是由于分页插件pagehelper的版本和mybatis不兼容的原因，修改分页插件的版本即可。
--------------------- 
