# Spring

```
目标: https://github.com/mafei007even/Spring-impl

在b站学习了老师的 Spring 原理课程之后，独立完成写了个仿 Spring 轮子项目，实现功能包括：
1. @ComponentScan 组件扫描
2. bean 完整的生命周期过程（创建-依赖注入-初始化-销毁），包括 Bean后处理器（BeanPostProcessor）的调用、各种 Aware 接口回调、执行销毁方法。
3. 使用三级缓存解决属性注入和 set 方法注入的循环依赖问题，@Lazy 注解、ObjectFactory 解决构造方法注入的循环依赖问题
4. 完成了 5 种通知类型（@Before、@AfterReturning、@After、@AfterThrowing、@Around）的解析，对符合切点的目标对象进行代理增强。 应用在目标方法上的多个通知会链式调用执行，且实现了通知的调用顺序控制。

用到的设计模式：代理（JDK 生成动态代理）、责任链（通知的链式调用）、适配器（适配各种销毁方法的调用）、单例（比较器）、工厂（ObjectFactory）

参考 Spring 源码实现，类名几乎都和 Spring 类名一致，代码质量我个人觉得还行，加上接口和注解，一共写了近 50 个类，没有依赖别的 jar 包，直接复制代码就能运行。

有感兴趣的同学的可以瞅瞅，跟着老师课程学完了也可以自己动手写一个轮子项目，对理解 Spring 运行流程很有帮助，搞懂了直接简历项目 + 1
```



## 1.入门

```java
package com.example.liu;

import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Tests {

    @Test
    public void testUserObject(){
        //加载Spring配置文件，创建对象, 说明反射是通过无参构造方法来创建对象交由spring容器管理的
        ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
        User user = (User) context.getBean("user");
        System.out.println("line1:" + user);   //line1:com.example.liu.User@6221a451
        System.out.println("line2:" + user);   //line2:com.example.liu.User@6221a451
        user.add();  //add()方法
    }
}
```

反射如何创建对象？创建后的对象放在哪里？

```java
1.解析XML文件获得bean标签；
2.反射根据类的全路径创建对象; 
   //通过反射创建对象
    @Test
    public void testCreateObject() throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException {
        Class clazz = Class.forName("com.example.liu.User");
        User u = (User) clazz.getDeclaredConstructor().newInstance();
        u.add();
    }

创建后的对象放在 Map<String,BeanDefinition> beanDefinitionMap
key是配置文件中id唯一标识，BeanDefinition是类的描述信息
这个map在DefaultListableBeanFactory类中
```

## 2.IOC控制反转

Spring通过IOC容器来管理所有Java对象的实例化和初始化，控制对象与对象之间的依赖关系。我们将由IOC容器管理的Java对象称为Spring Bean，它与使用关键字new创建的Java对象没有任何区别。

如果一个接口只有一个实现类，那么根据接口类型可以获取bean吗？

> 可以，因为bean唯一

如果一个接口有多个实现类，这些实现类都配置了bean，那么根据接口类型可以获取bean吗？

> 不行，因为bean不唯一

### 2.1依赖注入

创建对象过程中，向属性中设置值，有两种方法实现。

`setter注入` 

`构造器注入`

### 2.2 bean的作用域

```
在Spring中可以通过配置bean标签的scope属性来指定bean的作用域范围
singleton（默认）   单实例    创建对象的时机：IOC容器初始化时
prototype          多实例    创建对象的时机：获取bean时

如果是在WebApplicationContext环境下还会有另外的几个作用域（但不常用）:
request    在一个请求范围内有效
session    在一个会话范围内有效
```

## 2.3 bean的生命周期

- bean对象的创建（调用无参数构造）

- bean对象设置相关属性

- bean后置处理器（初始化之前）

- bean对象初始化（调用指定初始化方法）

- bean后置处理器（初始化之后）

- bean对象创建完成，使用

- bean对象销毁（配置指定销毁的方法）

  ```text
  bean的后置处理器会在生命周期的初始化前后添加额外的操作，需要实现BeanPostProcessor接口，且配置到IOC容器中，需要注意的是，bean后置处理器不是单独针对某一bean生效，而是针对IOC中所有bean都会执行。
  ```


# SpringBoot

## 一、源码阅读



# SpringCloud

# K8S

# Redis

# Docker

# Nginx



Mybatis