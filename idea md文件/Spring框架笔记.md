# Spring学习笔记

## 基本概念

1. Spring优点：

   > 1. 轻量级、非侵入式，对现有的类结构没有影响。
   > 2. 可以提供众多服务，如事务管理、WS等。
   > 3. AOP的很好支持，方便面向切面编程，使得业务逻辑与系统服务分开。
   > 4. 对主流的框架提供了很好的集成支持，如hibernate，Struts2，JPA等，像一个胶水一样，把一些好的框架粘合在一起方便使用。
   > 5. 使用Spring的IOC容器，将对象之间的依赖关系交给Spring，降低组件之间的耦合性，让我们更专注于应用逻辑。
   > 6. Spring DI机制降低了业务对象替换的复杂性。
   > 7. Spring的高度可开放性，并不强制依赖于Spring，开发者可以自由选择Spring部分或全部。
   >
   
2. Spring缺点：

   > 1. 缺少一个共用控制器
   > 2. 没有SpringBoot好用
   > 3. Spring像一个胶水，将框架黏在一起，后面拆分就不容易拆分了

## Spring类

> ApplicationContext：
>
> 	1. FileSystemXmlApplicationContext
>  	2. ClassPathXmlApplicationContext
>  	3. WebXmlApplicationContext

## IOC：Inversion of Control---------控制反转

1. 大致流程

   > 1. 首先根据配置文件找到对应的包，读取包中的类，找到所有含有@Bean，@Service等注解的类，利用反射解析它们，包括解析构造器，方法，属性等等，然后封装成各种信息类放到container(其实是一个map)里（ioc容器初始化）。
   > 2. 获取类时，首先从container中查找是否有这个类，如果没有，则报错，如果有，则通过构造器信息将这个类new出来。
   > 3. 如果这个类含有其他需要注入的属性，则进行依赖注入，如果有则还是从container找对应的解析类，new出对象，并通过之前解析出来的信息类找到setter方法(setter方法注入)，然后用该方法注入对象（这就是依赖注入）。如果其中有一个类container里没有找到，则抛出异常。
   > 4. 如果有嵌套bean的情况，则通过递归解析。
   > 5. 如果bean的scope是singleton，则会重用这个bean不在重新创建，将这个bean放到一个map里，每次用都会先从这个map里面找，如果scope是session，则该bean会放到session。
   >
   > 总结：通过解析xml文件，获取到bean的属性(id,name,class,scope,属性等等)里面的内容利用反射原理创建配置文件里类的实例对象，存入到Spring的bean容器中。

2. 依赖注入

   > 1. 装配方式（依赖注入的具体行为）
   >
   >    1. 基于注解的自动装配
   >    2. 基于XML配置的显示装配
   >    3. 基于java配置的显示装配
   >
   > 2. 依赖注入的方式
   >
   >    1. 构造器方式注入
   >    2. setter方法注入
   >
   >    最好的方法就是用构造器参数实现强制依赖，setter方法实现可选依赖

3. bean知识

   > 1. bean的生命周期
   >    1. 实例化Bean
   >    2. 设置Bean
   >    3. 检查Aware相关接口并设置相关依赖
   >    4. 检查BeanPostProcessor接口并进行前置处理
   >    5. 检查Bean在Spring配置文件中配置的init-method属性并自动调用其配置的初始化方法。
   >    6. 检查BeanPostProcess接口并进行后置处理
   >    7. 当Bean不再需要时，会经过清理阶段，如果Bean实现了DisposableBean这个接口，会调用那个其实现的destroy()方法。
   >    8. 最后，如果这个Bean的Spring配置中配置了destroy-method属性，会自动调用其配置的销毁方法。
   > 2. bean的作用域
   >    1. singleton（单例）
   >       1. singleton是Bean的默认作用域
   >       2. 默认情况下是容器初始化的时候创建，但也可设定运行时在初始化bean
   >       3. DefaultSingletonBeanRegistry类里的singletonObject哈希表保存了单例对象
   >       4. Spring容器可以管理singleton作用域下的bean的生命周期，在此作用域下，Spring能够精确地知道bean何时被创建，何时初始化完成，以及何时被销毁
   >    2. prototype（多例）
   >       1. 每次通过Spring容器获取prototype定义的bean，容器都将创建一个新的Bean实例，每个bean实例都有自己的属性和状态。
   >       2. 当容器创建了bean的实例后，bean的实例就交给了客户端的代码管理，Spring容器将不再跟踪其生命周期，并且不会管理那些被配置成prototype作用域的bean生命周期。
   >       3. 对有状态的bean使用prototype作用域，而对无状态的bean使用singleton作用域。
   >    3. request
   >       1. 在一次Http请求中，容器会返回该Bean的同一实例。而对不同的Http请求则会产生新的Bean，而且该Bean仅在当前Http Request内有效。
   >    4. session
   >       1. 在一次Http Session，容器会返回该Bean的同一实例。而对不同的Session请求则会创建新的实例，该bean实例仅在当前Session内有效。
   >    5. global Session
   >       1. 在一个全局的Http Session中，容器会返回该Bean的同一个实例，仅在使用portiet context时有效。
   > 3. bean的创建
   >    1. 进入getBean()方法
   >    2. 判断当前bean的作用域是否是单例，如果是，则去对应缓存中查找，没有查找到的话则新建实例并保存，如果不是单例，则直接新建实例(creatBeanInstance)
   >    3. 新建实例后再将注入属性(populateBean),并处理回调

   ## AOP：Aspect-OrientedProgramming----------面向切面编程

   > 1. 实现原理
   >
   >    1. jdk动态代理
   >
   >       1. 主要通过Proxy.newProxyInstance()和InvocationHandler这两个类和方法实现
   >       2. 实现过程
   >
   >          1. 创建代理类proxy实现Invocation接口，重写invoke()方法（调用被代理类方法时默认调用此方法）
   >          2. 将被代理类作为构造函数的参数传入代理类proxy
   >          3. 调用Proxy.newProxyInsatnce(classloader,interface,handler)方法生成代理类
   >             1. 生成的代理类
   >                1. $Proxy() extends Proxy inplements Person
   >                2. 类型为$Proxy0
   >                3. 因为已经继承了Proxy，所以java动态代理 只能对接口进行代理
   >             2. 代理对象会实现用户提供的这组接口，因此可以将这个代理对象强制类型转化为这组接口中的任意一个
   >             3. 通过反射生成对象
   >
   >       总结：代理类调用自己方法时，通过自身持有的中介类对象来调用中介类对象的invoke方法，从而达到代理执行被代理对象的方法。
   >
   >    2. cglib
   >
   >       1. 生成对象类型为Enhancer
   >       2. 实现原理类似于jdk动态代理，只是他在运行期间生成的代理对象是针对目标类扩展的子类
   >
   >    3. 静态代理
   >
   >       1. 缺点：
   >          1. 如果要代理一个接口的多个实现的话需要定义不同的代理类
   >          2. 代理类和被代理类必须实现同样的接口，万一接口有变动，代理、被代理类都得修改
   >       2. 在编译的时候就直接生成代理类
   >
   >    4. JDK动态代理和cglib的对比
   >
   >       1. CGLib所创建的动态代理对象在实际运行时候的性能要比JDK动态代理高
   >       2. CGLib在创建对象的时候或者具有实例池的代理，因为无需频繁的创建代理对象，所以比较适合采用CGlib动态代理，反之，则适合用JDK动态代理
   >       3. JDK动态代理是面向接口的，CGLib动态代理是通过字节码底层继承代理类来实现（如果被代理类被final关键字所修饰的，那么就会失败）
   >       4. JDK生成的代理类类型是Proxy（因为继承的是Proxy），CGLib生成的代理类类型是Enhancer类型
   >       5. 如果要被代理的对象是个实现类，那么Spring会使用JDK动态代理来完成操作（Spring默认采用JDK动态代理实现机制）
   >       6. 如果要被代理的对象不是实现类，那么Spring会强制使用CGLib来实现动态代理。
   >
   > 2. 配置方式
   >
   >    1. XML方式
   >    2. 注解方式
   >    3. 基于java类配置  通过@Configuration和@Bean这两个注解实现的
   >       1. @Conflguration作用于类上，相当于一个xml配置文件
   >       2. @Bean作用于方法上，相当于xml配置中的<bean>
   >
   > 3. 基本概念
   >
   >    1. 核心业务功能和切面功能分别独立进行开发，然后把切面功能和核心业务功能“编织”在一起，这就叫AOP
   >    2. 让关注点代码于业务代码分离
   >    3. 面向切面编程就是指：对很多功能都有重复的代码抽取，再运行的时候往业务方法上动态植入“切面类代码”。
   >    4. 应用场景：日志、事务管理、权限控制

## 事务管理

> 1. 基本概念：
>    1. 如果需要某一组操作具有原子性，就用注解的方式开启事务，按照给定的事务规则来执行提交或者回滚操作
>    2. 事务管理一般在Service层
> 2. 事务控制
>    1. 编程式事务控制：用户通过代码的形式手动控制事务
>       1. Conn.setAutoCommite(false);//设置手动控制事务
>       2. 粒度较细，比较灵活，但开发起来比较繁琐；每次都要开启，提交，回滚
>    2. 声明式事务控制：Spring提供对事务的控制管理
>       1. XML方式
>       2. 注解方式
>    3. 事务属性：
>       1. 事务传播行为：
>          1. required_new：如果当前方法有事务了，当前方法事务会挂起，在为加入的方法开启一个新的事务，直到新的事务执行完、当前方法的事务才开始
>          2. required(默认方法)：如果当前方法已经有事务了，加入当前方法事务
>          3. 其余五种方式(拓展学习)
>          4. 事务传播行为用来描述由某一个事务传播行为修饰的方法被嵌套进另一个方法的时候事务如何传播。
>       2. 数据库的隔离级别：
>          1. TransactionDefinition.ISOLATION_DEFAULT：使用后端数据库默认的隔离级别，Mysql默认采用的REPEATABLE_READ隔离级别Oracle默认采用的READ_COMMITTED隔离级别。
>          2. TransactionDefinition.ISOLATION_READ_UNCOMMITTED：最低的隔离级别，允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读。
>          3. TransactionDefinition.ISOLATION_READ_COMMITTED：允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或者不可重复读仍有可能发生。
>          4. TransactionDefinition.ISOLATION_REPEATABLE_READ：对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读或不可重复读，但幻读仍有可能发生。
>          5. TransactionDefinition.ISOLATION_SERIALIZABLE：最高的隔离级别，完全服从ACID的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。但是这将严重影响程序的性能，通常情况下也不会用到该级别。
>       3. 事务超时属性：指一个事务所允许执行的最长时间，如果超过该时间限制但事务还没有完成，则自动回滚事务。
>       4. 事务只读属性：对事务资源是否执行只读操作
>       5. 回滚规则：定义了哪些异常会导致事务回滚而哪些不会，默认情况下，事务只有遇到运行期异常时才会回滚，而在遇到检查型异常时不会回滚，也可以用户自己定义
>    4. Spring事务管理接口：
>       1. PlarformTransactionManager
>          1. (平台)事务管理器
>          2. Spring并不直接管理事务，而是提供了多种事务管理器，通过PlatformTransactionManager这个接口，Spring为各个平台如JDBC、Hibernate等都提供了对应的事务管理器，但是具体的实现就是各个平台自己的事情了。
>       2. TransactionDefinition：事务定义属性(事务隔离级别、传播行为、超时、只读、回滚规则)
>       3. TransactionStatus：事务运行状态
>    5. 事务管理一般在Service层：
>       1. 如果在dao层，回滚的时候只能回滚到当前方法，但一般我们的service层的方法都是由很多dao层的方法组成的
>       2. 如果在dao层，commit的次数会很多

# SpringMVC

> 1. servlet生命周期
>    1. 加载和实例化
>    2. 初始化
>    3. 请求处理
>    4. 服务终止
> 2. 执行流程
>    1. 用户发送请求至前端控制器DispatherServlet
>    2. DispathcherServlet收到请求调用HandlerMapping处理器映射器
>    3. 处理器映射器根据请求url找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet
>    4. DispatcherServlet通过HandlerAdapter处理器适配器调用处理器
>    5. 执行处理器(Controller，也叫后端控制器)
>    6. Controller执行完成返回ModelAndView
>    7. HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet
>    8. DispatcherServlet将ModelAndView传给ViewReslover视图解析器
>    9. ViewReslover解析后返回具体View
>    10. DispatcherServlet对View进行渲染视图(即将模型数据填充至视图中)。
>    11. DispatcherServlet响应用户。

