# Spring MVC

## 1.Spring MVC的原理

Spring MVC中的MVC即模型-视图-控制器，该框架围绕一个DispatcherServlet设计，DipatcherServlet会把请求分发给各个处理器，并支持可配置的处理器映射和视图渲染等功能，spring mvc的工作流程如下所示：

![](D:\workspace\Java-Interview-Offer\images\springmvc001.png)

( 1 ）客户端发起HTTP请求：客户端将请求提交到DispatcherServlet。

( 2 ）寻找处理器：由DispatcherServlet控制器查询一个或多个HandlerMapping，找到处理该请求的Controller。

( 3 ）调用处理器：DispatcherServlet将请求提交到Controller。

( 4 ）调用业务处理逻辑并返回结果：Controller调用业务处理逻辑后，返回ModelAndView。

( 5 ）处理视图映射并返回模型：DispatcherServlet查询一个或多个ViewResoler视图解析器，找到ModelAndView指定的视图。

( 6) HTTP响应：视图负责将结果在客户端浏览器上渲染和展示。

