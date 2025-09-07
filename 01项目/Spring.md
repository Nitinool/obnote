
01 入门案列
```JAVA
package com.atguigu.spring6;  
  
import org.junit.jupiter.api.Test;  
import org.springframework.context.ApplicationContext;  
import org.springframework.context.support.ClassPathXmlApplicationContext;  
  
public class TestBean {  
    @Test  
    public void testUserObject() {  
        // 加载spring配置文件，对象创建  
        ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");  
        // 获取创建的对象  
        User user = (User)context.getBean("user");  
        System.out.println(user);  
        // 使用对象调用方法进行测试  
        user.test();  
    }  
}

1 在spring中不用new对象 而是将对象直接交给了spring 
2 spring底层是通过反射来创建对象 创建对象时会加载类的无参构造
	1 加载bean.xml配置文件(写一个类 然后写进bean.xml配置文件)
	2 对xml文件进行解析
	3 获取xml文件bean标签属性值 id和class
	4 通过反射根据类全路径创建对象
	public void testObject() throws Exception {  
	    // 获取类class对象  
	    Class<?> aClass = Class.forName("com.atguigu.spring6.User");  
	    //调用方法创建对象  
	    //Object o = aClass.newInstance();  
	    User user = (User)aClass.getDeclaredConstructor().newInstance();  
	    System.out.println(user);  
	}
3 创建的对象放在一个map里面 key是唯一标识 value是类的描述信息
```

02 日志框架 apache Log4j2
日志级别
日志输出位置
日志输出格式
