这部分是对 Web开发的自己的理解，侧重于随手记录，主要反映自己对知识的理解程度。

Web 服务器只能输出 静态页面 ， 而对于动态页面，则借助于 `小程序` 的方式来动态生成 HTML，Servlet 就是在这个背景下出现的。每个 Servlet 应对一个动态的请求，返回对应的内容，这个内容可以是 HTML，JSON，IMG，等 Web资源。Servlet 是暴露给用户的接口，用于处理 Web的请求和响应。这些 Servlet 和 Web 服务器的交互则有 Web容器负责，Web 容器负责 创建 ServerSocket ，监听端口，创建流等。同时管理这些 Servlet的生命周期。

什么是 **容器**？

Servlet 是没有 main 方法的，他们受控于另一个java应用，这个java应用称为容器。



### 开发工作流

有了 Web 容器和 Servlet ，那么 Web开发 只需要做：

- 实现 Servlet 提供的方法逻辑，基本上是对应每个请求的处理和响应。

- 实现 请求URL 到 Servlet的映射，也就是路由的管理。有两种实现方式，第一种是 `Deployment Descriptor` （web.xml）；第二种是直接注解的方式。

  >  貌似路由映射这个直接面向开发的部分，可以单独抽象出来。

  

**明确**

Servlet 三个语义：

- 请求的URL对应的哪个 `Servlet`。
- 代码的 `Servlet` 类，也就是全类名。
- DD文件定义的 Servlet 名字，这部分是服务于 Servlet-mapping。



### 学习记录线

Web 开发的体系结构

- HTTP 协议
- MVC 模式
- Web 容器，Web服务器
- Servlet 细节，结合 Servlet 的实现来讲解 HTTP 

> 👉 参考书籍： Head First Servlet and JSP 
>
> 👉 参考书籍： 图解 HTTP



构建一个Web 应用（Servlet HelloWorld）


Servlet 细节

- HTTP 请求产生，处理，响应的前后端的流程。




Java Web 后端的演进：


##### 1. Spring 和 java EE 关系？

JavaEE （现在是 Jakarta EE）：Sum 开发，用于构建企业级应用的规范和工具，提供了一整套规范，包括 Servlet, EJB, JPA, JMS 等。

**Spring Framework:**  Spring 最初是作为一个轻量级的、可逆的替代方案来解决 Java EE 的复杂性和重量级问题。

这是两个独立的、但相互关联的技术堆栈。

##### 2. Java EE 演进的路线

J2EE => Java EE 5 =>  Java EE 6 => Java EE 7 =>  Java EE 8 => Jakarta EE 8  => Jakarta EE 9

##### Spring Framework 的工作



核心概念

- Web 服务器
- Web 容器（servlet 容器）
- Java EE 



