---
title: javaWeb
date: 2022-09-27 21:14:37
tags: 技术
---

http协议
    请求：
        请求头
        空行
        请求体

    响应：
        响应头
        空行
        响应体

web服务器
    tomcat(jsp/servlet容器)：可以运行servlet(javaweb应用)

        目录：
            conf: 配置文件目录，server.xml
            webapps： 主要用来存放要发布的web应用
            bin:  tomcat相关的命令文件夹
    
    在tomcat中部署web应用的方法：
        1. 将web应用的webroot目录拷贝到tomcat的webapps下，并重命名该应用。
           项目上下文就是该项目的名字，访问地址：http://localhost:8080/web虚拟目录
        2. (开发过程中常用)在tomcat的conf下找到server.xml，在该文件中Host节点处，增加Context子节点，定义如下：
            <Context path="\项目名" docBase="项目(WebRoot)的物理地址">,例如：
            <Context path="day0513" docBase="D:\Workspace\codes\myeclipse_workspace\day0513\WebRoot">


web应用：
    静态web：静态的html资源组织成的web应用，html叫做静态资源。这种资源会一成不变。
    动态web：jsp/servlet程序

    web应用的目录结构：
        项目上下文(虚拟目录)
            WEB-INF
                web.xml  描述了整个web应用，为javaweb的核心配置文件
                lib    存放web应用中所有用到的第三方jar（除了jdk自带的jar文件之外的）
                classes  整个javaweb应用中所有的java源文件编译后的字节码文件的存放地址  
            jsp   项目中的jsp文件
            js    项目中使用到的js库存放地址(可选)
            css   项目中使用到的css文件存放地址(可选)
            images  项目中所有使用到的图片资源(可选)

Servlet
    什么是servlet？
        运行在服务器(Tomcat)上的一个继承了javax.servlet.HttpServlet类的java程序，它可以用来接收客户端的请求和响应数据到客户端。

    servlet的生命周期
        在服务器启动(在接收首次客户端请求)的时候实例并初始化，
        当接收到客户端请求时执行service方法对客户端提供服务，
        当服务器关闭的时候，servlet销毁。
    
        重点：在整个应用过程中servlet只实例化一次（单例），一个servlet有且只有一个实例对象
    
    相关API：
        HttpServletRequest  请求对象
            接收参数值的方法：
                String  HttpServletRequest.getParameter(参数名)
                String[] HttpServletRequest.getParameterValues(参数名)  获取checkbox的值
                Map<String, String[]> HttpServletRequest.getParameterMap()  获取请求中所有的参数和参数对应的值的映射关系
                String HttpServletRequest.getContextPath()  获取项目上下文对象
                Locale HttpServletRequest.getLocale()       获取浏览器默认的语言环境
                String HttpServletRequest.getServletPath()  获取当前请求的路径
                HttpServletRequest.setAttribute(key, obj); //设置信息
                Object HttpServletRequest.getAttribute(key);//通过key获取设置的值
    
        HttpServletResponse 响应对象
            主要用来向客户端(浏览器)进行结果响应
            HttpServletResponse.setContentType("text/html;charset=utf-8");//设置响应文本类型和编码
            PrintWriter HttpServletResponse.getWriter();
            PrintWriter.out()/print() 将结果输出到浏览器页面
    
        HttpSession         会话
            用来存储登录信息等全局信息，标识一次会话(从用户登录到用户退出的范围)
            HttpSession.setAttribute(key, obj); //设置信息
            Object HttpSession.getAttribute(key);//通过key获取设置的值
    
        ServletContext      应用上下文
            标识在一个应用范围内，是整个web应用中最大的作用域
            ServletContext.setAttribute(key, obj); //设置信息
            Object ServletContext.getAttribute(key);//通过key获取设置的值
    
        ServletConfig       servlet配置对象
            ServletConfig.getInitParameter(参数名) //获取初始化参数的值
            ServletConfig.getServletContext() //获取项目上下文对象

JSP
    jsp是什么？
        jsp（java server page），是运行在服务器端的一个特殊的servlet，它将静态文本和动态数据相分离，扩展名为.jsp

    jsp的组成
        jsp 指令：
            page： 必须有一个page指令放在jsp文件的第一行，表明改文件是一个jsp文件。
            include： 静态包含（先将代码合在一处，再进行编译，输出结果）
            taglib： 引入标签库
    
        静态元素: html标签
    
        jsp动作指令：
            userBean： 实例化javabean
            setProperty： 给javabean的属性赋值
            getProperty： 获取javabean属性的值
            include： 动态包含（先编译，然后将结果合在一处）
        java声明： <%!  java声明 %>
        java脚本：<% java脚本 %>
        java表达式： <%=java表达式  %>
        注释：<%--  --%>和<!-- -->
    
    jsp内置对象
        request         HttpServletRequest
        response        HttpServletResponse
        out             JSPWriter
    
        pageContext     PageContext
        session         HttpSession
        application     ServletContext
    
        page            Object
        config          ServletConfig
    
        exceotion       Throwable

过滤器
    1. 什么是过滤器？
        拦截请求和响应的一个java 类，该java类实现了javax.servlet.Filter接口。

    2. 生命周期
        在服务器启动的时候实例化并初始化，当拦截到指定请求时，执行doFilter进行请求过滤，
        使用FilterChain.doFilter()执行下一个过滤器，若无过滤器，则执行指定的servlet。
        在服务关闭的时候，过滤器销毁。
    
        在整个应用中，过滤器有且只有一个实例！！！


监听器
    1. 监听应用过程中相关对象的状态和属性的变化。

    2. 监听器分类：
        ServletContext监听器
            监听对象的创建和销毁
            监听对象属性的改变
        HttpSession监听器
            监听对象的创建和销毁
            监听对象属性的改变
        HttpServletRequest监听器
            监听对象的创建和销毁
            监听对象属性的改变


    3. 监听器的声明周期
        在服务器启动的时候实例并初始化，在服务器关闭的时候销毁。整个应用过程中有且只有一个实例！！！


web应用中的三大组件： servlet/filter/listener
        
    