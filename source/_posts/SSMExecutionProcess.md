---
title: SSM各层级作用
date: 2020-08-11 09:42:35
cover: true
tags: 
    - Spring
    - SpringMVC
    - MyBatis
categories: SSM
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/Ee1nlx7WAAEuyMu.jpg
summary: SSM框架集由Spring、SpringMVC、MyBatis三个开源框架整合而成，常作为数据源较简单的web项目的框架。
---

## SSM

> SSM：spring+springMVC+mybaits
>
> Spring：是一个容器，就是一个bean（实体对象）大集合。
>
> SpringMVC：控制器（业务逻辑层）(视图分发器)。
>
> Mybaits：jdbc的封装（数据库框架）Mapper.xml。

SSM框架集由Spring、SpringMVC、MyBatis三个开源框架整合而成，常作为数据源较简单的web项目的框架。

其中spring是一个[轻量级](https://baike.sogou.com/lemma/ShowInnerLink.htm?lemmaId=7988600&ss_c=ssc.citiao.link)的[控制反转](https://baike.sogou.com/lemma/ShowInnerLink.htm?lemmaId=666629&ss_c=ssc.citiao.link)（IoC）和面向切面（AOP）的容器框架。

[SpringMVC](https://baike.sogou.com/lemma/ShowInnerLink.htm?lemmaId=40917807&ss_c=ssc.citiao.link)分离了控制器、模型对象、分派器以及处理程序对象的角色，这种分离让它们更容易进行定制。

MyBatis是一个支持普通SQL查询，存储过程和高级映射的优秀[持久层](https://baike.sogou.com/lemma/ShowInnerLink.htm?lemmaId=55187093&ss_c=ssc.citiao.link)框架。
页面发送请求给控制器，控制器调用业务层处理逻辑，[逻辑层](https://baike.sogou.com/lemma/ShowInnerLink.htm?lemmaId=17228&ss_c=ssc.citiao.link)向持久层发送请求，持久层与数据库交互，后将结果返回给业务层，业务层将处理逻辑发送给控制器，控制器再调用视图展现数据。

## 各层级

- 表现层（springMVC）：Controller层（Handler层）
  - 负责具体的业务模块流程的控制
  - Controller层通过要调用Service层的接口来控制业务流程，控制的配置也在Spring配置文件里面。
- 业务层（Spring）：Service层
  - Service层：负责业务模块的逻辑应用设计。
  - 首先设计其接口，然后再实现他的实现类。
  - 通过对Spring配置文件中配置其实现的关联，完成此步工作，我们就可以通过调用Service的接口来进行业务处理。
  - 最后通过调用DAO层已定义的接口，去实现Service具体的 实现类。
- 持久层（Mybatis）：Dao层（Mapper层）
  - Dao层：负责与数据库进行交互设计，用来处理数据的持久化工作。
  - DAO层的设计首先是设计DAO的接口，
  - 然后在Spring的配置文件中定义此接口的实现类，就可在其他模块中调用此接口来进行数据业务的处理，而不用关心接口的具体实现类是哪个类，这里用到的就是反射机制， DAO层的数据源配置，以及有关数据库连接的参数都在Spring的配置文件中进行配置。
- 视图层：View层
  - 负责前台jsp页面的展示。
  - 此层需要与Controller层结合起来开发。
- 各层间的联系：
  - 本来Controller层与View层是可以放在.jsp文件里一起开发的，但是为了降低代码的复杂度，提高其可维护性，将其分为了这两层，这也体现了MVC框架的特性，即结构清晰，耦合度低。
  - Service层是建立在DAO层之上的，建立了DAO层后才可以建立Service层，而Service层又是在Controller层之下的，因而Service层应该既调用DAO层的接口，又要提供接口给Controller层的类来进行调用，它刚好处于一个中间层的位置。每个模型都有一个Service接口，每个接口分别封装各自的业务处理方法。

<img src="https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/ssm框架原理流程图.png">

### Spring MVC

>  Spring MVC框架也是一个基于请求驱动的Web框架，并且也使用了前端控制器模式来进行设计，再根据请求映射规则分发给相应的页面控制器（动作/处理器）进行处理。

**执行流程**

-  首先用户发送请求——>DispatcherServlet，前端控制器收到请求后自己不进行处理，而是委托给其他的解析器进行处理，作为统一访问点，进行全局的流程控制；
-  DispatcherServlet——>HandlerMapping， HandlerMapping将会把请求映射为HandlerExecutionChain对象（包含一个Handler处理器（页面控制器）对象、多个HandlerInterceptor拦截器）对象，通过这种策略模式，很容易添加新的映射策略；
- DispatcherServlet——>HandlerAdapter，HandlerAdapter将会把处理器包装为适配器，从而支持多种类型的处理器，即适配器设计模式的应用，从而很容易支持很多类型的处理器；
- HandlerAdapter——>处理器功能处理方法的调用，HandlerAdapter将会根据适配的结果调用真正的处理器的功能处理方法，完成功能处理；并返回一个ModelAndView对象（包含模型数据、逻辑视图名）；
- ModelAndView的逻辑视图名——> ViewResolver， ViewResolver将把逻辑视图名解析为具体的View，通过这种策略模式，很容易更换其他视图技术；
- View——>渲染，View会根据传进来的Model模型数据进行渲染，此处的Model实际是一个Map数据结构，因此很容易支持其他视图技术；
- 返回控制权给DispatcherServlet，由DispatcherServlet返回响应给用户，到此一个流程结束。

<img src="https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/springmvc框架原理流程图.png">

### Maybatis

- 获取配置文件mybatis-config.xml，由SqlSessionFactoryBuilder对象，读取并解析配置文件，返回SqlSessionFactory对象
- 由SqlSessionFactory创建SqlSession 对象，没有手动设置的话事务默认开启
- 调用SqlSession中的api，获取指定UserMapper.class文件
- 由jdk动态代理，获取到UserMapper.xml文件，内部进行复杂的处理，最后调用jdbc执行SQL语句，封装结果返回。