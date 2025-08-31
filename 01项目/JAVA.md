8/31之前过一遍java基础


开始 java基础
变量类型 基本控制语句
面向对象构建使用 三大特性 
。。。
后续 javaweb



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










## 集合 collection
java中的集合其实是集合框架 针对单一数据类型和键值对类型

1 什么时候需要集合对象？数据的个数的时候。
2 为什么不使用数组？因为数组使用起来不方便。


1 Cllection接口
	常用的子接口
		List 按顺序保存数据 数据可以重复
			具体实现类：ArrayList，LinkedList
		Set  无序保存 数据不能重复
			具体实现类： HashSet
		Queue 队列
			具体实现类： ArrayBlockingQueue
2 Map接口
	具体实现类 HashMap HashTable


### 01 ArrayList
```java
可以看成一个封装了数组的类

1 创建对象
ArrayList list = new ArrayList()
参数：无参，数字（表示初始化数组长度），集合对象
返回值:

2 增删改查

增加数据 list.add("zhangsan") 

存放的数据是数据的内存地址，不是真实的值。
如果存放数据时，集合为空，那么会自动创建一个长度为10的数组。
如果数据个数超出数组容量时，集合会自动扩容。（集合类会复制一个长度比原来数组更长的数组，然后把数据放到新的数组里面）

获取长度 list.size() 
获取指定位置的数据 list.get(index)
修改指定位置的数据 list.set(1,"lisi") 传入要改的index和要改的值 返回修改前的值
删除指定位置的数据 list.remove(1) 传入要删除的indx 返回删除的值
```

### 02 LinkedList  
```java
类似于双链表

1 创建对象
LinkedList list = new LinkedList()
参数：无参，集合对象

2 增删改查
增加数据 list.add("zhangsan") list.addFirst("zhangsan") 
获取数据 list.getFirst() list.getLast()
```


### 03 泛型
```java

泛型作用就是定义容器里面的数据类型

01 在集合里面使用
原始方式
ArrayList list = new ArrayList()
Person person = new Person()
list.add(person)
Object o = list.get(0) 这里获取的类型是Object 后续要使用的话需要强制转换
Person p = (Person) o;

定义泛型
ArrayList<Person> list = new ArrayList() //声明集合里面存放的是泛型
Person person = new Person()
list.add(person)
Person o = list.get(0) 这里获取的类型是不需要强制转换

02 在其他容器里面使用

MyContainer<User> myContainer = new MyContainer()

class MyContainer<C>{
	public C data;
}
```


### 04 集合--比较器

```java

java的集合实现了排序的方法 


public class Main {  
    public static void main(String[] args) {  
  
        ArrayList list = new ArrayList();  
  
        list.add(1);  
        list.add(2);  
        list.add(3);  
  
  
        list.sort(new NumComparator());  
        System.out.println(list);  
    }  
}  
//排序需要传递一个实现了比较器接口的对象 
class NumComparator implements Comparator<Integer> {  
    @Override  
    public int compare(Integer o1, Integer o2) {  
        return o1 - o2;  正的就是升序 负的就是降序
    }  
}

```

### 05 ArrayList 和 LinkedList对比
顺序表和链表的对比

|            | 原理  | 增加数据开销  | 扩容开销    | 查询数据位置开销 |
| ---------- | --- | ------- | ------- | -------- |
| ArrayList  | 数组  | 小（除第一个） | 大（需要拷贝） | 大（线性查找）  |
| LinkedList | 链表  | 大（除第一个） | 小       |          |



### 06 集合--HashSet

```JAVA

Hash 哈希算法，散列算法
HashSet底层是hash算法(hashcode)+数组+链表 hash算法会把重复hashcode的放到同一个位置，所以会被覆盖掉，所以hashset里面没有重复的数据。如果两个不一样的数据hashcode算出来的位置一样，那么会链在原来的那个位置上的数据后面

增加数据 add
修改数据 不能修改
删除数据 remove
查询数据 遍历

```

### 07 集合--Queue 

```java
Array + Blocking（堵塞） + Queue
ArrayBlockingQueue queue = new ArrayBlockingQueue();



```

### 08 集合--HashMap
HashMap 映射

```JAVA
HashMap底层也是 hash算法 + 数组 + 链表
HashMap map = new HashMap()
hashmap中的hash只针对key 也就是key不能相同


HashMap<Stirng,Interger> map = new HashMap<Stirng,Interger>();

添加数据 put 如果输入的key已经存在，那么会覆盖掉之前的key，返回旧的值。
		putIfAbsent 如果输入的key已经存在
查询数据 get 输入key返回Value
修改数据 replace


```


### 09 集合--Hashtable

Hashtable和HashMap区别
1 实现方式不一样：继承父类不一样
2 底层结构的容量不同 HashMap(16) Hashtable(11)
3 Hashtabel 的k，v不能为null
4 HashMap的数据定位采用的是Hash算法 Hashtable采用的是Hashcode
5 HashMap的性能较高 



### 10集合--迭代器
```java

public class Main {  
    public static void main(String[] args) {  
        HashMap<String, Integer> map = new HashMap<>();  
        map.put("a", 1);  
        map.put("b", 2);  
        map.put("c", 3);  
        Set<String> keys = map.keySet();  
        
        //如果在遍历map的过程移除数据会对map对象进行修改 导致后面的语法运行不了
        for (String key : keys) {  
            if("a".equals(key)) {  
                map.remove(key);  
            }  
            System.out.println(map.get(key));  
        }  
        
        所以使用迭代器来操作
        Iterator<String> iterator = keys.iterator();  
		while (iterator.hasNext()) {  
		    String key = iterator.next();  
		    if("a".equals(key)) {  
		        //remove方法只能对当前数据删除  
		        iterator.remove();  
		    }  
		}
    }  
    
    
}




```

### 11集合--工具类 工具类都是静态类 方法可以直接使用
```java

1 Arrays



```




## IO 数据流操作

数据 + 流(Stream)转 操作

数据 通过 阀门和管道 在不同地方流转

### 01文件流

Java无法直接访问文件 所有要通过 file 类来对关联文件 然后对文件进行操作

```java
public class Main {  
    public static void main(String[] args) throws IOException {  
          
        //创建文件对象 使用文件路径关联系统文件  
        String filePath = "C:\\Users\\kk\\Desktop\\java学习\\JavaWeb\\web-all\\test\\test.txt";  
        File file = new File(filePath);  
        //直接打印出来的是文件的路径  
        System.out.println(file);  
          
        //文件对象的操作  
        //file可能是文件也可能是文件夹 所以需要先判断是否为文件  
        if(file.exists()){  
            System.out.println("文件对象存在");  
            if(file.isFile()){  
                System.out.println("文件对象关联的是一个文件");  
                System.out.println(file.getName());  
                System.out.println(file.length());  
                System.out.println(file.getAbsolutePath());  
                  
            }else if(file.isDirectory()){  
                System.out.println(file.getName());  
                System.out.println(file.getAbsolutePath());  
                //遍历文件夹里面的文件名  
                String[] files = file.list();  
                for(String name : files){  
                    System.out.println(name);  
                }  
                //遍历文件夹里面的文件对象  
                File[] files1 = file.listFiles();  
                for (File listFile : files1) {  
                    System.out.println(listFile.getName());  
                }  
  
            }  
        }else {  
            System.out.println("文件不存在，创建新的文件");  
            file.mkdirs(); //创建文件目录  
            file.createNewFile();  
        }  
    }  
}
```


### 02管道流--文件复制

```java

public class Main {  
    public static void main(String[] args) throws IOException {  
  
        //01 创建文件对象 使用文件路径关联系统文件  
        String departure = "C:\\Users\\kk\\Desktop\\java学习\\JavaWeb\\web-all\\test\\test.txt";  
        //假设我们要生成一个复制文件  
        String destination = "C:\\Users\\kk\\Desktop\\java学习\\JavaWeb\\web-all\\test\\test.copy.txt";  
  
        //02 创建数据流传的管道  
//        FileInputStream in = new FileInputStream();  
//        FileOutputStream out = new FileOutputStream();  
        FileInputStream in = null;  
        FileOutputStream out = null;  
        //这里不能直接new 需要捕获异常  
        try {  
            //把文件对象放进去  
             in = new FileInputStream(departure);  
             out = new FileOutputStream(destination);  
             //打开阀门  
//            int data = in.read();  
//            // 每次传送一个字符就会关闭  
//            out.write(data);  
//            //            data = in.read();  
//            out.write(data);  
//  
//            data = in.read();  
//            out.write(data);  
            //如果文件读取到最后 那就是-1  
            int data = -1;  
            while ((data = in.read()) != -1) {  
                out.write(data);  
            }  
        }catch (IOException e){  
            throw new RuntimeException(e);  
        }finally {  
            //关闭阀门  
            if (in != null) {  
                in.close();  
            }  
            if (out != null) {  
                out.close();  
            }  
        }  
  
    }  
}
```

### 03缓冲流 提高每次读取的效率

在管道和开关之间加上一个缓冲区
```java
public class Main {  
    public static void main(String[] args) throws IOException {  
  
        //01 创建文件对象 使用文件路径关联系统文件  
        String departure = "C:\\Users\\kk\\Desktop\\java学习\\JavaWeb\\web-all\\test\\test.txt";  
        //假设我们要生成一个复制文件  
        String destination = "C:\\Users\\kk\\Desktop\\java学习\\JavaWeb\\web-all\\test\\test.copy.txt";  
  
        //02 创建数据流传的管道对象  
        FileInputStream in = null;  
        FileOutputStream out = null;  
  
        //03 创建缓冲输入输出流  
        BufferedInputStream buffIn = null;  
        BufferedOutputStream buffOut = null;  
  
        //04 缓冲区 水桶  
        byte[] cache = new byte[1024];  
  
        try {  
            //把文件对象放进开关  
            in = new FileInputStream(departure);  
            out = new FileOutputStream(destination);  
            //把文件对象放进缓冲管道  
            buffIn = new BufferedInputStream(in);  
            buffOut = new BufferedOutputStream(out);  
  
            //如果文件读取到最后 那就是-1  
            int data = -1;  
            while ((data = buffIn.read(cache)) != -1) {  
                buffOut.write(cache,0,data);  
            }  
  
  
        }catch (IOException e){  
            throw new RuntimeException(e);  
        }finally {  
            //关闭阀门  
            if (buffIn != null) {  
                buffIn.close();  
            }  
            if (buffOut != null) {  
                buffOut.close();  
            }  
        }  
  
    }  
}
```

### 04 字符流 比字节流更加高效 可以按行读取文本数据

```java
public class Main {  
    public static void main(String[] args) throws IOException {  
  
        //01 创建文件对象 使用文件路径关联系统文件  
        String departure = "C:\\Users\\kk\\Desktop\\java学习\\JavaWeb\\web-all\\test\\test.txt";  
        //假设我们要生成一个复制文件  
        File destination = new File("C:\\Users\\kk\\Desktop\\java学习\\JavaWeb\\web-all\\test\\test.copy.txt");  
  
        //02 字符输入输出流管道对象  
        BufferedReader reader = null;  
        PrintWriter writer = null;  
  
        try {  
            //把文件对象放进开关  
            reader = new BufferedReader(new FileReader(departure));  
            writer = new PrintWriter(destination);  
  
            String line = null;  
            while ((line = reader.readLine()) != null) {  
                System.out.println(line);  
                writer.println(line);  
            }  
            writer.flush();  
        }catch (IOException e){  
            throw new RuntimeException(e);  
        }finally {  
            //关闭阀门  
            if (reader != null) {  
                reader.close();  
            }  
            if (writer != null) {  
                writer.close();  
            }  
        }  
    }  
}

```


### 05 序列化

序列化： 把对象变成字节 

```java
public class Main {  
    public static void main(String[] args) throws IOException {  
  
        File file = new File("C:\\Users\\kk\\Desktop\\java学习\\JavaWeb\\web-all\\test\\test.copy.txt");  
  
        // 输入输出流管道对象  
        ObjectOutputStream objOut = null;  
        FileOutputStream out = null;  
        try{  
            out = new FileOutputStream(file);  
            objOut = new ObjectOutputStream(out);  
  
            //java中只有增加了特殊标记的类才能在写文件时进行序列化  
            //这里的标记其实就是一个接口  
            User user = new User();  
            objOut.writeObject(user);  
            objOut.flush();  
  
  
        } catch (IOException e){  
            throw new RuntimeException(e);  
        } finally {  
  
        }  
    }  
}  
class User implements Serializable{  
  
}

```


## 线程 

进程就是一个程序
线程就是一个程序里面的某个功能

java程序在运行中默认会产生一个进程
这个进程会有一个主线程 代码都在主线程中执行

```Java
public class Main {  
    public static void main(String[] args) throws IOException {  
  
        // 创建线程  
        MyThread thread = new MyThread();  
        // 启动线程  
        thread.start();  
        System.out.println(Thread.currentThread().getName());  
        //main线程的优先级最高  
  
    }  
}  
  
class MyThread extends Thread {  
    public void run() {  
        System.out.println(Thread.currentThread().getName());  
    }  
}

输出
main
Thread-0
```

线程有五个状态（类似408里面操作系统的进程管理相关知识）

new -> runnable -> terminated

blocked 阻塞
waiting 等待
timed-waiting 定时等待


### 线程的执行方式 串行和并发

串行执行 多个线程链接成串，然后按照顺序执行
并行执行 多个线程互相独立 谁先拿到cpu 谁就运行
```java
public class Main {  
    public static void main(String[] args) throws Exception {  
  
        // 创建线程  
        MyThread t1 = new MyThread();  
        MyThread t2 = new MyThread();  
        // 启动线程  
        // 使用join在下一个线程启动前将当前线程加入到串行执行流里面  
        t1.start();  
        t1.join();  
        t2.start();  
        t2.join();  
        System.out.println(Thread.currentThread().getName());  
        System.out.println("main执行完毕");  
        //main线程的优先级最高  
    }  
}  
  
class MyThread extends Thread {  
    public void run() {  
        System.out.println(Thread.currentThread().getName());  
    }  
}
```

线程休眠 sleep方法

自定义线程
```java
1 继承线程父类
2 重写run方法
class MyThread extends Thread {  
    public void run() {  
        System.out.println(Thread.currentThread().getName());  
    }  
}
MyThread t1 = new MyThread();  

也可以直接用 () -> {逻辑} 的方法创建线程
Thread t1 = new Thread(() -> {System.out.println(Thread.currentThread().getName());
});  

还可以传递实现了runnable接口的类的对象 一般使用匿名类
Thread t1 = new Thread(new Runnable(){
			@Override
			public void run(){
				System.out.println(Thread.currentThread().getName());
			}
} );  
```


### 线程池

为了方便管理 不同的线程 可以用线程池去获取线程
线程池就是线程的容器，可以根据需要在启动时创建一个或多个线程对象

java有四种比较常见的线程池
```java

代码运行逻辑
线程池对象里面有多个线程 任务提交给线程池 线程池分配给空闲的线程

1 创建固定数量的线程对象
ExecutorService是线程服务对象

2 根据实际任务动态创建线程对象 也就是Executors.newFixedThreadPool(3)变成Executors.newCachedThreadPool();

public class Main {  
    public static void main(String[] args) throws Exception {  
  
        ExecutorService executorService = Executors.newFixedThreadPool(3);  
  
        for (int i = 0; i < 5; i++) {  
            executorService.submit(new Runnable() {  
                @Override  
                public void run() {  
                    System.out.println(Thread.currentThread().getName());  
                }  
            });  
        }  
    }  
}

3 单一线程 Executors.newSingleThreadExecutor();

4 定时调度线程 Executors.newScheduledThreadPool(4);

```

### 线程--同步

## 反射

```JAVA

user.getClass()获取自己的字节码对象
通过自己的对象可以获取自己对象的相关信息 父类 属性 接口等信息

public class Main {  
    public static void main(String[] args) throws Exception {  
        User user = new User();  
        Class<? extends User> aClass = user.getClass();  
    }  
}  
  
class User{  
      
}
```

### 类加载器
java中的类主要分为三种 不同的类会用不同的加载器加载
1 java核心库的类
2 jvm软件开发商 
3 自己写的类

三种类加载器
1 BootClassLoader ： 启动类加载器
2 PlatformClassLoader：平台类加载器
3 AppClassLoader：应用类加载器
