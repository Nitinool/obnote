
### 讨论过程
```jAVA
// 这是一个简单的“汽车”类
class Car {
    String color = "红色"; // 汽车有一个颜色属性
}

// 这是我们程序的主入口
public class Main {
    public static void main(String[] args) {
        int year = 2024; // 1. 定义一个基本类型的变量
        Car myCar = new Car(); // 2. 创建一个Car对象
    }
}
```

1 当程序启动 main方法被调用时 java虚拟机jvm会为main方法在栈上创建一个专属的栈帧 Stack Frame

2 执行 `int year = 2024;`  int是基本的数据类型 值很简单 所以变量名和数值2024直接被放在main的栈帧里面 

3 执行 `Car myCar = new Car();`
当jvm识别到new关键字的时候 会在堆这个大空间里寻找到一块地方创建一个Car的实例 这个实例里面包含了他的所有数据
myCar是一个对象应用(object reference) 他只存放对象的内存地址 
所以 myCar这个变量存放在栈里面 Car对象放在堆里面

4 main方法执行结束
结束后 main的栈帧会被清空 里面的变量都会消失
但是堆里面的Car对象并不会马上消失 它只是因为没有任何变量引用他而变成了孤儿对象  jvm的垃圾回收期 garbage collector会在稍后某个时间点发现并回收内存空间

如果在 main 方法的最后加上一行代码 Car anotherCar = myCar;，你觉得内存中会发生什么？是会创建一辆新的车，还是别的什么呢？

`anotherCar` 确实只是在**栈**上创建的一个新的引用变量，它保存的地址和 `myCar` 完全相同，都指向**堆**中唯一的那一个 `Car` 对象。

既然 `myCar` 和 `anotherCar` 都指向同一个对象，那么如果我们通过其中一个引用去修改对象，会发生什么呢？

```java
public class Main {
    public static void main(String[] args) {
        Car myCar = new Car();      // myCar 指向一个 "红色" 的车
        Car anotherCar = myCar;   // anotherCar 也指向同一个 "红色" 的车

        // 现在，我们通过 anotherCar 把车的颜色改了
        anotherCar.color = "蓝色";

        // 那么，当我们打印 myCar 的颜色时，会输出什么呢？
        System.out.println(myCar.color); 
    }
}
```

最后会打印出“蓝色”。
这个特性在Java（以及很多其他语言）中被称为“**引用传递**” (pass-by-reference)。当你把一个对象赋值给另一个变量时，你传递的只是一个引用（地址），而不是对象本身的一个全新副本。



String 的不可变特性 

```java
public class java01 {
    public static void main(String【】 args) {
        String s = "abc";
        test(s);
        System.out.println(s);
    }
    public static  void test(String s){
        s = "lisi";
    }
}
最后输出的s结果没有发生改变 
```
在Java中，除了`String`，还有一些常用的类型也是不可变的。最常见的一类就是**基本数据类型的包装类 (Primitive Wrapper Classes)**。

你还记得`int`, `double`, `boolean`这些基本类型吧？它们各自都有一个对应的“对象版本”，就是包装类：

- `int` -> `Integer`
    
- `long` -> `Long`
    
- `double` -> `Double`
    
- `boolean`-> `Boolean`
    

这些包装类的对象，和`String`一样，都是**不可变的**。一旦创建，它们内部封装的那个值就不能再被更改。那怎么修改呢？？

```java
String greeting = "Hello";

// 我们想把 greeting "修改" 成 "Hello World"
greeting = greeting + " World";
```
**背后发生的事情是：**

1. `greeting + " World"` 这个表达式会先在**堆**里创建一个**全新的**字符串对象，它的内容是 "Hello World"。
    
2. 然后，`=` 这个赋值操作会把 `greeting` 这个栈上的引用，从指向旧的 "Hello" 对象，改为指向这个全新的 "Hello World" 对象。
    
3. 旧的 "Hello" 对象如果不再有任何引用指向它，就会在未来被垃圾回收器清理掉。

int“感觉”上是不可变的，从最终效果来看，确实是这样。这正是一个非常关键且容易混淆的点。它们“不可变”的原因是完全不同的。

对于Integer这样的对象，我们说它不可变，是因为对象本身的设计（class design）不允许你修改它内部的值。它是一种特性。对于int这样的基本类型，它不是对象，无所谓“可变”或“不可变”的特性。它表现出这种效果，是因为它的工作方式是“值传递” (pass-by-value)。

也就是说方法得到的只是一个**值的副本**，因此，方法必须通过 `return` 语句，把计算出的**新值**明确地“交还”出来。方法外部的代码需要主动去**接收**这个返回的新值，并用它来**覆盖**原始变量的值


### 总结
当然，我们来把整个讨论浓缩成几个核心要点：

内存两大区域：堆 vs. 栈

栈 (Stack) 💾: 存取速度快，空间小。主要存放基本类型（如 int）和对象的地址（引用）。

堆 (Heap) 📦: 空间大，但管理稍慢。存放所有用 new 创建的对象实例。

两种传递方式：值传递 vs. 引用传递

当你把基本类型（如 int）传入方法时，是值传递。方法得到的是一个副本，修改副本不影响原始值。

当你把对象（如 Car, String）传入方法时，是引用传递。方法得到的是地址的副本，它和原始引用指向堆中同一个对象。

两种对象类型：可变 vs. 不可变

可变对象 (Mutable)，比如我们自己写的Car类。可以通过引用直接修改其内部属性（myCar.color = "蓝色"），这个修改是永久的。

不可变对象 (Immutable)，比如String和Integer。一旦创建就无法修改。任何“修改”操作实际上都是创建一个新对象。

核心操作模式

正因为上面的区别，当你需要“修改”一个基本类型或一个不可变对象时，正确的方法是：创建一个新值/新对象，并用 return 将其返回，然后重新赋值给原始变量。

而对于可变对象，你可以直接在方法内修改它的属性，这个修改会立即生效，通常不需要return。

你从最底层的内存，一直理解到了最上层的编程范式，把这些关键概念都串起来了。这是一个非常扎实的基础！