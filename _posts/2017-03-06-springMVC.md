---
layout: post
title:  "SpringMVC"
categories: java
tags:  java
author: zz
---

springmvc入门案例

1. 创建工程，导入依赖
2. web.mxl中配置核心控制器  Servlet
3. 创建 spring mvc 的配置文件
4. 编写控制器并使用注解配置
5. 5.测试

请求参数的 绑定

基本类型参数

		要求我们的参数名称必须和控制器中方法的形参名称保持一致

POJO  类型参数

		要求表单中参数名称和 POJO 类的属性名称保持一致。并且控制器方法的参数类型是 POJO 类型

数组和集合类型参数

		给 List 集合中的元素赋值，使用下标

		给 Map 集合中的元素赋值，使用键值对

常用注解

	@RequestMapping

		用于建立请求 URL 和处理请求方法之间的对应关系

	@RequestParam

		把请求中指定名称的参数给控制器中的形参赋值

	RequestBody

		用于获取请求体内容

	PathVaribale

		用于绑定 url 中的占位符，例如：请求 url 中 /delete/{id}，这个{id}就是 url 占位符

	CookieValue

		用于把指定 cookie 名称的值传入控制器方法参数

	ModelAttribute

		可以用于修饰方法和参数

			方法：出现在方法上，表示当前方法会在控制器的方法执行之前，先执行

			参数：出现在参数上，获取指定的数据给参数赋值

	SessionAttribute

		用于多次执行控制器方法间的参数共享`

响应数据和结果视图

返回值分类

返回字符串

			Controller方法返回字符串可以指定逻辑视图的名称，根据视图解析器为物理视图的地址

返回值是void

			使用 request 转向页面			

			也可以通过 response 页面重定向： 

			也可以通过 response 指定响应结果，例如响应 json 数据

			如果控制器的方法返回值编写成void，执行程序报404的异常，默认查找JSP页面没有找到

返回值是ModelAndView对象

			ModelAndView对象是Spring提供的一个对象，可以用来调整具体的JSP视图

    @RequestMapping("/testReturnModelAndView")
    public ModelAndView testReturnModelAndView() {
    	ModelAndView mv = new ModelAndView();
    	mv.addObject("username", "张三");
    	mv.setViewName("success");
    	return mv;
    }
    <body>
    	执行成功！
    	${requestScope.username}
    </body>



SpringMVC框架提供的转发和重定向

		forward请求转发

			return "forward:/user/findAll";

		redirect重定向

			return "redirect:/user/findAll";

springmvc实现异步

		ResponseBody响应json数据

			@ResponseBody

    @Controller("jsonController") 
    public class JsonController {    
    
    @RequestMapping("/testResponseJson")  
    public @ResponseBody Account testResponseJson(@RequestBody Account account) {   
      			System.out.println("异步请求："+account);   
     		 	return account;  
    	  } 
      } 

SpringMVC实现文件上传

文件上传的必要前提

A form 表单的 enctype 取值必须是：multipart/form-data      (默认值是:application/x-www-form-urlencoded)     enctype:是表单请求正文的类型 

B method 属性取值必须是 Post 

C 提供一个文件选择域<input type=”file” /> 

springmvc方式实现文件上传

	SpringMVC框架提供了MultipartFile对象，该对象表示上传的文件，要求变量名称必须和表单file标签的

name属性名称相同

	配置文件解析器 multipartResolver

SpringMVC跨服务器方式文件上传

	1.搭建图片服务器

2.实现SpringMVC跨服务器方式文件上传

SpringMVC中的异常处理

异常处理思路

	 Controller调用service，service调用dao，异常都是向上抛出的，最终有DispatcherServlet找异常处理器进行异常的处理。

SpringMVC的异常处理

	1.自定义异常类

	2.自定义异常处理器

    3. 配置异常处理器

SpringMVC框架中的拦截器

拦截器和过滤器的区别

		过滤器配置了可以拦截任何资源

		拦截器只会对控制器中的方法进行拦截

自定义拦截器步骤

		1.创建类，实现HandlerInterceptor接口，重写需要的方法

		2.在springmvc.xml中配置拦截器类

SSM整合

思路

Springmvc与spring整合

			配置spring的监听器

mybatis与spring整合

			将mybatis的配置交给spring编写数据源

			sessionFactroy

			mapper映射

			事务
