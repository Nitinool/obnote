8/31之前过一遍java基础


java基础
变量类型 基本控制语句
面向对象构建使用 三大特性 
javaweb



## 面向对象
### 面向对象-静态  static类里面的方法的关键字 

和类相关的方法都叫静态方法 可以直接通过类名而不是某个实例来调用

和类相关的属性都叫静态属性

静态代码块会在类初始化时自动调用
```JAVA
Public class demo {
	public static void main(){
		new User01；
		User01.test
	}
}

class User01{
	static {
		//静态代码块 在类调用的时候这块代码会执行
		print("静态代码块执行 ")
	}
	static void test{
		print("test.... ")
	}
}
```


### 面向对象-构建对象 

构造方法： 专门用于构建对象
1 基本语法 类名( ){ }
2 方法名和类名完全相同  而且也是方法但是没有void关键字
```java

class User01{
	String username;  
	User01(){
	//这是一个构造方法 如果没写 默认是无参构造方法
	}
	User01(String name){
	//这是一个有参数的构造方法 
		username = name;
	
	}

}
```
   

### 面向对象--继承 extends   关键字 this
每次创建一个子类对象时会先调用父类的构造方法
如果不是默认的构造方法 那么子类的构造方法第一句要加上super()
```java

class Parent{
	String name = "zhangsan";
	int age;
	parent(String name){
		name = name;
	}
	void test(name age){
		System.out.println("name:" + name)
	} 
}

class Child extends Parent{
	String name = "lisi";
	
	Child(){
		super("lisi");
	}

	void test(){
		//调用子类属性
		System.out.println(this.name)
		//调用父类属性
		System.out.println(super.name)
	}
}

// 如果父类和子类里面都有相同的变量名 用super和this进行区分
// super && this


```

### 面向对象--多态 
其实就是一个对象在不同情况下的不同状态和形态
以父类声明的子类 只能使用父类方法
```JAVA

Class Person{
	void testPerson(){
		sout("person");
	} 
}

Class Boy extends Person{
	void testBoy(){
		sout("Boy");
	} 
}

//可以这么用
Person p = new Person();
p.testPerson();
Person boy = new Person();
boy.testPerson();
//但是不能这么用
boy.testBoy(); //如果想用自己的方法 那么 Person boy 得改成 Boy boy。


```

### 面向对象--重载
使用场景 假设一个类有相同名字的多个方法 这是不被允许的 但是只要参数不一样就可以 这叫做重载

构造方法也可以重载 意思是可以同时有无参构造和有参构造 
如果一个构造方法想调用另一个构造方法 可以使用this() 类似super()的使用
```java

class user{
	user(){
		this("zhangsan") //这句等同于 new user("zhangsan")
	}
	user(String name){
	}
	user(String name,int age){
	}

}

```
### 面向对象--重写

假如父类和子类都有同名的方法 如果new一个子类并调用他的方法 那么调用的是子类里面的方法

但是 如果是以父类声明的子类 然后都有同名的方法 那么调用的是子类的方法 这与前面的多态中 的 “以父类声明的子类 只能使用父类方法”并不冲突 前面强调的是能不能用这个方法 既然两者方法同名 后面强调的是怎么用这个方法 

```JAVA


CCC ccc = new DDD();
int sum = ccc.sum();

System.out.println(sum)
//结果是30 运行ccc.sum()时因为ccc里面没有sum方法 但是ccc有 所以会调用ccc的方法
//然后到了geti方法的时候他会回去使用ddd的geti方法 



Class CCC{
	int i = 10;
	
	int sum(){
		return getI() + 10;
	}
	
	int getI(){
		return i
	}
}

Class DDD extends CCC{
	int i = 20;
	
//	int sum(){
//		return getI() + 10;
//	}
	
	int getI(){
		return i
	}
}
```


### 面向对象--访问权限

权限关键字之public 一个java文件里面只能有一个公共类 且必须与文件名同名

关于main方法的public关键字 main方法由jvm调用 调用时不应该设置权限 
关于main方法的static关键字 如果不加static就变成了成员方法 必须实例化类才能调用

java中的访问权限主要分为4种：
- private： 私有的 同个类可以使用
- default：默认权限 同个包可以使用
- protected：受保护的权限 同类同包
- 注意文件里面的公共类和自定义类使用的object方法的小问题 自定义类能用 main不能用
- public：公共的 任意范围


#### 面向对象--内部类和外部类

内部类 在类中声明的类 内部类就当成外部类的属性使用即可
外部类 在源码中声明的类 java不允许外部类使用private 和 protected

```java

Public class demo {
	public static void main(){
	
		OuterClass outer = new Outerclass();
		OuterClass.InnerClass inner = outer.new InnerClass(); 
		
	}
}

class OuterClass{
	private InnerClass{
	}
	
}
```

#### 单例模式
p61 完全听不懂

### 面向对象 final关键字
用final修饰的变量初始化之后不可修改‘

### 抽象类和抽象方法 abstract

abstract 和 final 关键字不能一起使用。
```java
abstract class Person{
	public abstract void test();
}
```

#### 接口
简单理解为规范
```JAVA
接口也是抽象的
基本语法：interface 接口名称 {规则属性，规则的行为}

属性必须是固定值而且不能修改
属性的行为的访问权限必须是公共的
属性应该是静态的 行为应该是抽象的

类可以通过关键字implement实现接口

interface TestInterface {
	public void test()
}

class demo implements TestInterface{
	@override
	public void test(){
	}
}


```


#### 枚举 Enum
枚举是一个特殊的类 其中包含了一组特定的对象，这些对象不会发生改变 一般使用大写

```java

City.Beijing

enum City{
	Beijing("beijing",1001)，Shanghai;
	
	City(String name, int code){
		this.name = name;
		this.code = code;
	}
	
	public String name;
	public int code;
	
}
等价于下面这种写法

class City{
	public String name;
	public int code;
	
	private City(String name, int code){
		this.name = name;
		this.code = code;
	}
	
	public static final City Beijing = new City("beijing",1001);

}
```

### 面向对象--JavaBean规范
### 总结

1.创建子类时父类对象并没有被创建 只是把父类对象的相关变量传递到了子类里
2.



## 常用基本类

### Obeject

```java


1 Object.toString()
任何类都可以使用 输出对象的内存地址

public class Main {  
    public static void main(String[] args) {  

        Person person = new Person();  
        System.out.println(person.toString());  
    }  
}  
  
class Person{  
    String name = "zhangsan";  
    int age = 10;  
	  
	//重写父类Object的toString方法 快捷键ctrl + o
    @Override  
    public String toString() {  
        return "name: " + name + ", age: " + age;  
    }  
}

2 Object.hashcode()
3 Object.equals(
)
```

### 引用数据类型-数组和二维数组
```JAVA
1 初始值就是类型的默认值


声明方式 类型[] 名字
创建方式1 new 类型[容量]
创建方式2 直接用{}里面添加所需要的变量

String[] strings = new String[3];
String[] strings = {"zhangsan","lisi"};
int[] nums = new int[10];
int[] nums = {1,2,3,4,5};

2 增删改查


3 二维数组
在一维数组的[]在添加一个[]

练习1 九层妖塔
public class Main {  
    public static void main(String[] args) {  
  
        int row = 9;  
        int column = 17;  
        String[][] nineTower = new String[row][column];  
        for (int i = 0; i < row; i++) {  
            for (int j = 0; j < column; j++) {  
                if (j <= 8 + i && j >= 8 - i ) {  
                    System.out.print("*");  
                }else {  
                    System.out.print("-");  
                }  
            }  
            System.out.println();  
        }  
    }  
}

练习2 冒泡排序 每次交换找到最大或者最小的数

public class Main {  
    public static void main(String[] args) {  
  
        int[] nums = {3,1,4,63,76,32};  
  
        for (int j = 0; j < nums.length ; j++) {  
            for (int i = 0; i < nums.length-1; i++) {  
                int num1 = nums[i];  
                int num2 = nums[i+1];  
                if (num1 > num2){  
                    nums[i+1] = num1;  
                    nums[i] = num2;  
                }  
            }  
        }  
        for(int num : nums){  
            System.out.println(num);  
        }  
    }  
}
练习3 选择排序 每次设最大下标是0 然后找一个最大或者最小的数的下标然后进行交换
练习4 二分查找
```

### 引用数据类型--字符串String

```java
1 字符串 字符 字节
字符串可以看成字符数组 每个字符里面又有n个字节 按照不同的编码格式

char[] a = {'a','b','c'}
byte[] b = {-28,-72,-83,-27,-101,-67}

String s = new String(a)  //char数组能变成字符串
String s1 = new String(b,"UTF-8")

2 拼接操作

String a = "zhangsan"
String b = "333"
String c = "111"

使用 + 号
String s = a + b + c
使用concat方法
String s = ""
s.concat(a)

使用Stringbuilder替换
因为普通的String拼接操作会新建一个字符串，开销大
可以用new Stringbuilder 然后使用append方法拼接 最后使用toString转为字符串

3 字符串的比较
//基于编码判断
s.equal(b)
//忽略大小写
s.equalsIgnoreCase(b)
//返回ASCII码的差值
s.compareTo(b)

4 字符串的截取操作
S.substring() 取字符串子串
一个参数：起始位置 默认截取到最后
两个参数：起始位置和截取长度
返回值：字符串

S.split(" ")  分割字符串
参数：分割的字符
返回值：字符串数组

S.trim() 去掉字符串首尾的空格

5 字符串的替换
S.replace("a","b")
参数：字符串里面的内容，替换后的内容
返回值：字符串
S.repalceAll("","")
参数：替换规则，替换后的内容
返回值：字符串

6 字符串的查找
char[] = S.toCharArray() 
返回值：返回一个字符数组
byte[] = S.getBytes("UTF-8")
参数：编码规则
返回值：一个字节码数组

s.charAt() 查询字符串中指定下标的字符
参数：下标
返回值：字符

s.indexOf("") 获取子串在字符串第一次出现的起始位置
s.lastIndexOf("") 获取子串在字符串最后一次出现的起始位置
s.contain("") 判断是否包含指定的字符串，返回布尔类型
s.startwith("") 判断是否以指定的字符串开头
s.endwith("") 判断是否以指定的字符串结尾
s.isEmpty() 判断字符串是否为空
 ```


### 包装类

每一个基础数据类型都有一个包装类  
其原因是包装类可以表示null值，而基本数据类型不能
在某些情况下，比如接口传参，如果使用包装类不传参也不会报错，而使用基本数据类型不传参会报错
boolean 

byte
short
int
long

char

float
double

自动拆箱 和 自动装箱  也就是提供了一个简单的互相转化的方法 直接赋值

### 日期类 和 日历类
```java
Date 日期类
Date date = new Date()


java日期格式化 和 字符串互相转化
y -> 年 yyyy  Y -> 
m -> 分钟 mm   M  月份


Clender 日历类

  
```



## 异常类

```JAVA
1 异常的语法

try {
	可能会出现异常的代码
	如果出现异常 jvm会将异常抛出形成一个具体的异常类
}catch(抛出的异常 对象引用){
	异常的解决方案
	捕捉多个异常时 先捕捉范围小的 再捕捉大的
}
... ...
catch(){

}finally{
	最终执行的代码逻辑
}

int j = 0;
int i = 0;
try{
	j = 10 / i;
}catch(ArithmeticException e){
	e.getMessage() // 错误的消息
	e.printStackTrace();
}finally{
	
}

2 如果是方法可能会出现问题 就要使用throws关键字来抛出异常
如果需要再方法里面手动抛出异常 那么就要catch之后new一个异常

3 自定义异常 p97
主要是自定义异常 抛异常和处理异常
```
#没听懂