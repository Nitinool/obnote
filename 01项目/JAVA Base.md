## **第一章：面向对象基本概念**

面向对象编程（Object-Oriented Programming, OOP）是一种编程范式，它使用“对象”来设计软件和应用程序。它的核心思想是封装、继承和多态。

---

### **面向对象 - 封装 (Encapsulation)**

**【补充的标题】**

封装是面向对象的三大特性之一，也是最基础的特性。它指的是将数据（属性）和操作数据的方法（行为）捆绑在一起，形成一个独立的实体——类。同时，通过访问权限控制，隐藏对象的内部细节，只对外提供公共的访问方式。

这样做的好处是：

1. **安全性**：防止外部代码随意修改对象内部的状态。
    
2. **易用性**：调用者只需知道如何使用对象提供的公共方法，无需关心内部复杂的实现逻辑。
    
3. **可维护性**：当内部实现需要修改时，只要公共接口不变，就不会影响到外部的调用方。
    

**JavaBean 规范** 就是封装思想的最佳体现。一个标准的 JavaBean 类应该满足以下条件：

1. 类必须是公共的（`public`）。
    
2. 必须有一个公共的无参构造方法。
    
3. 属性（成员变量）应该是私有的（`private`）。
    
4. 为私有属性提供公共的（`public`）`getter` 和 `setter` 方法来访问。
    

**代码示例：**

Java

```
public class Student {
    // 1. 私有化属性
    private String name;
    private int age;

    // 2. 公共的无参构造方法
    public Student() {
    }

    // 3. 为私有属性提供公共的 getter 和 setter 方法
    public String getName() {
        return name;
    }

    public void setName(String name) {
        // 可以在 setter 中加入逻辑控制，保证数据合法性
        if (name == null || name.isEmpty()) {
            System.out.println("无效的姓名！");
        } else {
            this.name = name;
        }
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        if (age < 0 || age > 150) {
            System.out.println("年龄范围不正确！");
        } else {
            this.age = age;
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Student stu = new Student();
        // stu.name = "张三"; // 编译错误！因为 name 是 private 的，不能直接访问
        
        // 必须通过公共的 setter 方法设置值
        stu.setName("张三");
        stu.setAge(20);
        
        // 通过公共的 getter 方法获取值
        System.out.println("姓名: " + stu.getName() + ", 年龄: " + stu.getAge());
    }
}
```

---

### **面向对象 - 构造方法 (Constructor)**

构造方法（或构造器）是一种特殊的方法，专门用于创建和初始化对象。

1. **基本语法**：`访问修饰符 类名(参数列表) { ... }`
    
2. **核心特点**：
    
    - 方法名必须与类名完全相同。
        
    - 没有返回类型，连 `void` 关键字都不能有。
        
    - 在通过 `new` 关键字创建对象时，会自动被调用。
        
3. **默认构造方法**：如果你没有编写任何构造方法，Java 编译器会自动为你添加一个无参数的、方法体为空的公共构造方法。但一旦你定义了任何构造方法，编译器就不再提供默认的无参构造方法了。
    

**代码示例（纠正与完善）：**

Java

```
public class User {
    String username;
    int level;

    // 无参数的构造方法
    public User() {
        System.out.println("一个 User 对象被创建了（通过无参构造）！");
    }

    // 一个参数的构造方法
    public User(String name) {
        System.out.println("一个 User 对象被创建了（通过有参构造）！");
        this.username = name; // 使用 this 关键字区分成员变量和局部变量
    }

    // 多个参数的构造方法（构造方法重载）
    public User(String name, int level) {
        this(name); // 使用 this(..) 调用本类中已存在的其他构造方法，必须放在第一行
        this.level = level;
    }
    
    public void showInfo() {
        System.out.println("用户名: " + this.username + ", 等级: " + this.level);
    }
}

public class Main {
    public static void main(String[] args) {
        User user1 = new User(); // 调用无参构造
        user1.showInfo();

        User user2 = new User("zhangsan"); // 调用一个参数的构造
        user2.showInfo();

        User user3 = new User("lisi", 99); // 调用两个参数的构造
        user3.showInfo();
    }
}
```

- **纠正**：在有参构造中，当参数名与成员变量名相同时，必须使用 `this.成员变量名` 来明确指定是为对象的属性赋值，否则会产生歧义。
    
- **完善**：增加了 `this(...)` 的用法示例，它用于在一个构造方法中调用另一个构造方法，可以简化代码。`this(...)` 的调用必须是构造方法中的第一条语句。
    

---

### **面向对象 - 静态 `static`**

`static` 关键字用于修饰类的成员（属性和方法），表示这些成员是属于**类本身**的，而不是属于某个具体的对象实例。

1. **静态属性（类变量）**：
    
    - 被所有对象共享。内存中只有一份，存储在方法区。
        
    - 通常通过 `类名.属性名` 来访问。
        
2. **静态方法（类方法）**：
    
    - 可以直接通过 `类名.方法名` 调用，无需创建对象。
        
    - **重要限制**：静态方法中不能直接访问非静态的成员（属性或方法），也不能使用 `this` 或 `super` 关键字，因为静态成员在类加载时就存在了，而非静态成员需要对象创建后才存在。
        
3. **静态代码块**：
    
    - 使用 `static { ... }` 定义。
        
    - 在类第一次被加载到内存时执行，且只执行一次。
        
    - 通常用于初始化静态资源，如加载驱动、初始化静态变量等。
        

**代码示例（纠正与完善）：**

Java

```
// 文件名: UserUtil.java
public class UserUtil {
    // 静态属性，用于计数创建了多少个用户
    public static int userCount = 0;

    // 静态代码块，在类加载时执行一次
    static {
        System.out.println("UserUtil 类被加载了...");
        // 可以在这里进行一些静态资源的初始化
    }

    // 静态方法
    public static void showUserCount() {
        System.out.println("当前已创建的用户数量是: " + userCount);
        // System.out.println(this.name); // 编译错误！静态方法不能访问非静态成员
    }
    
    // 普通成员属性
    private String name;
    
    // 构造方法
    public UserUtil(String name) {
        this.name = name;
        userCount++; // 每创建一个对象，计数器加一
        System.out.println("用户 '" + name + "' 已创建。");
    }
}

// 文件名: Main.java
public class Main {
    public static void main(String[] args) {
        // 无需创建对象，直接通过类名调用静态方法和访问静态属性
        System.out.println("初始用户数: " + UserUtil.userCount);
        UserUtil.showUserCount();

        System.out.println("-------------------------");
        
        // 创建对象会触发构造方法
        UserUtil u1 = new UserUtil("张三");
        UserUtil u2 = new UserUtil("李四");

        System.out.println("-------------------------");

        // 再次访问静态属性，值已被所有对象共享和修改
        System.out.println("当前用户数: " + UserUtil.userCount);
        UserUtil.showUserCount(); // 也可以通过对象引用访问，但不推荐：u1.showUserCount();
    }
}
```

- **纠正**：你的示例代码语法有多处错误，如 `Public` 应为 `public`，方法定义缺少 `()`，调用语句 `new User01；` 多了分号，`print` 应为 `System.out.println` 等。以上代码已全部修正为可运行的 Java 代码。
    

---

### **面向对象 - 继承 `extends`**

继承是面向对象的核心特性之一，它允许一个类（子类/派生类）获取另一个类（父类/基类）的属性和方法。Java 中使用 `extends` 关键字实现继承，且只支持单继承（一个子类只能有一个直接父类）。

**关键字 `this` 和 `super`**：

- `this`：代表**当前对象**的引用。
    
    - `this.属性` / `this.方法()`：调用当前对象的属性或方法（当局部变量与成员变量重名时用以区分）。
        
    - `this()`：调用本类的其他构造方法。
        
- `super`：代表**父类对象**的引用（在子类中）。
    
    - `super.属性` / `super.方法()`：调用父类的属性或方法（当子类重写了父类方法或隐藏了父类属性时使用）。
        
    - `super()`：调用父类的构造方法。
        

**构造过程**：

- 创建任何一个子类对象时，程序会首先调用其父类的构造方法，以确保父类的成员已经被正确初始化。
    
- 子类的构造方法中，第一行总是**隐式地**含有一个 `super()` 调用，去调用父类的无参构造。
    
- 如果你想调用父类的有参构造，必须在子类构造方法的第一行**显式地**写出 `super(参数)`。
    
- `super(...)` 和 `this(...)` 都必须是构造方法的第一句，因此它们不能同时存在。
    

**代码示例（纠正与完善）：**

Java

```
// 父类
class Parent {
    String name;
    int money = 1000;

    // 父类提供了有参构造方法
    public Parent(String name) {
        // 当参数名和成员变量名相同时，必须用 this 区分
        // 你原来的写法 parent(String name){ name = name; } 是错误的，这是局部变量给自己赋值
        this.name = name;
        System.out.println("父类 Parent 的构造方法被调用, name=" + this.name);
    }

    public void sayHello() {
        System.out.println("Hello, 我是父类, 我的名字是 " + this.name);
    }
}

// 子类
class Child extends Parent {
    String name = "lisi"; // 与父类属性同名，这叫“隐藏”
    int money = 10;

    public Child() {
        // 因为父类没有无参构造，所以必须显式调用 super(...)
        super("zhangsan_from_child"); // 调用父类的有参构造
        System.out.println("子类 Child 的构造方法被调用");
    }

    @Override // 建议加上注解，表示这是方法重写
    public void sayHello() {
        System.out.println("Hi, 我是子类, 我的名字是 " + this.name);
    }

    public void showInfo() {
        // 调用子类自己的属性
        System.out.println("子类的名字 (this.name): " + this.name);
        // 调用父类的属性
        System.out.println("父类的名字 (super.name): " + super.name);

        // 调用子类自己的方法 (因为已经重写)
        this.sayHello();
        // 显式调用父类被重写的方法
        super.sayHello();
        
        System.out.println("我有 " + this.money + " 元，继承了 " + super.money + " 元。");
    }
}

public class Main {
    public static void main(String[] args) {
        Child child = new Child();
        System.out.println("-------------------");
        child.showInfo();
    }
}
```

---

### **面向对象 - 多态 (Polymorphism)**

多态是继封装、继承之后的第三大特性，它意味着“多种形态”。具体表现为：**父类的引用可以指向子类的对象**。

`父类类型 引用名 = new 子类类型();`

**多态的特点**：

1. **编译时看左边（引用类型）**：当使用多态方式调用方法时，编译器只会检查引用类型（父类）中是否声明了该方法。如果没有，则编译失败。这就是你笔记中“以父类声明的子类 只能使用父类方法”的正确理解。
    
2. **运行时看右边（实际对象类型）**：当程序运行时，如果子类重写了该方法，那么实际执行的是子类重写后的版本。这被称为**动态绑定**。
    

**向下转型**：如果确实需要调用子类特有的方法，需要将父类引用强制转换回子类类型，这称为“向下转型”。为了安全，转型前最好使用 `instanceof` 关键字进行检查。

**代码示例（纠正与完善）：**

Java

```
class Person {
    public void say() {
        System.out.println("一个人在说话。");
    }
}

class Boy extends Person {
    @Override
    public void say() {
        System.out.println("一个男孩在大声说话！");
    }

    // Boy 类特有的方法
    public void playGames() {
        System.out.println("男孩正在玩游戏。");
    }
}

class Girl extends Person {
    @Override
    public void say() {
        System.out.println("一个女孩在轻声说话。");
    }

    // Girl 类特有的方法
    public void shopping() {
        System.out.println("女孩正在购物。");
    }
}

public class Main {
    public static void main(String[] args) {
        // 这就是多态：父类引用指向子类对象
        Person p1 = new Boy();
        Person p2 = new Girl();

        // 编译时看左边：p1 和 p2 的类型是 Person，Person 类有 say() 方法，编译通过。
        // 运行时看右边：
        p1.say(); // p1 实际指向 Boy 对象，所以执行 Boy 类重写后的 say()
        p2.say(); // p2 实际指向 Girl 对象，所以执行 Girl 类重写后的 say()

        // p1.playGames(); // 编译错误！因为 Person 类没有声明 playGames() 方法。
        
        // 如果想调用子类特有方法，需要向下转型
        if (p1 instanceof Boy) {
            Boy boy = (Boy) p1; // 强制类型转换
            boy.playGames();
        }
        
        if (p2 instanceof Girl) {
            Girl girl = (Girl) p2;
            girl.shopping();
        }
    }
}
```

---

### **面向对象 - 重载 (Overloading) 与 重写 (Overriding)**

这两个概念非常重要，也容易混淆。

**方法重载 (Overloading)**：

- **定义**：在**同一个类**中，允许存在一个以上的同名方法，只要它们的**参数列表不同**（参数个数、类型或顺序不同）。
    
- **目的**：方便调用者，用同样的方法名处理不同类型的数据。
    
- **规则**：与方法的返回值类型、访问修饰符无关。
    
- **示例**：构造方法就是典型的重载。
    

Java

```
class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    // 与上一个方法构成重载（参数个数不同）
    public int add(int a, int b, int c) {
        return a + b + c;
    }

    // 与上两个方法构成重载（参数类型不同）
    public double add(double a, double b) {
        return a + b;
    }
}
```

**方法重写 (Overriding)**：

- **定义**：在**子类**中，可以创建一个与**父类**中某个方法具有**相同方法签名**（方法名、参数列表完全相同）的新方法。
    
- **目的**：子类根据自己的需求，重新实现父类的方法，以表现出不同的行为。
    
- **规则**（“两同两小一大”）：
    
    1. `两同`：方法名、参数列表必须相同。
        
    2. `两小`：子类方法的返回值类型应小于或等于父类方法；子类方法抛出的异常应小于或等于父类方法。
        
    3. `一大`：子类方法的访问权限应大于或等于父类方法（`public` > `protected` > `default` > `private`）。
        
- **注解**：建议在重写方法前加上 `@Override` 注解，编译器会帮助检查重写是否正确。
    

**你的 `CCC` 和 `DDD` 示例分析（非常经典！）：** 这个例子完美地展示了多态中方法的动态绑定。

Java

```
class CCC {
    int i = 10;
    
    int sum() {
        // 第2步：执行 CCC 的 sum 方法
        // 第3步：调用 getI() 方法。此时要看 this 是谁，this 是 new DDD()，
        // 所以 JVM 会去 DDD 类里找 getI() 方法
        return getI() + 10;
    }
    
    int getI() {
        return i;
    }
}

class DDD extends CCC {
    int i = 20;
    
    // 子类重写了 getI 方法
    @Override
    int getI() {
        // 第4步：执行 DDD 的 getI 方法，返回的是 DDD 里的 i
        return i;
    }
}

public class Main {
    public static void main(String[] args) {
        // 第1步：创建对象，父类引用指向子类实例
        CCC ccc = new DDD();
        // 调用 sum()。编译器检查 CCC 类，有 sum() 方法，通过。
        // 运行时，实际对象是 DDD，但 DDD 没有重写 sum()，所以还是调用父类 CCC 的 sum()。
        int result = ccc.sum(); 
        
        // 第5步：sum() 返回 20 + 10 = 30
        System.out.println(result); // 输出 30
        
        // 注意：属性没有多态性，访问哪个属性由引用类型决定
        System.out.println(ccc.i); // 输出 10，因为引用 ccc 的类型是 CCC
    }
}
```

- **总结**：**方法调用看对象，属性访问看引用**。`ccc.sum()` 的执行流程是：`CCC.sum()` -> `DDD.getI()` -> 返回 `DDD.i` 的值 `20` -> `20 + 10 = 30`。你的理解是正确的，这里为你做了更详细的步骤分解和规则总结。
    

---

### **面向对象 - 访问权限修饰符**

Java 通过四个关键字来控制类、方法、属性的访问范围：

|修饰符|同一个类 (`private`)|同一个包 (`default`)|不同包的子类 (`protected`)|任何地方 (`public`)|
|---|---|---|---|---|
|`private`|✔️|❌|❌|❌|
|`default`|✔️|✔️|❌|❌|
|`protected`|✔️|✔️|✔️|❌|
|`public`|✔️|✔️|✔️|✔️|

导出到 Google 表格

- **`private`**：最严格的权限，只能在当前类内部访问。常用于封装属性。
    
- **`default`** (默认，即不写任何修饰符)：包级私有，只能被同一个包下的类访问。
    
- **`protected`**：受保护的，除了具有 `default` 的访问权限外，还可以被不同包中的子类访问。
    
- **`public`**：最宽松的权限，可以在任何地方被访问。
    

**关于 `main` 方法 `public static void main(String[] args)` 的解释**：

- `public`：`main` 方法是程序的入口，必须是公共的，这样 JVM 才能在任何位置找到并调用它。
    
- `static`：JVM 启动程序时，不会创建 `main` 方法所在类的对象，而是直接通过类名来调用 `main` 方法。因此 `main` 必须是静态的。
    
- `void`：`main` 方法执行结束后，不会返回任何值给 JVM。
    
- `String[] args`：用于接收程序启动时从命令行传入的参数。
    

---

### **面向对象 - 内部类 (Inner Class)**

在一个类的内部定义的另一个类，称为内部类。内部类可以方便地访问其外部类的私有成员。

**【补充的标题】内部类的分类**

1. **成员内部类（非静态）**：
    
    - 作为外部类的一个成员存在，可以访问外部类的所有成员（包括私有的）。
        
    - 依赖于外部类的实例存在，必须先创建外部类对象，然后通过外部类对象来创建内部类对象。
        
2. **静态内部类**：
    
    - 使用 `static` 修饰的内部类。
        
    - 不依赖于外部类的实例，可以直接创建。
        
    - 只能访问外部类的静态成员。
        
3. **局部内部类**：
    
    - 定义在方法或代码块内部的类。
        
    - 作用范围仅限于该方法或代码块。
        
4. **匿名内部类**：
    
    - 没有名字的内部类，通常用于简化代码，常用于创建接口或抽象类的实例。
        

**代码示例（纠正与完善）：**

Java

```
// 外部类
public class OuterClass {
    private int outerVar = 10;
    private static int staticOuterVar = 20;

    // 1. 成员内部类
    public class InnerClass {
        public void display() {
            // 可以访问外部类的私有成员和静态成员
            System.out.println("InnerClass: outerVar = " + outerVar);
            System.out.println("InnerClass: staticOuterVar = " + staticOuterVar);
        }
    }
    
    // 2. 静态内部类
    public static class StaticInnerClass {
        public void display() {
            // 只能访问外部类的静态成员
            // System.out.println("StaticInnerClass: outerVar = " + outerVar); // 编译错误！
            System.out.println("StaticInnerClass: staticOuterVar = " + staticOuterVar);
        }
    }

    // 方法，包含局部内部类和匿名内部类
    public void test() {
        // 3. 局部内部类
        class LocalInnerClass {
            void show() {
                System.out.println("LocalInnerClass: outerVar = " + outerVar);
            }
        }
        LocalInnerClass lic = new LocalInnerClass();
        lic.show();
        
        // 4. 匿名内部类 (实现一个Runnable接口)
        Runnable r = new Runnable() {
            @Override
            public void run() {
                System.out.println("Anonymous Inner Class is running...");
            }
        };
        new Thread(r).start();
    }
}

public class Main {
    public static void main(String[] args) {
        // 创建成员内部类对象
        OuterClass outer = new OuterClass();
        OuterClass.InnerClass inner = outer.new InnerClass();
        inner.display();

        System.out.println("--------------------");

        // 创建静态内部类对象
        OuterClass.StaticInnerClass staticInner = new OuterClass.StaticInnerClass();
        staticInner.display();

        System.out.println("--------------------");

        // 调用包含局部和匿名内部类的方法
        outer.test();
    }
}
```

- **纠正**：你的内部类定义 `private InnerClass{}` 语法错误，应为 `private class InnerClass {}`。以上代码展示了更完整的内部类用法。
    

---

### **面向对象 - 单例设计模式 (Singleton Pattern)**

**【补充的标题】**

单例模式是一种创建型设计模式，它确保一个类**只有一个实例**，并提供一个全局访问点来获取这个实例。

**应用场景**：配置管理器、数据库连接池、日志对象等，这些场景中，拥有多个实例会造成资源浪费或逻辑混乱。

**实现方式主要有两种：**

1. **饿汉式 (Eager Initialization)**：
    
    - **做法**：在类加载时就直接创建实例。
        
    - **优点**：实现简单，线程安全。
        
    - **缺点**：无论是否使用，实例都会被创建，可能造成资源浪费。
        
    
    Java
    
    ```
    public class SingletonEager {
        // 1. 私有化构造方法，防止外部 new
        private SingletonEager() {}
    
        // 2. 在类内部直接创建好唯一的实例
        private static final SingletonEager INSTANCE = new SingletonEager();
    
        // 3. 提供一个公共的静态方法，返回这个实例
        public static SingletonEager getInstance() {
            return INSTANCE;
        }
    
        public void showMessage() {
            System.out.println("这是一个饿汉式单例。");
        }
    }
    ```
    
2. **懒汉式 (Lazy Initialization)**：
    
    - **做法**：在第一次调用 `getInstance()` 方法时才创建实例。
        
    - **优点**：实现了延迟加载，节省资源。
        
    - **缺点**：在多线程环境下，需要处理线程安全问题。
        
    
    **线程安全的懒汉式（双重检查锁定 - Double-Checked Locking）**
    
    Java
    
    ```
    public class SingletonLazy {
        // 1. 私有化构造方法
        private SingletonLazy() {}
    
        // 2. 声明一个 volatile 的静态实例变量，但不立即初始化
        // volatile 保证了多线程间的可见性和禁止指令重排序
        private static volatile SingletonLazy instance;
    
        // 3. 提供公共的静态方法
        public static SingletonLazy getInstance() {
            // 第一次检查：如果实例已存在，直接返回，避免不必要的同步开销
            if (instance == null) {
                // 同步代码块，保证同一时间只有一个线程能进入
                synchronized (SingletonLazy.class) {
                    // 第二次检查：防止多个线程同时通过第一次检查后重复创建实例
                    if (instance == null) {
                        instance = new SingletonLazy();
                    }
                }
            }
            return instance;
        }
    
        public void showMessage(){
            System.out.println("这是一个线程安全的懒汉式单例。");
        }
    }
    ```
    

---

### **面向对象 - `final` 关键字**

`final` 关键字代表“最终的、不可改变的”。它可以修饰类、方法和变量。

1. **修饰变量**：
    
    - **基本类型变量**：值不能被改变，成为一个**常量**。
        
    - **引用类型变量**：引用的**地址**不能被改变（不能再指向另一个对象），但该引用指向的**对象内容是可以修改的**。
        
2. **修饰方法**：
    
    - 该方法**不能被子类重写**（Override）。
        
3. **修饰类**：
    
    - 该类**不能被继承**。例如 `String` 类就是 `final` 类。
        

**代码示例：**

Java

```
public final class FinalClassExample { // 这个类不能被继承
    
    // final 变量必须在声明时或构造方法中初始化
    public final int CONSTANT_VALUE = 100;
    public final String GREETING;
    
    public FinalClassExample() {
        this.GREETING = "Hello";
    }

    public final void showMessage() { // 这个方法不能被重写
        System.out.println(GREETING + ", a final method.");
    }

    public static void main(String[] args) {
        FinalClassExample ex = new FinalClassExample();
        // ex.CONSTANT_VALUE = 200; // 编译错误！final 变量不能被修改
        
        final StringBuilder builder = new StringBuilder("initial");
        // builder = new StringBuilder("new"); // 编译错误！final 引用不能指向新对象
        builder.append("-modified"); // 正确！可以修改引用指向的对象内容
        System.out.println(builder.toString()); // 输出 initial-modified
    }
}
```

---

### **面向对象 - 抽象类 `abstract` 和接口 `interface`**

**抽象类 (`abstract class`)**：

- **定义**：被 `abstract` 关键字修饰的类。它是一种“不完整”的类，不能被实例化（不能 `new`）。
    
- **目的**：作为一个模板，为子类提供一个共同的结构，并强制子类实现某些方法。
    
- **特点**：
    
    - 可以包含**抽象方法**（只有声明，没有方法体，用 `abstract` 修饰），也可以包含普通方法。
        
    - 子类继承抽象类后，**必须实现**父类中所有的抽象方法，除非子类自己也是一个抽象类。
        
    - 可以有构造方法（供子类调用 `super()`）、成员变量等。
        

**接口 (`interface`)**：

- **定义**：比抽象类更彻底的“抽象”，它是一种**行为规范或契约**。
    
- **目的**：定义一组所有实现类都必须遵守的方法。
    
- **特点 (JDK 8 之前)**：
    
    - 只能包含**常量**（`public static final`，可省略）和**抽象方法**（`public abstract`，可省略）。
        
    - 一个类可以通过 `implements` 关键字**实现多个接口**，弥补了 Java 单继承的不足。
        
- **JDK 8+ 新特性**：接口中可以包含 `default`（默认）方法和 `static`（静态）方法，这些方法可以有方法体。
    

**代码示例：**

Java

```
// 抽象类：定义了一个“动物”的模板
abstract class Animal {
    private String name;

    public Animal(String name) {
        this.name = name;
    }

    // 抽象方法：吃，具体怎么吃由子类实现
    public abstract void eat();

    // 普通方法：所有动物都会呼吸
    public void breathe() {
        System.out.println(name + " 正在呼吸...");
    }
}

// 接口：定义了“会飞”这个行为规范
interface Flyable {
    // 接口中的方法默认是 public abstract
    void fly();
}

// Dog 类继承了 Animal，必须实现 eat 方法
class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }

    @Override
    public void eat() {
        System.out.println("小狗 '" + super.name + "' 正在啃骨头。");
    }
}

// Bird 类继承 Animal，同时实现了 Flyable 接口
class Bird extends Animal implements Flyable {
    public Bird(String name) {
        super(name);
    }
    
    @Override
    public void eat() {
        System.out.println("小鸟 '" + super.name + "' 正在吃虫子。");
    }

    @Override
    public void fly() {
        System.out.println("小鸟 '" + super.name + "' 在天上飞。");
    }
}
```

- **注意**：`abstract` 和 `final` 不能同时使用，因为 `abstract` 要求被继承和实现，而 `final` 禁止继承。`abstract` 和 `private` 不能同时修饰一个方法，因为 `private` 的方法子类不可见，无法重写。
    

---

### **面向对象 - 枚举 `enum`**

枚举 (`enum`) 是一个特殊的类，它用来表示一组固定的常量。使用枚举可以增强代码的可读性和类型安全。

- **本质**：每一个枚举常量（如 `City.BEIJING`）都是 `enum` 类型的一个**公共、静态、final** 的实例。枚举类的构造方法是私有的，防止外部创建实例。
    
- **优点**：
    
    - **类型安全**：限制了参数只能是预定义的几个值之一。
        
    - **可读性强**：`Season.SPRING` 比 `int season = 1` 更清晰。
        
    - **功能强大**：可以有自己的属性和方法，可以实现接口。
        

**代码示例（完善）：**

Java

```
// 定义一个表示季节的枚举
public enum Season {
    // 1. 列出所有枚举实例，必须在最前面
    // 每个实例都会调用构造方法
    SPRING("春天", "温暖"),
    SUMMER("夏天", "炎热"),
    AUTUMN("秋天", "凉爽"),
    WINTER("冬天", "寒冷");

    // 2. 定义私有属性
    private final String chineseName;
    private final String description;

    // 3. 构造方法必须是 private 的（可省略 private）
    private Season(String chineseName, String description) {
        this.chineseName = chineseName;
        this.description = description;
    }

    // 4. 可以定义公共方法
    public String getChineseName() {
        return chineseName;
    }

    public void showDescription() {
        System.out.println(this.chineseName + "的特点是：" + this.description);
    }
}

public class Main {
    public static void main(String[] args) {
        // 获取一个枚举实例
        Season autumn = Season.AUTUMN;

        // 调用枚举的方法
        autumn.showDescription();

        // 遍历所有枚举实例
        System.out.println("-----------------");
        System.out.println("所有季节：");
        for (Season s : Season.values()) {
            System.out.println(s.name() + " -> " + s.getChineseName());
        }

        // switch 语句对枚举有很好的支持
        switch (autumn) {
            case SPRING:
                System.out.println("是春天");
                break;
            case AUTUMN:
                System.out.println("是秋天");
                break;
            default:
                System.out.println("是其他季节");
                break;
        }
    }
}
```

---

### **章节总结 (问答式)**

**Q1：`static` 关键字有什么用？为什么 `main` 方法是静态的？** **A1**：`static` 用于创建属于类本身而不是对象实例的成员（属性和方法）。静态成员在类加载时就存在，可以通过类名直接访问。`main` 方法是程序的入口，JVM 在运行程序时需要直接通过类名调用它，而此时还没有创建任何对象，所以 `main` 方法必须是静态的。

**Q2：什么是构造方法？`this()` 和 `super()` 有什么区别和联系？** **A2**：构造方法是用于创建和初始化对象的特殊方法，方法名与类名相同且无返回值。`this()` 用于调用本类的其他构造方法，`super()` 用于调用父类的构造方法。它们都必须放在构造方法的第一行，因此不能同时出现。子类构造方法会默认调用父类的 `super()`，除非显式指定了 `this()` 或其他的 `super(...)`。

**Q3：重载 (Overloading) 和重写 (Overriding) 的核心区别是什么？** **A3**：

- **重载**：同一个类中，方法名相同，参数列表不同。是编译时多态。
    
- **重写**：子类中，方法签名（方法名、参数列表）与父类完全相同。是运行时多态。
    

**Q4：谈谈你对多态的理解。** **A4**：多态是指父类引用可以指向子类对象。它的好处是提高了代码的扩展性和可维护性。其核心机制是：编译时，检查父类引用中是否有该方法；运行时，实际执行的是子类对象重写后的方法（动态绑定）。

**Q5：抽象类和接口有什么异同？** **A5**：

- **相同点**：都不能被实例化，都可以包含抽象方法，都可以被子类继承/实现。
    
- **不同点**：
    
    - **继承关系**：类只能单继承抽象类，但可以实现多个接口。
        
    - **成员类型**：抽象类可以有各种成员（普通方法、成员变量、构造方法等），而接口（JDK 8前）只能有常量和抽象方法。
        
    - **设计理念**：抽象类是对“is-a”关系的抽象（例如：狗 _是_ 动物），侧重于代码复用；接口是对“has-a”或“can-do”关系的抽象（例如：鸟 _能_ 飞），侧重于定义行为规范。
        

**Q6：创建子类对象时，父类对象被创建了吗？** **A6**：**是的，可以理解为父类的部分被创建并集成到了子类对象中**。更准确地说，当你 `new` 一个子类对象时，内存中只分配了一块空间来存放这一个对象。但这个对象的初始化过程是**从父类开始**的。子类的构造方法会首先调用父类的构造方法，完成父类部分的初始化（成员变量赋值等），然后才执行子类自己的构造逻辑。所以，子类对象内部包含了父类的所有成员，但它们共同组成一个完整的子类对象，而不是两个独立的对象。

---

### **章节小练习**

**练习1：图形计算器**

1. 创建一个抽象类 `Shape`，包含一个抽象方法 `getArea()` 用于计算面积，和一个普通方法 `getName()` 返回图形名称。
    
2. 创建两个子类 `Circle` (圆形) 和 `Rectangle` (矩形) 继承 `Shape`。
    
3. `Circle` 类有半径 `radius` 属性，并实现 `getArea()` 方法（面积 = pitimesradius2）。
    
4. `Rectangle` 类有宽 `width` 和高 `height` 属性，并实现 `getArea()` 方法（面积 = `width` * `height`）。
    
5. 在 `main` 方法中，使用多态创建一个 `Shape` 数组，分别放入一个 `Circle` 对象和一个 `Rectangle` 对象，并遍历数组，打印出每个图形的名称和面积。
    

**练习2：实现一个单例模式的配置读取器**

1. 创建一个 `ConfigurationReader` 类。
    
2. 使用**饿汉式**或**线程安全的懒汉式**实现该类的单例模式。
    
3. 在该类中添加一个 `getProperty(String key)` 方法，用于模拟读取配置。例如，如果 `key` 是 "database_url"，就返回 "jdbc:mysql://localhost:3306/mydb"。
    
4. 在 `main` 方法中，通过 `getInstance()` 获取两次 `ConfigurationReader` 的实例，并比较它们是否为同一个对象（`==`）。然后调用 `getProperty` 方法获取配置信息并打印。
## **第二章：常用基本类**

本章将介绍 Java 标准库中一些最基础、最核心的类。熟练掌握它们是进行日常开发的基础。

---

### **`Object` 类：万物之源**

`java.lang.Object` 类是 Java 中所有类的“根”，处于类继承层次结构的顶端。如果你创建的类没有明确指定父类，那么它将默认继承 `Object` 类。因此，`Object` 类中的方法对所有 Java 对象都适用。

以下是它最常用的几个方法：

#### **1. `toString()` 方法**

- **默认行为**：`Object` 类中 `toString()` 的默认实现是返回一个由“类名@十六进制哈希码”组成的字符串，例如 `Person@1b6d3586`。这个字符串通常被理解为对象的“内存地址”标识，虽然不完全等同于物理地址，但可以唯一标识一个对象。
    
- **重写目的**：默认的输出信息可读性很差。我们通常会重写（Override）这个方法，以便返回能清晰描述对象**状态（属性）**的字符串。当你直接使用 `System.out.println(someObject)` 时，Java 会自动调用该对象的 `toString()` 方法。
    

**代码示例（你的示例很好，这里保留并稍作说明）：**

Java

```
public class Person {
    String name = "zhangsan";
    int age = 10;

    // 重写 Object 类的 toString() 方法
    // 快捷键：在类中 Alt + Insert (或 a) -> toString()
    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

public class Main {
    public static void main(String[] args) {
        Person person = new Person();
        // 直接打印对象时，会自动调用其 toString() 方法
        System.out.println(person); // 输出: Person{name='zhangsan', age=10}

        // 等同于显式调用
        System.out.println(person.toString()); // 输出: Person{name='zhangsan', age=10}
    }
}
```

#### **2. `equals()` 方法**

- **`==` 与 `equals()` 的区别**：
    
    - `==`：对于**基本数据类型**，比较的是**值**是否相等。对于**引用数据类型**，比较的是两个引用是否指向**同一个内存地址**（即是否为同一个对象）。
        
    - `equals()`：`Object` 类的默认实现与 `==` 完全相同，也是比较内存地址。
        
- **重写目的**：在实际业务中，我们往往不关心两个引用是否指向同一个对象，而是关心它们所代表的**内容**是否相同（例如，两个 `Person` 对象的 `id` 相同，我们就认为它们是“同一个人”）。因此，需要重写 `equals()` 方法来定义我们自己的“相等”逻辑。
    

#### **3. `hashCode()` 方法**

- **作用**：返回该对象的哈希码值（一个 `int` 类型的整数）。哈希码主要用于提高基于哈希表的集合（如 `HashSet`, `HashMap`）的性能。
    
- **`equals()` 与 `hashCode()` 的协定**：
    
    1. 如果两个对象通过 `equals()` 方法比较后是相等的，那么它们的 `hashCode()` 方法**必须**返回相同的值。
        
    2. 如果两个对象的 `hashCode()` 返回值不同，那么它们通过 `equals()` 比较后**一定**不相等。
        
    3. 如果两个对象的 `hashCode()` 返回值相同，它们 `equals()` 比较**不一定**相等（这被称为哈希冲突）。
        
- **重写规则**：**当你重写 `equals()` 方法时，为了遵守这个协定，必须同时重写 `hashCode()` 方法。** 否则，当你把对象存入 `HashSet` 或 `HashMap` 时，可能会出现意想不到的错误（比如存不进去，或者取不出来）。
    

**重写 `equals()` 和 `hashCode()` 的综合示例：**

Java

```
import java.util.Objects;

public class Student {
    private long id;
    private String name;

    public Student(long id, String name) {
        this.id = id;
        this.name = name;
    }

    // 重写 equals 方法，业务逻辑：只要 id 相同，就认为是同一个学生
    // 快捷键：Alt + Insert -> equals() and hashCode()
    @Override
    public boolean equals(Object o) {
        // 1. 检查是否为同一个对象引用
        if (this == o) return true;
        // 2. 检查对象是否为 null 或类型不匹配
        if (o == null || getClass() != o.getClass()) return false;
        // 3. 向下转型，以便访问子类成员
        Student student = (Student) o;
        // 4. 比较核心属性
        return id == student.id;
    }

    // 根据 equals 中使用的属性来生成 hashCode
    @Override
    public int hashCode() {
        // 使用 Objects 工具类来生成哈希码，这是推荐的做法
        return Objects.hash(id);
    }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student(1001, "张三");
        Student s2 = new Student(1001, "张三");
        Student s3 = new Student(1002, "李四");

        System.out.println("s1 == s2 ? " + (s1 == s2));           // false, 因为是两个不同的对象
        System.out.println("s1.equals(s2) ? " + s1.equals(s2)); // true, 因为 id 相同

        System.out.println("s1.hashCode(): " + s1.hashCode());
        System.out.println("s2.hashCode(): " + s2.hashCode());
        System.out.println("s3.hashCode(): " + s3.hashCode());
    }
}
```

---

### **数组 (Array)**

数组是 Java 中一种基础的数据结构，它是一个**固定长度**的、用于存储**相同数据类型**元素的容器。

#### **1. 声明与创建**

数组的元素会被赋予其数据类型的默认值（`int` 为 0，`boolean` 为 `false`，引用类型为 `null`）。

Java

```
// 声明方式1 (推荐)
int[] nums; 
String[] names;

// 声明方式2
// int nums[];

// 创建方式1: 动态初始化（指定长度）
nums = new int[5]; // 创建一个长度为5的int数组，所有元素默认为0
names = new String[3]; // 创建一个长度为3的String数组，所有元素默认为null

// 创建方式2: 静态初始化（直接赋值）
int[] nums2 = {1, 2, 3, 4, 5};
String[] names2 = {"zhangsan", "lisi", "wangwu"};
```

#### **2. 数组的“增删改查”**

数组的**长度是不可变**的，因此没有真正意义上的“增加”或“删除”元素的方法。

- **查 (Access)**：通过索引直接访问，效率极高。索引从 `0` 开始。 `int firstNum = nums2[0]; // 访问第一个元素`
    
- **改 (Update)**：通过索引直接修改。 `nums2[0] = 100; // 修改第一个元素`
    
- **“增/删”的模拟**：通常是通过创建一个**新的、更大或更小**的数组，然后将旧数组的元素复制过去来实现的。这个过程比较低效，也是为什么在需要动态增删元素的场景下，我们更多地使用集合（如 `ArrayList`）。
    

#### **3. 二维数组**

二维数组本质上是一个“数组的数组”，即一个一维数组，它的每个元素又是一个一维数组。

Java

```
// 创建一个3行4列的二维数组
int[][] matrix = new int[3][4];

// 静态初始化
String[][] users = {
    {"001", "张三", "男"},
    {"002", "李四", "女"}
};

// 访问元素
System.out.println(users[0][1]); // 输出 "张三"
```

#### **【补充的标题】`java.util.Arrays` 工具类**

Java 提供了一个专门操作数组的工具类 `java.util.Arrays`，非常方便。

Java

```
import java.util.Arrays;

public class ArrayHelper {
    public static void main(String[] args) {
        int[] nums = {3, 1, 4, 63, 76, 32};
        
        // 1. 打印数组（非常常用）
        System.out.println("原始数组: " + Arrays.toString(nums));

        // 2. 排序
        Arrays.sort(nums);
        System.out.println("排序后数组: " + Arrays.toString(nums));

        // 3. 二分查找 (必须在排序后使用)
        int index = Arrays.binarySearch(nums, 63);
        System.out.println("63 的索引是: " + index); // 输出 4

        // 4. 数组复制
        int[] newNums = Arrays.copyOf(nums, 10); // 复制并扩展到长度10
        System.out.println("复制后的新数组: " + Arrays.toString(newNums));
    }
}
```

#### **练习代码纠正与完善**

**练习1: 九层妖塔** (你的逻辑是正确的，只是没有用到数组)

Java

```
// 你的代码是直接打印，没有使用二维数组存储。如果要使用数组，可以这么写：
public class Main {
    public static void main(String[] args) {
        int row = 9;
        String[][] tower = new String[row][2 * row - 1];
        
        // 填充数组
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < 2 * row - 1; j++) {
                if (j >= (row - 1 - i) && j <= (row - 1 + i)) {
                    tower[i][j] = "*";
                } else {
                    tower[i][j] = " ";
                }
            }
        }
        
        // 打印数组
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < 2 * row - 1; j++) {
                System.out.print(tower[i][j]);
            }
            System.out.println();
        }
    }
}
```

**练习2: 冒泡排序** (你的代码可以优化，减少不必要的比较)

Java

```
// 优化：每轮外层循环都会确定一个最大值并放到末尾，
// 所以内层循环的比较范围可以逐渐缩小。
public class BubbleSort {
    public static void main(String[] args) {
        int[] nums = {3, 1, 4, 63, 76, 32};
        
        // 外层循环控制轮数，总共需要 nums.length - 1 轮
        for (int j = 0; j < nums.length - 1; j++) {
            boolean swapped = false; // 优化：如果一轮下来没有发生交换，说明已经有序
            // 内层循环控制每轮的比较
            // -1 是因为要访问 i+1
            // -j 是因为每轮都会把一个最大数放到最后，无需再比较
            for (int i = 0; i < nums.length - 1 - j; i++) {
                if (nums[i] > nums[i + 1]) {
                    // 交换元素
                    int temp = nums[i];
                    nums[i] = nums[i + 1];
                    nums[i + 1] = temp;
                    swapped = true;
                }
            }
            if (!swapped) {
                break; // 提前结束
            }
        }
        System.out.println("冒泡排序后: " + Arrays.toString(nums));
    }
}
```

**练习3 & 4 代码实现：**

Java

```
// 练习3: 选择排序
public static void selectionSort(int[] nums) {
    for (int i = 0; i < nums.length - 1; i++) {
        int minIndex = i; // 假设当前元素是最小的
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[j] < nums[minIndex]) {
                minIndex = j; // 找到更小的，更新最小值的索引
            }
        }
        // 将找到的最小值与当前位置交换
        if (minIndex != i) {
            int temp = nums[i];
            nums[i] = nums[minIndex];
            nums[minIndex] = temp;
        }
    }
}

// 练习4: 二分查找（循环实现）
public static int binarySearch(int[] sortedNums, int target) {
    int left = 0;
    int right = sortedNums.length - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2; // 防止整数溢出
        if (sortedNums[mid] == target) {
            return mid;
        } else if (sortedNums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1; // 没有找到
}
```

---

### **`String` 类：不可变的字符序列**

`java.lang.String` 是 Java 中使用最频繁的类之一。

#### **【补充的标题】核心特性：不可变性 (Immutability)**

- **定义**：一个 `String` 对象一旦被创建，其内部的字符序列就**永远不能被改变**。
    
- **体现**：所有看似修改字符串的方法（如 `replace`, `substring`, `concat`）实际上都不是在原字符串上修改，而是**创建并返回一个全新的 `String` 对象**。
    
- **好处**：
    
    1. **安全**：因为不可变，所以可以安全地在多线程中共享，无需同步。
        
    2. **可哈希**：字符串的哈希码可以被缓存，因为值不会变，这使得 `String` 非常适合做 `HashMap` 的键。
        

#### **【补充的标题】`String` vs `StringBuilder` vs `StringBuffer`**

- **`String`**：不可变。适用于少量、简单的字符串拼接。
    
- **`StringBuilder`**：**可变**。适用于**单线程**环境中大量的、复杂的字符串拼接操作。效率最高。
    
- **`StringBuffer`**：**可变**、**线程安全**。适用于**多线程**环境中的字符串拼接。因为有同步锁，效率低于 `StringBuilder`。
    

**拼接操作的正确选择：**

Java

```
// 不推荐：会产生多个中间 String 对象
String result = "a" + "b" + "c"; // (编译器会优化为 "abc")
String s = "";
for (int i=0; i<1000; i++) {
    s += i; // 极度不推荐，循环内会创建大量无用 String 对象
}

// 推荐：使用 StringBuilder
StringBuilder sb = new StringBuilder();
for (int i=0; i<1000; i++) {
    sb.append(i);
}
String finalResult = sb.toString();
```

#### **常用方法示例（完善你的笔记）**

Java

```
public class StringDemo {
    public static void main(String[] args) {
        String s = "  Hello World, Hello Java!  ";
        
        // 比较
        System.out.println("equals 'hello world...': " + s.trim().equalsIgnoreCase("hello world, hello java!")); // true

        // 截取与分割
        String sub = s.trim().substring(6, 11); // 从索引6开始，到索引11之前
        System.out.println("substring(6, 11): " + sub); // World
        
        String[] parts = s.trim().split(", ");
        System.out.println("split by ', ': " + Arrays.toString(parts)); // [Hello World, Hello Java!]

        // 替换
        String replaced = s.replace("Hello", "Hi");
        System.out.println("replace: " + replaced); // "  Hi World, Hi Java!  "
        
        // 查找
        System.out.println("indexOf 'Java': " + s.indexOf("Java")); // 20
        System.out.println("lastIndexOf 'Hello': " + s.lastIndexOf("Hello")); // 14
        System.out.println("startsWith '  H': " + s.startsWith("  H")); // true
        System.out.println("contains 'World': " + s.contains("World")); // true
        
        // 其他
        System.out.println("length: " + s.length()); // 28
        System.out.println("charAt(3): " + s.charAt(3)); // e
        System.out.println("isEmpty: " + "".isEmpty()); // true
        System.out.println("trim: '" + s.trim() + "'"); // 'Hello World, Hello Java!'
    }
}
```

---

### **包装类 (Wrapper Classes)**

Java 为 8 个基本数据类型分别提供了对应的引用类型——包装类。

|基本数据类型|包装类|
|---|---|
|`boolean`|`Boolean`|
|`byte`|`Byte`|
|`short`|`Short`|
|`int`|`Integer`|
|`long`|`Long`|
|`char`|`Character`|
|`float`|`Float`|
|`double`|`Double`|

导出到 Google 表格

#### **为什么需要包装类？**

1. **让基本类型拥有对象特性**：Java 的集合框架（如 `ArrayList`, `HashMap`）只能存储对象，不能存储基本类型。
    
2. **表示 `null` 值**：包装类对象可以为 `null`，而基本类型不行。这在表示“缺失”或“未定义”的数据时非常有用（如数据库查询结果）。
    
3. **提供强大的工具方法**：包装类提供了很多静态方法，用于类型转换、获取最大/最小值等。
    

#### **自动装箱与拆箱 (Autoboxing/Unboxing)**

从 JDK 5 开始，Java 提供了自动转换机制，大大简化了代码。

- **自动装箱**：将基本类型自动转换为对应的包装类对象。
    
- **自动拆箱**：将包装类对象自动转换为对应的基本类型。
    

Java

```
// 自动装箱 (int -> Integer)
Integer a = 100; // 编译器会自动处理，等同于 Integer a = Integer.valueOf(100);

// 自动拆箱 (Integer -> int)
int b = a; // 等同于 int b = a.intValue();

// 包装类可以为 null，但拆箱时要注意 NullPointerException
Integer c = null;
// int d = c; // 运行时会抛出 NullPointerException!
```

#### **常用方法**

Java

```
// 1. 字符串与基本类型之间的转换
String numStr = "123";
int num = Integer.parseInt(numStr); // String -> int

String boolStr = "true";
boolean flag = Boolean.parseBoolean(boolStr); // String -> boolean

String intStr = String.valueOf(500); // int -> String
String doubleStr = Double.toString(3.14); // double -> String

// 2. 获取常量
System.out.println("int 的最大值: " + Integer.MAX_VALUE);
System.out.println("double 的最小值: " + Double.MIN_VALUE);
```

---

### **日期与时间类**

#### **1. 传统日期类（JDK 8 之前）**

- `java.util.Date`：表示一个特定的时间点，精确到毫秒。但其大部分方法已被废弃，因为它设计混乱（既能表示日期又能表示时间）、可变且不是线程安全的。
    
- `java.text.SimpleDateFormat`：用于格式化（日期 -> 文本）和解析（文本 -> 日期）。**注意：它是线程不安全的！**
    
- `java.util.Calendar`：一个抽象类，提供了更丰富的日期/时间操作方法，用于替代 `Date` 中许多已废弃的方法。
    

**代码示例（了解即可，不推荐在新代码中使用）：**

Java

```
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Calendar;

public class OldDateDemo {
    public static void main(String[] args) throws Exception {
        // 1. 创建 Date 对象
        Date now = new Date();
        System.out.println("当前时间 (Date): " + now);

        // 2. 格式化 Date
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String formattedDate = sdf.format(now);
        System.out.println("格式化后: " + formattedDate);

        // 3. 解析字符串为 Date
        String dateStr = "2025-01-01 10:30:00";
        Date parsedDate = sdf.parse(dateStr);
        System.out.println("解析后: " + parsedDate);
        
        // 4. 使用 Calendar
        Calendar cal = Calendar.getInstance();
        cal.setTime(now);
        cal.add(Calendar.DAY_OF_MONTH, 5); // 增加5天
        System.out.println("5天后: " + cal.getTime());
    }
}
```

#### **【补充的标题】2. 全新日期时间 API（JDK 8+，强烈推荐）**

`java.time` 包提供了一套全新的、设计优良的日期时间 API，解决了旧 API 的所有问题。

- **核心特性**：不可变、线程安全、设计清晰。
    
- **主要类**：
    
    - `LocalDate`：只表示日期（年-月-日），如 `2025-08-31`。
        
    - `LocalTime`：只表示时间（时:分:秒），如 `22:30:15`。
        
    - `LocalDateTime`：表示日期和时间，是上面两者的结合。
        
    - `DateTimeFormatter`：用于格式化和解析，是 `java.time` 版的 `SimpleDateFormat`，并且是**线程安全**的。
        

**代码示例：**

Java

```
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class NewDateDemo {
    public static void main(String[] args) {
        // 1. 获取当前日期和时间
        LocalDate today = LocalDate.now();
        LocalDateTime now = LocalDateTime.now();
        System.out.println("今天日期: " + today);
        System.out.println("当前时间: " + now);

        // 2. 创建指定的日期时间
        LocalDate specifiedDate = LocalDate.of(2025, 12, 25);
        System.out.println("指定日期: " + specifiedDate);
        
        // 3. 日期时间计算 (因为不可变，会返回新对象)
        LocalDate nextWeek = today.plusWeeks(1);
        System.out.println("一周后: " + nextWeek);

        // 4. 格式化与解析
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy年MM月dd日 HH:mm:ss");
        String formatted = now.format(formatter);
        System.out.println("格式化后: " + formatted);
        
        LocalDateTime parsed = LocalDateTime.parse("2026年01月01日 08:00:00", formatter);
        System.out.println("解析后: " + parsed);
    }
}
```

- **格式化符号**：`y`年, `M`月, `d`日, `H`时, `m`分, `s`秒。大小写敏感，`MM` 表示两位月份 `08`，`mm` 表示两位分钟 `05`。
    

---

### **章节总结 (问答式)**

**Q1：`==` 和 `equals()` 方法有什么区别？在什么情况下需要重写 `equals()`？** **A1**：`==` 对于引用类型比较的是内存地址，而 `equals()` 的默认行为和 `==` 一样。当我们需要根据对象的“内容”或业务逻辑来判断两个对象是否相等时，就需要重写 `equals()` 方法。同时，重写 `equals()` 时必须重写 `hashCode()`。

**Q2：为什么 `String` 类被设计成不可变的？在需要频繁修改字符串内容时应该使用什么？** **A2**：`String` 的不可变性带来了线程安全和可哈希的优点。因为字符串常量池和方法参数传递等场景都依赖于其不可变性来保证安全和性能。在需要频繁修改字符串内容的场景下，应使用 `StringBuilder`（单线程）或 `StringBuffer`（多线程）。

**Q3：Java 中的包装类有什么作用？什么是自动装箱和拆箱？** **A3**：包装类的主要作用是让基本数据类型可以作为对象来使用，以便存入集合或表示 `null` 值，并且它们提供了很多实用的静态方法。自动装箱是基本类型到包装类的自动转换，自动拆箱是包装类到基本类型的自动转换。

**Q4：数组的长度是可变的吗？如何“扩容”一个数组？** **A4**：不可变。数组一旦创建，其长度就固定了。所谓的“扩容”实际上是创建一个新的、更大的数组，然后通过循环或 `System.arraycopy()` / `Arrays.copyOf()` 等方法将旧数组的内容复制到新数组中。

**Q5：JDK 8 新的日期时间 API (`java.time`) 相对于旧的 `Date` 和 `Calendar` 有什么优势？** **A5**：主要优势有：1. **不可变性**，确保了线程安全。2. **API 设计清晰**，将日期(`LocalDate`)、时间(`LocalTime`)、日期时间(`LocalDateTime`)等概念明确分开。3. **操作方便**，提供了链式调用的 `plus`、`minus` 等方法，代码更易读。

---

### **章节小练习**

**练习1：用户信息统计**

1. 定义一个 `User` 类，包含 `id` (int) 和 `email` (String) 两个属性。
    
2. 重写 `User` 类的 `equals()` 和 `hashCode()` 方法，规定只要 `email` 相同，就认为是同一个用户。
    
3. 创建一个 `User` 数组，并存入几个 `User` 对象，其中部分对象的 `email` 相同但 `id` 不同。
    
4. 编写一个方法 `countUniqueUsers(User[] users)`，利用 `HashSet` 集合（提示：`HashSet` 依赖 `equals` 和 `hashCode` 来保证元素唯一性）来统计数组中有多少个不重复的用户，并返回数量。
    

**练习2：日期格式转换器** 编写一个方法 `String convertDateFormat(String dateString)`，该方法接收一个 "yyyy/MM/dd" 格式的日期字符串（例如 "2025/08/31"），并将其转换为 "yyyy年MM月dd日" 的格式（例如 "2025年08月31日"）返回。请分别使用 `java.time` API 和传统的 `SimpleDateFormat` 两种方式实现。


## **第三章：异常类 (Exception)**

### **1. 什么是异常？**

在程序运行过程中，可能会发生各种预料之外的事件，这些事件会中断程序正常的指令流，我们称之为**异常 (Exception)**。例如：试图除以零、打开一个不存在的文件、数组访问时下标越界等。

Java 的异常处理机制是一套强大的错误处理方案，它能让你的程序在出错时不会直接崩溃，而是有机会“捕获”这个错误并进行相应的处理，从而增强程序的**健壮性 (Robustness)**。

### **【补充的标题】异常的体系结构**

理解异常的继承体系至关重要。在 Java 中，所有异常类型的根类是 `java.lang.Throwable`。

`Throwable` 有两个主要的子类：

1. **`Error` (错误)**：
    
    - 表示 JVM 自身发生的严重问题，例如 `OutOfMemoryError` (内存溢出)、`StackOverflowError` (栈溢出)。
        
    - `Error` 是程序无法处理的，一旦发生，程序通常会终止。我们**不应该**也**不需要**去捕获 `Error`。
        
2. **`Exception` (异常)**：
    
    - 这是我们编程时主要关注和处理的。它表示程序本身可以处理的、可恢复的问题。
        
    - `Exception` 又分为两大类：
        
        - **Checked Exception (受检异常/编译时异常)**：
            
            - **定义**：除了 `RuntimeException` 及其子类之外的所有 `Exception`。例如 `IOException`、`SQLException`。
                
            - **特点**：Java 编译器会**强制**你必须处理这类异常。处理方式有两种：要么使用 `try-catch` 捕获，要么使用 `throws` 在方法签名上声明抛出。如果不处理，代码将无法通过编译。
                
        - **Unchecked Exception (非受检异常/运行时异常)**：
            
            - **定义**：`RuntimeException` 及其所有子类。例如 `NullPointerException` (空指针)、`ArithmeticException` (算术异常)、`ArrayIndexOutOfBoundsException` (数组越界)。
                
            - **特点**：编译器**不强制**你处理这类异常。它们通常是由程序中的逻辑错误（Bug）引起的。你可以选择捕获处理，也可以不处理。
                

---

### **2. 异常的处理：`try-catch-finally`**

这是 Java 中捕获并处理异常的核心语法。

- **`try` 块**：
    
    - 包裹可能会抛出异常的代码。
        
- **`catch` 块**：
    
    - 用于捕获并处理 `try` 块中抛出的特定类型的异常。
        
    - 可以有多个 `catch` 块，用于捕获不同类型的异常。
        
    - **捕获规则**：当有多个 `catch` 块时，必须将**子类异常放在前面，父类异常放在后面**。因为 Java 会自上而下匹配 `catch` 块，如果父类在前，子类异常将永远没有机会被捕获。
        
- **`finally` 块**：
    
    - 无论是否发生异常，`finally` 块中的代码**总会执行**（除非在 `try` 或 `catch` 中调用 `System.exit()` 退出虚拟机）。
        
    - **主要用途**：用于执行资源释放操作，如关闭文件流、数据库连接、网络连接等，确保资源一定会被清理。
        

**代码示例（完善你的示例）：**

Java

```
public class ExceptionHandlingDemo {
    public static void main(String[] args) {
        try {
            // 场景1: 算术异常
            int result = 10 / 0; 
            System.out.println("结果是: " + result);

            // 场景2: 空指针异常 (如果上面不出错，会执行到这里)
            String text = null;
            System.out.println(text.length());

        } catch (ArithmeticException e) {
            System.out.println("捕获到算术异常！");
            System.out.println("错误信息: " + e.getMessage()); // 通常只返回简短的错误描述，如 "/ by zero"
            // e.printStackTrace(); // 打印详细的异常堆栈信息，这是调试时最有用的
        
        } catch (NullPointerException e) {
            System.out.println("捕获到空指针异常！");
            System.out.println("错误原因: " + e.toString()); // 返回异常类型和信息

        } catch (Exception e) {
            // Exception 是所有编译和运行时异常的父类，可以捕获任何未被捕获的异常
            // 放在最后，作为“兜底”方案
            System.out.println("捕获到其他未知异常！");
            e.printStackTrace();

        } finally {
            // 无论是否发生异常，这里都会执行
            System.out.println("Finally 块：程序执行结束，进行资源清理。");
        }
        
        System.out.println("程序在异常处理后继续运行...");
    }
}
```

**异常对象的常用方法：**

- `String getMessage()`: 获取异常的简短描述信息。
    
- `String toString()`: 获取异常的类型和简短描述信息。
    
- `void printStackTrace()`: 打印异常的完整堆栈跟踪信息到标准错误流，是调试的最重要工具。
    

---

### **3. 异常的声明与抛出：`throws` 与 `throw`**

这里是你的困惑点，我们来彻底分清它们。

- **`throws` 关键字 (声明异常)**
    
    - **作用位置**：用在**方法签名**上，即方法的 `()` 和 `{` 之间。
        
    - **功能**：告诉方法的**调用者**：“我这个方法内部可能会抛出某种类型的**受检异常 (Checked Exception)**，但我自己不处理，谁调用我，谁就负责处理（用 `try-catch`）或者继续声明抛出（用 `throws`）。”
        
    - 它是一种“甩锅”机制，将处理异常的责任转移给上层调用者。
        
- **`throw` 关键字 (抛出异常)**
    
    - **作用位置**：用在**方法体内部**。
        
    - **功能**：这是一个**动作**。用于手动创建一个异常对象 (`new Exception(...)`) 并将其**抛出**。一旦 `throw` 语句被执行，方法会立即终止，并将异常抛给调用者。
        
    - 你可以抛出任何 `Throwable` 或其子类的实例。通常用于当代码逻辑检测到某个错误条件时，主动通知外界出错了。
        

**综合示例：**

Java

```
import java.io.FileNotFoundException;

public class ThrowVsThrows {

    // 使用 throws 关键字在方法签名上声明：
    // 这个方法可能会抛出 FileNotFoundException (这是一个受检异常)
    public static void readFile(String filePath) throws FileNotFoundException {
        if (!filePath.equals("C:/my-file.txt")) {
            // 使用 throw 关键字在方法内部主动抛出一个异常实例
            // 当条件不满足时，我们手动创建一个异常并把它“扔”出去
            throw new FileNotFoundException("文件路径不正确，找不到文件！");
        }
        System.out.println("文件读取成功！");
    }

    public static void main(String[] args) {
        // 因为 readFile 方法声明了 throws FileNotFoundException，
        // 所以调用它的时候，编译器强制我们必须处理这个异常。
        try {
            readFile("C:/some-other-path.txt");
        } catch (FileNotFoundException e) {
            System.out.println("在 main 方法中捕获到了异常！");
            e.printStackTrace();
        }
        
        System.out.println("\n--- 分割线 ---\n");
        
        try {
            readFile("C:/my-file.txt");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

**总结一下：**

- `throws` 是**声明**，写在方法签名上，告诉别人“我可能会出问题”。
    
- `throw` 是**动作**，写在方法体内部，表示“现在就出问题，把这个异常扔出去”。
    

---

### **4. 自定义异常 (Custom Exception)**

现在来解决你“没听懂”的自定义异常。

**为什么需要自定义异常？** Java 内置的异常（如 `NullPointerException`, `IOException`）只能描述技术层面的错误。但在复杂的业务逻辑中，我们需要描述更具体的业务错误，例如：

- 注册时用户名已存在 (`UsernameExistsException`)
    
- 取款时余额不足 (`InsufficientBalanceException`)
    
- 输入的年龄不合法 (`InvalidAgeException`)
    

使用自定义异常可以让你的代码更具可读性，错误定位也更精确。

**如何创建和使用自定义异常？** 创建自定义异常非常简单，只需三步：

1. **创建一个类**，让它继承自一个合适的异常类。
    
    - 如果希望它是**受检异常** (强制调用者处理)，就继承 `Exception`。
        
    - 如果希望它是**非受检异常** (不强制调用者处理)，就继承 `RuntimeException`。
        
2. **提供构造方法**，通常会提供一个无参构造和一个接收错误信息字符串的构造方法，并使用 `super()` 调用父类的构造方法。
    
3. 在你的业务代码中，当检测到特定错误时，使用 `throw new YourCustomException(...)` 将其抛出。
    

**代码示例：创建一个银行取款的自定义异常**

**第一步：创建自定义异常类 `InsufficientBalanceException`**

Java

```
// 这是一个受检异常，因为我们希望调用者必须处理“余额不足”的情况
public class InsufficientBalanceException extends Exception {

    // 无参构造方法
    public InsufficientBalanceException() {
        super(); // 调用父类 Exception 的无参构造
    }

    // 带有错误信息的构造方法
    public InsufficientBalanceException(String message) {
        super(message); // 调用父类 Exception 的有参构造
    }
}
```

**第二步：在业务类中使用这个异常**

Java

```
public class BankAccount {
    private String accountNumber;
    private double balance;

    public BankAccount(String accountNumber, double initialBalance) {
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
    }

    // 取款方法
    // 因为方法内部可能抛出受检异常，所以必须用 throws 声明
    public void withdraw(double amount) throws InsufficientBalanceException {
        System.out.println("尝试取款: " + amount);
        if (amount <= 0) {
            // 这里可以抛出一个运行时异常，因为这是调用者的逻辑错误
            throw new IllegalArgumentException("取款金额必须为正数！");
        }
        
        if (balance < amount) {
            // 当余额不足时，抛出我们自定义的异常
            String message = "余额不足！当前余额: " + balance + ", 需要取款: " + amount;
            throw new InsufficientBalanceException(message);
        }
        
        balance -= amount;
        System.out.println("取款成功！剩余余额: " + balance);
    }
}
```

**第三步：调用并处理异常**

Java

```
public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount("123456", 1000.0);

        try {
            // 第一次取款，成功
            account.withdraw(500.0);
            
            // 第二次取款，会抛出 InsufficientBalanceException
            account.withdraw(800.0);

        } catch (InsufficientBalanceException e) {
            System.err.println("操作失败，捕获到业务异常！"); // System.err 打印红色错误信息
            System.err.println("原因: " + e.getMessage());
        
        } catch (IllegalArgumentException e) {
            System.err.println("操作失败，捕获到参数异常！");
            System.err.println("原因: " + e.getMessage());
        
        } finally {
            System.out.println("感谢使用本银行服务。");
        }
    }
}
```

---

### **章节总结 (问答式)**

**Q1：`Error` 和 `Exception` 的根本区别是什么？** **A1**：`Error` 表示 JVM 级别的、程序无法恢复的严重错误，如内存溢出。`Exception` 表示程序本身可以处理的、可能恢复的错误，如文件未找到或空指针。我们只处理 `Exception`。

**Q2：受检异常 (Checked Exception) 和非受检异常 (Unchecked/Runtime Exception) 有什么区别？** **A2**：

- **受检异常**：编译器强制要求必须处理（用 `try-catch` 或 `throws`），否则编译不通过。通常用于表示外部环境导致的、调用者应该预料到的问题，如 `IOException`。
    
- **非受检异常**：编译器不强制处理。通常表示程序内部的逻辑错误（Bug），如 `NullPointerException`。
    

**Q3：`throw` 和 `throws` 关键字的区别是什么？** **A3**：

- `throws` 是方法**声明**的一部分，用于告诉调用者该方法可能抛出哪些受检异常。
    
- `throw` 是方法体内的**动作**，用于手动创建并抛出一个具体的异常实例。
    

**Q4：`finally` 块有什么作用？它一定会执行吗？** **A4**：`finally` 块主要用于资源清理，确保无论是否发生异常，某些代码都能得到执行。它几乎总会执行，唯一的例外是在 `try` 或 `catch` 块中执行了 `System.exit()` 来终止虚拟机。

**Q5：我们为什么要创建自定义异常？** **A5**：为了更精确地描述特定于业务逻辑的错误，使代码的可读性更强，错误处理也更有针对性。例如，用 `InsufficientBalanceException` 来表示余额不足，比用一个通用的 `Exception` 带有一串描述字符串要清晰得多。

---

### **章节小练习**

**练习1：健壮的除法计算器**

1. 编写一个程序，使用 `Scanner` 从控制台接收两个整数。
    
2. 计算并打印第一个数除以第二个数的结果。
    
3. 使用 `try-catch` 结构处理可能发生的两种异常：
    
    - 如果用户输入的不是整数，程序会抛出 `InputMismatchException`。
        
    - 如果第二个数为 0，程序会抛出 `ArithmeticException`。
        
4. 为这两种异常提供友好的提示信息给用户，并确保程序无论如何都能正常结束。
    

**练习2：用户注册功能**

1. 创建一个自定义的**受检异常** `InvalidUsernameException`。
    
2. 创建一个 `UserRegistry` 类，其中包含一个方法 `void register(String username, String password) throws InvalidUsernameException`。
    
3. 在 `register` 方法中，添加逻辑检查：
    
    - 如果 `username` 为 `null` 或长度小于 6，就 `throw new InvalidUsernameException("用户名长度不能小于6位！")`。
        
    - 如果检查通过，就打印 "用户 [username] 注册成功！"。
        
4. 在 `main` 方法中调用 `register` 方法，并使用 `try-catch` 来处理可能抛出的 `InvalidUsernameException`，将错误信息打印出来。
   
   
## **第四章：集合框架 (Collections Framework)**

### **1. 为什么需要集合？**

当程序需要处理一组**数量不确定**的对象时，我们就需要一个容器来存储它们。数组虽然能存储一组对象，但它有两大缺点：

1. **长度固定**：数组一旦创建，其长度就不能改变，无法动态增删。
    
2. **功能有限**：数组本身没有提供方便的添加、删除、查找等方法，需要我们自己实现，使用不便。
    

Java 集合框架（Java Collections Framework, JCF）提供了一套性能优良、使用方便的接口和类，专门用于解决这类问题。

### **【补充的标题】集合框架核心接口**

Java 集合框架主要由两大接口派生而来：

1. **`Collection`接口**：用于存储**单一元素的集合**。它是所有单值集合的“根”接口。
    
    - **`List`接口**：一个**有序**的集合，允许元素**重复**。可以通过索引访问元素。
        
        - 主要实现类：`ArrayList`, `LinkedList`。
            
    - **`Set`接口**：一个**无序**的集合，**不允许**元素**重复**。
        
        - 主要实现类：`HashSet`, `TreeSet`。
            
    - **`Queue`接口**：一个**队列**集合，通常遵循**先进先出 (FIFO)** 原则。
        
        - 主要实现类：`LinkedList`, `ArrayBlockingQueue`。
            
2. **`Map`接口**：用于存储**键值对 (key-value)** 的集合。`key` 是唯一的，`value` 可以重复。
    
    - 主要实现类：`HashMap`, `Hashtable`, `TreeMap`。
        

---

### **泛型 (Generics)**

在你深入了解各种集合类之前，必须先理解泛型。

- **作用**：泛型就像是给容器贴上一个标签，**限定**这个容器里只能存放什么类型的元素。
    
- **好处**：
    
    1. **类型安全**：在编译时就能检查出类型错误。如果你往 `ArrayList<String>` 里放一个 `Integer`，编译器会直接报错，而不是等到运行时才出问题。
        
    2. **代码优雅**：从集合中获取元素时，不再需要进行强制类型转换，代码更简洁、可读性更高。
        

**代码示例（你的理解非常正确，这里进行规范化）：**



```Java
class Person {}

public class GenericsDemo {
    public static void main(String[] args) {
        // 原始方式 (不推荐)
        ArrayList listWithoutGenerics = new ArrayList();
        listWithoutGenerics.add(new Person());
        listWithoutGenerics.add("这是一个字符串"); // 编译不报错，但逻辑上可能有风险

        // 从集合中取数据时，只能得到 Object 类型
        Object obj = listWithoutGenerics.get(0);
        // 需要强制类型转换，如果转换失败会抛出 ClassCastException
        Person p1 = (Person) obj;
        System.out.println("原始方式获取成功");

        // ----------------------------------------------------

        // 使用泛型 (推荐)
        ArrayList<Person> listWithGenerics = new ArrayList<>();
        listWithGenerics.add(new Person());
        // listWithGenerics.add("这是一个字符串"); // 编译错误！类型不匹配，保证了类型安全

        // 从集合中取数据时，直接得到指定类型
        Person p2 = listWithGenerics.get(0); // 无需强转
        System.out.println("泛型方式获取成功");
    }
}
```

---

### **`List` 接口详解**

#### **`ArrayList` (动态数组)**

- **底层原理**：基于**动态数组**实现。它封装了一个数组，并提供了动态扩容的能力。
    
- **核心特性**：
    
    - **查询快**：通过索引 `get(index)` 直接访问元素，时间复杂度为 O(1)，速度极快。
        
    - **增删慢**：在数组**中间或开头**添加/删除元素时，需要移动后续所有元素，时间复杂度为 O(n)，效率较低。但在**末尾**添加元素时，速度很快（均摊 O(1)）。
        
- **扩容机制**：
    
    1. 首次 `add` 时，若为空，会创建一个默认长度为 10 的内部数组。
        
    2. 当元素数量超过当前容量时，会创建一个新的、更大的数组（通常是原容量的 1.5 倍），然后将旧数组的元素全部复制到新数组中。这是一个开销较大的操作。
        

**代码示例：**

Java

```Java
import java.util.ArrayList;
import java.util.List;

public class ArrayListDemo {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();

        // 增
        list.add("Java");
        list.add("Python");
        list.add(0, "C++"); // 在指定索引位置添加

        // 查
        String firstElement = list.get(0); // "C++"
        int size = list.size(); // 3

        // 改
        list.set(2, "Go"); // 把 "Python" 改为 "Go"

        // 删
        list.remove(1); // 删除索引为 1 的 "Java"
        list.remove("Go"); // 直接删除指定的元素

        System.out.println(list); // 输出: [C++]
    }
}
```

#### **`LinkedList` (双向链表)**

- **底层原理**：基于**双向链表**实现。每个元素（节点）都记录着它的前一个元素和后一个元素。
    
- **核心特性**：
    
    - **增删快**：在**开头或末尾**添加/删除元素，只需改变指针指向，时间复杂度为 O(1)，非常快。在中间操作也比 `ArrayList` 快，因为它不需要移动元素，但需要先遍历到指定位置。
        
    - **查询慢**：不支持通过索引直接访问。`get(index)` 操作需要从头或尾开始一个一个地遍历，直到找到指定索引的元素，时间复杂度为 O(n)，效率很低。
        

**代码示例：**

Java

```Java
import java.util.LinkedList;

public class LinkedListDemo {
    public static void main(String[] args) {
        LinkedList<String> list = new LinkedList<>();

        // 增
        list.add("Apple");
        list.addFirst("Banana"); // 在头部添加
        list.addLast("Orange"); // 在尾部添加

        // 查
        String first = list.getFirst(); // "Banana"
        String last = list.getLast(); // "Orange"

        // 删
        list.removeFirst(); // 删除 "Banana"
        list.removeLast(); // 删除 "Orange"

        System.out.println(list); // 输出: [Apple]
    }
}
```

#### **`ArrayList` 与 `LinkedList` 对比 (纠正与完善)**

你的对比表格有些不准确，这里提供一个修正和更详细的版本。

|特性|`ArrayList` (动态数组)|`LinkedList` (双向链表)|选择场景|
|---|---|---|---|
|**底层结构**|数组|双向链表|-|
|**查询 (Get by index)**|**极快** (O(1))，直接通过索引定位。|**极慢** (O(n))，需要从头或尾遍历。|**需要频繁随机访问元素的场景**，用 `ArrayList`。|
|**添加/删除 (在末尾)**|**快** (均摊 O(1))，除非需要扩容。|**快** (O(1))，只需修改尾节点指针。|两者性能都很好。|
|**添加/删除 (在开头/中间)**|**慢** (O(n))，需要移动后面所有元素。|**相对较快** (O(1) 操作 + O(n) 遍历)，只需修改指针。|**需要频繁在头部或中间增删的场景**，用 `LinkedList`。|
|**内存占用**|较低，内存是连续的。可能会有部分预留空间造成浪费。|较高，每个节点除了数据外，还需存储前后指针。|对内存敏感的场景，`ArrayList` 略有优势。|

导出到 Google 表格

**结论**：在绝大部分场景下，**`ArrayList` 的性能都更好**，因为“查询”操作远比“在开头/中间增删”操作更常用。只有在你确定业务场景主要是**队列操作（在头尾增删）**时，才优先考虑 `LinkedList`。

---

### **`Set` 接口详解**

#### **`HashSet`**

- **底层原理**：基于 **`HashMap`** 实现。它将元素作为 `HashMap` 的 `key` 存储，而 `value` 则是一个固定的虚拟对象。
    
- **核心特性**：
    
    - **元素唯一**：不允许存储重复元素。
        
    - **无序**：元素的存储和取出顺序不一定相同。
        
    - **依赖 `hashCode()` 和 `equals()`**：
        
        1. 当你 `add(element)` 时，`HashSet` 首先计算 `element` 的 `hashCode()`，确定其在内部数组中的存储位置。
            
        2. 如果该位置没有其他元素，直接存入。
            
        3. 如果该位置已有其他元素（哈希冲突），则调用 `element` 的 `equals()` 方法，与该位置链表上的所有元素进行比较。
            
        4. 如果 `equals()` 返回 `true`，说明元素已存在，添加失败；如果都返回 `false`，则将新元素添加到链表末尾。
            
    - **结论**：如果你要把自定义对象存入 `HashSet`，**必须正确重写该对象的 `hashCode()` 和 `equals()` 方法**。
        

**代码示例：**

Java

```Java
import java.util.HashSet;
import java.util.Set;

public class HashSetDemo {
    public static void main(String[] args) {
        Set<String> set = new HashSet<>();
        set.add("Apple");
        set.add("Banana");
        set.add("Apple"); // 尝试添加重复元素
        set.add("Orange");

        System.out.println("Set size: " + set.size()); // 输出 3，因为 "Apple" 是重复的
        System.out.println(set); // 输出 [Apple, Orange, Banana] (顺序不保证)

        boolean containsApple = set.contains("Apple"); // true
        set.remove("Banana");
    }
}
```

---

### **`Queue` 接口详解**

- **核心特性**：遵循**先进先出 (FIFO - First-In, First-Out)** 原则，就像排队一样，先进入队列的元素会先被取出。
    
- **常用方法**：
    
    - `offer(e)`：将元素添加到队尾。如果队列已满，返回 `false`。
        
    - `poll()`：从队头移除并返回元素。如果队列为空，返回 `null`。
        
    - `peek()`：查看队头元素但不移除。如果队列为空，返回 `null`。
        

**代码示例：**

Java

```Java
import java.util.LinkedList;
import java.util.Queue;

public class QueueDemo {
    public static void main(String[] args) {
        // LinkedList 实现了 Queue 接口，可以作为队列使用
        Queue<String> queue = new LinkedList<>();

        // 入队
        queue.offer("A");
        queue.offer("B");
        queue.offer("C");

        // 查看队头元素
        System.out.println("队头元素是: " + queue.peek()); // A

        // 出队
        System.out.println("出队: " + queue.poll()); // A
        System.out.println("出队: " + queue.poll()); // B

        System.out.println("当前队列: " + queue); // [C]
    }
}
```

- **`ArrayBlockingQueue`**：你提到的这个是一个**阻塞队列**，通常用于多线程环境。它是一个有界（固定容量）的队列，当生产者试图向满队列添加元素时会被阻塞，当消费者试图从空队列获取元素时也会被阻塞。这是线程章节的重要内容。
    

---

### **`Map` 接口详解**

#### **`HashMap`**

- **底层原理**：基于**哈希表（数组 + 链表/红黑树）**实现。
    
- **核心特性**：
    
    - 以**键值对 (Key-Value)** 形式存储数据。
        
    - **Key 必须唯一**，`value` 可以重复。
        
    - **无序**。
        
    - 允许 `key` 和 `value` 为 `null`（但 `key` 只能有一个 `null`）。
        
    - **非线程安全**。
        
    - 性能极高，`put` 和 `get` 操作的平均时间复杂度为 O(1)。
        
- **工作原理**：与 `HashSet` 类似，通过 `key` 的 `hashCode()` 和 `equals()` 方法来确定存储位置和唯一性。
    

**代码示例：**

Java

```Java
import java.util.HashMap;
import java.util.Map;

public class HashMapDemo {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();

        // 增/改
        map.put("Apple", 10);
        map.put("Banana", 20);
        Integer oldValue = map.put("Apple", 15); // key 已存在，会覆盖旧值并返回旧值
        System.out.println("被覆盖的旧值: " + oldValue); // 10

        // 查
        Integer price = map.get("Banana"); // 20
        boolean hasApple = map.containsKey("Apple"); // true

        // 删
        map.remove("Banana");

        System.out.println(map); // {Apple=15}
    }
}
```

#### **`Hashtable` vs `HashMap` (纠正与完善)**

`Hashtable` 是一个非常古老的类，基本已被弃用。在现代 Java 开发中，应优先使用 `HashMap` 或 `ConcurrentHashMap`。

|特性|`HashMap` (推荐使用)|`Hashtable` (已过时)|
|---|---|---|
|**线程安全**|**非线程安全**，性能高。|**线程安全**，方法都用 `synchronized` 修饰，性能低。|
|**对 `null` 的支持**|**允许**一个 `null` key 和多个 `null` value。|**不允许** `null` key 和 `null` value，会抛出 `NullPointerException`。|
|**继承体系**|继承 `AbstractMap` 类。|继承 `Dictionary` 类 (一个已过时的类)。|
|**性能**|**高**，因为没有同步开销。|**低**，因为所有公共方法都是同步的。|
|**替代方案**|需要线程安全时，使用 `ConcurrentHashMap`。|-|

导出到 Google 表格

**结论**：忘记 `Hashtable`。单线程用 `HashMap`，多线程用 `ConcurrentHashMap`。

---

### **集合的遍历与排序**

#### **遍历集合 (迭代器 `Iterator`)**

你的笔记对迭代器的理解非常深刻和正确！当在遍历一个集合的同时需要修改（删除）它时，必须使用迭代器。使用 `for-each` 循环会导致 `ConcurrentModificationException`。

**三种主要遍历方式：**

1. **`for-each` 循环 (增强 for 循环)**：语法最简洁，适用于**只读**遍历。
    
2. **迭代器 (`Iterator`)**：最通用的遍历方式，是唯一能在遍历时**安全删除**元素的方式。
    
3. **普通 `for` 循环 (仅限 `List`)**：可以通过索引遍历，但灵活性不如迭代器。
    

**代码示例（完善你的示例）：**

Java

```
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class IteratorDemo {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("A");
        list.add("B");
        list.add("C");

        // 错误的做法：在 for-each 循环中删除元素
        try {
            for (String item : list) {
                if ("B".equals(item)) {
                    list.remove(item); // 会抛出 ConcurrentModificationException
                }
            }
        } catch (Exception e) {
            System.out.println("果然出错了: " + e);
        }

        // 正确的做法：使用迭代器
        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()) {
            String item = iterator.next();
            if ("B".equals(item)) {
                iterator.remove(); // 使用迭代器的 remove 方法
            }
        }
        System.out.println("使用迭代器删除后: " + list); // [A, C]
    }
}
```

#### **排序集合 (`Comparable` 和 `Comparator`)**

Java 提供了两种机制来对集合（或数组）中的对象进行排序。

1. **`Comparable<T>` 接口 (内部比较器/自然排序)**
    
    - 让一个**类自己实现**此接口，表明该类的对象是“可比较的”。
        
    - 需要实现 `compareTo(T other)` 方法，定义对象的“自然顺序”，例如学生按学号排序，字符串按字典序排序。
        
    - **优点**：简单直观。 **缺点**：一个类只能有一种自然排序，耦合性高。
        
2. **`Comparator<T>` 接口 (外部比较器/定制排序)**
    
    - 创建一个**单独的比较器类**来实现此接口。
        
    - 需要实现 `compare(T o1, T o2)` 方法。
        
    - **优点**：非常灵活，可以为同一个类创建多种不同的排序规则（如学生按年龄排序、按姓名排序），且不需修改学生类本身。
        
    - `list.sort()` 或 `Collections.sort(list, comparator)` 时传入。
        

**`compare` 方法的返回值约定（非常重要）：**

- `return o1 - o2;` （对于数字）
    
- **返回负数**：表示 `o1`应该排在`o2`**前面**。
    
- **返回零**：表示 `o1`和`o2`相等。
    
- **返回正数**：表示 `o1`应该排在`o2`**后面**。
    

**结论**：`o1 - o2` 是**升序**，`o2 - o1` 是**降序**。你的笔记中“正的就是升序 负的就是降序”这个说法不准确，容易记混。

---

### **【补充的标题】`Collections` 工具类**

与操作数组的 `Arrays` 工具类类似，Java 提供了 `java.util.Collections` 工具类，包含大量用于操作集合的静态方法。

Java

```Java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class CollectionsUtilDemo {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        list.add(30);
        list.add(10);
        list.add(50);
        list.add(20);

        // 排序
        Collections.sort(list);
        System.out.println("排序后: " + list); // [10, 20, 30, 50]

        // 反转
        Collections.reverse(list);
        System.out.println("反转后: " + list); // [50, 30, 20, 10]

        // 随机打乱
        Collections.shuffle(list);
        System.out.println("打乱后: " + list);

        // 查找最大/最小值
        Integer max = Collections.max(list);
        Integer min = Collections.min(list);
        System.out.println("最大值: " + max + ", 最小值: " + min);
    }
}
```

---

### **章节总结 (问答式)**

**Q1：`ArrayList` 和 `LinkedList` 的主要区别是什么？我应该如何选择？** **A1**：主要区别在于底层数据结构。`ArrayList` 是动态数组，查询快，中间增删慢；`LinkedList` 是双向链表，查询慢，头尾增删极快。在绝大多数情况下，`ArrayList` 的综合性能更好，是首选。只有在确定有大量头尾增删操作（如实现队列）时，才考虑 `LinkedList`。

**Q2：`List`, `Set`, `Map` 有什么区别？** **A2**：

- `List`：有序，可重复。
    
- `Set`：无序（`HashSet`），不可重复。
    
- `Map`：存储键值对，Key 不可重复。
    

**Q3：为什么将自定义对象放入 `HashSet` 或作为 `HashMap` 的 Key 时，必须重写 `hashCode()` 和 `equals()` 方法？** **A3**：因为 `HashSet` 和 `HashMap` 都依赖这两个方法来确定元素的存储位置和判断元素是否重复。如果不重写，会使用 `Object` 类的默认实现（基于内存地址），导致即使内容相同的两个对象也被视为不同，从而无法实现预期的“去重”或“查找”功能。

**Q4：`HashMap` 和 `Hashtable` 最重要的区别是什么？** **A4**：最重要的区别是**线程安全性**。`Hashtable` 是线程安全的，但性能低下，已过时。`HashMap` 不是线程安全的，但性能高。在多线程环境中，应该使用 `ConcurrentHashMap`。

**Q5：为什么在遍历集合时删除元素要使用 `Iterator`？** **A5**：因为 `Iterator` 提供了安全的操作机制。如果在 `for-each` 循环中直接调用集合的 `remove()` 方法，会破坏循环内部维护的集合状态，导致抛出 `ConcurrentModificationException` 异常。

---

### **章节小练习**

**练习1：词频统计**

1. 给定一个字符串，例如 `"I love Java, I love coding, Java is the best language."`。
    
2. 将字符串按空格和逗号分割成单词数组。
    
3. 使用 `HashMap<String, Integer>` 来统计每个单词出现的次数。
    
4. 遍历 `HashMap` 并打印出每个单词及其出现的频率。
    

**练习2：学生信息排序**

1. 创建一个 `Student` 类，包含 `id` (int), `name` (String), 和 `score` (double) 三个属性。
    
2. 让 `Student` 类实现 `Comparable<Student>` 接口，实现 `compareTo` 方法，使其默认按 `id` **升序**排序。
    
3. 创建一个 `ArrayList<Student>`，并添加几个学生对象。
    
4. 使用 `Collections.sort(list)` 对列表进行排序并打印，验证其按 `id` 排序。
    
5. 创建一个实现了 `Comparator<Student>` 接口的外部比较器 `ScoreComparator`，使其能按学生**分数降序**排序。
    
6. 使用 `list.sort(new ScoreComparator())` 对列表进行二次排序并打印，验证其按分数排序。
   
   
## **第五章：IO 数据流操作**

### **1. 什么是 I/O 流？**

I/O 指的是**输入 (Input)** 和**输出 (Output)**。在 Java 中，程序与外部世界（如文件、网络、键盘、显示器）之间的数据交互，都是通过“流 (Stream)”来完成的。

你可以将“流”想象成一根连接程序和数据源/目的地的**管道**，数据就像水流一样在这根管道中单向流动。

- **输入流**：从外部（如文件）**读取**数据到程序（内存）中。
    
- **输出流**：将程序（内存）中的数据**写入**到外部（如文件）中。
    

### **【补充的标题】I/O 流的分类**

理解 Java I/O 的分类是掌握其体系的关键。我们可以从两个维度来划分：

**维度一：按处理的数据类型划分**

1. **字节流 (Byte Stream)**：
    
    - **特点**：以**字节 (byte)** 为单位处理数据，可以处理**任何类型**的文件（文本、图片、音视频等）。
        
    - **顶级父类**：`InputStream` (输入) 和 `OutputStream` (输出)。
        
2. **字符流 (Character Stream)**：
    
    - **特点**：以**字符 (character)** 为单位处理数据，专门用于处理**纯文本文件 (`.txt`, `.java`, `.html` 等)**。它内部会自动处理字节到字符的编码和解码。
        
    - **顶级父类**：`Reader` (输入) 和 `Writer` (输出)。
        
    - **优势**：处理文本时能有效避免乱码问题，并且通常效率更高，支持按行读写。
        

**维度二：按功能划分**

1. **节点流 (Node Stream)**：
    
    - **特点**：直接与数据源/目的地（如文件、数组）相连接的“基础管道”。例如 `FileInputStream`, `FileReader`。
        
2. **处理流 (Processing Stream) / 包装流 (Wrapper Stream)**：
    
    - **特点**：不直接连接数据源，而是“套”在已有的节点流之上，为其增加额外的功能（如缓冲、格式化、对象序列化）。例如 `BufferedInputStream`, `PrintWriter`, `ObjectOutputStream`。
        
    - 这是一种**装饰者设计模式**的应用，可以灵活地组合流的功能。
        

---

### **2. 文件操作：`java.io.File` 类**

在你进行任何文件读写之前，首先需要通过 `File` 类来操作文件系统中的文件或目录。

**核心要点**：`File` 类本身**不能读写文件内容**。它只是文件或目录**路径的抽象表示**，提供了创建、删除、重命名、获取文件元数据（如大小、修改时间）等功能。

**代码示例（你的示例很完整，这里稍作整理和注释）：**

Java

```Java
import java.io.File;
import java.io.IOException;
import java.util.Date;

public class FileDemo {
    public static void main(String[] args) throws IOException {
        // 1. 创建文件对象
        // 使用 File.separator 替代硬编码的 "\" 或 "/"，可以跨平台
        String filePath = "D:" + File.separator + "demo" + File.separator + "test.txt";
        File file = new File(filePath);

        // 2. 获取文件信息
        System.out.println("文件是否存在: " + file.exists());
        if (file.exists()) {
            System.out.println("是否是文件: " + file.isFile());
            System.out.println("是否是目录: " + file.isDirectory());
            System.out.println("文件名: " + file.getName());
            System.out.println("文件路径: " + file.getPath());
            System.out.println("绝对路径: " + file.getAbsolutePath());
            System.out.println("父目录: " + file.getParent());
            System.out.println("文件大小(字节): " + file.length());
            System.out.println("最后修改时间: " + new Date(file.lastModified()));
        }

        // 3. 目录操作
        File dir = new File("D:" + File.separator + "demo");
        if (dir.isDirectory()) {
            System.out.println("\n--- 遍历目录 ---");
            String[] names = dir.list(); // 获取目录下所有文件名和目录名
            for (String name : names) {
                System.out.println(name);
            }
        }

        // 4. 创建和删除操作
        if (!file.exists()) {
            // 先创建父目录，如果不存在的话
            File parentDir = file.getParentFile();
            if (!parentDir.exists()) {
                parentDir.mkdirs(); // mkdirs() 可以创建多级目录
                System.out.println("创建目录: " + parentDir.getAbsolutePath());
            }
            // 然后创建文件
            file.createNewFile();
            System.out.println("文件已创建。");
        } else {
            // file.delete(); // 删除文件或空目录
            // System.out.println("文件已删除。");
        }
    }
}
```

---

### **3. 字节流：文件复制**

你的笔记中展示了两种文件复制的方式，这正好体现了从基础流到缓冲流的性能优化过程。

**方式一：基础字节流 (效率低)** `FileInputStream` 和 `FileOutputStream` 一次只读写一个字节，每次读写都会触发一次硬盘 I/O，当文件很大时，这种频繁的 I/O 操作会导致性能极差。

**方式二：缓冲字节流 (效率高，推荐)** `BufferedInputStream` 和 `BufferedOutputStream` 是处理流，它们内部维护了一个**缓冲区（默认 8KB 的字节数组）**。

- **读取时**：`BufferedInputStream` 会一次性从文件中读取一大块数据到缓冲区，后续的 `read()` 操作直接从内存中的缓冲区获取数据，只有当缓冲区数据用完时，才会再次访问硬盘。
    
- **写入时**：`write()` 操作会先将数据写入缓冲区，直到缓冲区满了，`BufferedOutputStream` 才会将整个缓冲区的数据一次性写入硬盘。
    

这种机制**大大减少了实际的硬盘 I/O 次数**，从而极大地提升了读写性能。

### **【补充的标题】`try-with-resources` 语法 (现代写法)**

你的代码中使用了经典的 `try-catch-finally` 结构来关闭流，这是正确的，但比较繁琐。从 JDK 7 开始，Java 提供了 `try-with-resources` 语句，可以**自动关闭**实现了 `AutoCloseable` 接口的资源（所有 I/O 流都实现了该接口）。这是**目前推荐的最佳实践**。

**使用 `try-with-resources` 改进你的缓冲流复制示例：**

Java

```Java
import java.io.*;

public class FileCopyDemo {
    public static void main(String[] args) {
        String sourcePath = "D:\\demo\\source.zip";
        String destPath = "D:\\demo\\dest.zip";

        long startTime = System.currentTimeMillis();

        // 将需要自动关闭的资源声明在 try() 的括号内
        try (
            InputStream in = new BufferedInputStream(new FileInputStream(sourcePath));
            OutputStream out = new BufferedOutputStream(new FileOutputStream(destPath))
        ) {
            byte[] buffer = new byte[1024 * 8]; // 8KB的缓冲区
            int bytesRead; // 记录每次实际读取的字节数

            // in.read(buffer) 一次性读取最多 buffer.length 个字节到 buffer 数组中
            // 返回实际读取的字节数，如果到达文件末尾，返回 -1
            while ((bytesRead = in.read(buffer)) != -1) {
                // out.write(buffer, 0, bytesRead) 将 buffer 中从0开始的 bytesRead 个字节写入
                // 必须指定长度，否则最后一次可能写入多余数据
                out.write(buffer, 0, bytesRead);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        long endTime = System.currentTimeMillis();
        System.out.println("文件复制完成，耗时: " + (endTime - startTime) + "ms");
    }
}
```

**优点**：代码更简洁，并且绝对不会忘记关闭流，避免了资源泄漏的风险。`finally` 块不再是必需的。

---

### **4. 字符流：读写文本文件**

当你确定要处理的是纯文本文件时，应该优先使用字符流。

- `FileReader` / `FileWriter`：基础的字符文件流。
    
- `BufferedReader` / `BufferedWriter`：带缓冲的字符流，提供更高的性能。
    
- `BufferedReader` 的一个**核心优势**是提供了 `readLine()` 方法，可以方便地一次读取一行文本。
    
- `PrintWriter`：一个非常方便的输出流，它提供了 `println()` 方法，可以写入一行并自动添加换行符。
    

**使用 `try-with-resources` 改进你的字符流复制示例：**

Java

```Java
import java.io.*;

public class TextFileCopyDemo {
    public static void main(String[] args) {
        String sourcePath = "D:\\demo\\source.txt";
        String destPath = "D:\\demo\\dest.txt";

        try (
            BufferedReader reader = new BufferedReader(new FileReader(sourcePath));
            PrintWriter writer = new PrintWriter(new FileWriter(destPath))
        ) {
            String line;
            while ((line = reader.readLine()) != null) {
                writer.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        System.out.println("文本文件复制完成。");
    }
}
```

- **注意**：`PrintWriter` 和 `BufferedWriter` 内部也有缓冲区，当缓冲区满或流关闭时才会写入。如果想立即写入，可以手动调用 `writer.flush()`。
    

---

### **5. 对象流：序列化与反序列化**

**序列化 (Serialization)**：将 Java **对象**的状态转换为**字节序列**的过程。这个字节序列可以被保存到文件、数据库中，或通过网络传输。 **反序列化 (Deserialization)**：将字节序列恢复为 Java 对象的过程。

**核心用途**：实现对象的**持久化**和**网络传输**。

**如何实现？**

1. 让需要被序列化的类实现 `java.io.Serializable` **标记接口**。这个接口没有任何方法，它只是一个“许可证”，告诉 JVM 这个类的对象可以被序列化。
    
2. 使用 `ObjectOutputStream` 来执行序列化操作 (`writeObject()`)。
    
3. 使用 `ObjectInputStream` 来执行反序列化操作 (`readObject()`)。
    

**【补充的标题】`transient` 关键字和 `serialVersionUID`**

- **`transient`**：如果类中的某个字段（如密码、临时计算结果）不希望被序列化，可以用 `transient` 关键字修饰，该字段在序列化时会被忽略。
    
- **`serialVersionUID`**：它是一个版本号。在反序列化时，JVM 会检查文件中的 `serialVersionUID` 是否与当前类中的 `serialVersionUID` 一致。如果不一致，会抛出 `InvalidClassException`，防止因类版本不兼容导致的问题。强烈建议所有可序列化的类都显式声明一个 `private static final long serialVersionUID`。
    

**完整代码示例（序列化 + 反序列化）：**

Java

```Java
import java.io.*;

// 1. 实现 Serializable 接口
class User implements Serializable {
    // 2. 强烈建议添加 serialVersionUID
    private static final long serialVersionUID = 1L;

    String name;
    int age;
    transient String password; // 此字段不会被序列化

    public User(String name, int age, String password) {
        this.name = name;
        this.age = age;
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" + "name='" + name + '\'' + ", age=" + age + ", password='" + password + '\'' + '}';
    }
}

public class SerializationDemo {
    public static void main(String[] args) {
        File file = new File("D:\\demo\\user.ser");
        User user = new User("张三", 30, "123456");

        // --- 序列化 ---
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(file))) {
            oos.writeObject(user);
            System.out.println("对象序列化成功: " + user);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // --- 反序列化 ---
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(file))) {
            User deserializedUser = (User) ois.readObject(); // readObject() 返回 Object，需要强转
            System.out.println("对象反序列化成功: " + deserializedUser);
            System.out.println("反序列化后密码字段为: " + deserializedUser.password); // 输出 null
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

---

### **章节总结 (问答式)**

**Q1：字节流和字符流的根本区别是什么？应该如何选择？** **A1**：根本区别在于处理数据的基本单位。字节流以**字节**为单位，能处理任何类型的数据。字符流以**字符**为单位，专门用于处理纯文本，并能自动处理字符编码。**选择原则**：处理纯文本用字符流，处理其他一切（图片、视频、二进制文件等）用字节流。

**Q2：`File` 类是用来读写文件内容的吗？** **A2**：不是。`File` 类只是文件系统路径的一个抽象表示，用于对文件或目录本身进行操作（如创建、删除、重命名、获取属性），而不是读写其内容。内容的读写必须通过 I/O 流来完成。

**Q3：为什么缓冲流（如 `BufferedInputStream`）比基本的文件流（如 `FileInputStream`）快得多？** **A3**：因为缓冲流内部有一个缓冲区（内存中的数组）。它通过一次性从硬盘读取/写入一大块数据到缓冲区，来**显著减少物理硬盘的 I/O 次数**，从而大幅提升性能。

**Q4：什么是 `try-with-resources`？它有什么好处？** **A4**：`try-with-resources` 是 Java 7 引入的语法，用于自动管理资源。只需将需要关闭的资源在 `try()` 的括号中声明，Java 就会在 `try` 块结束时（无论正常结束还是异常结束）自动调用资源的 `close()` 方法。**好处**是代码更简洁、更安全，能有效避免资源泄漏。

**Q5：什么是对象序列化？一个类要能被序列化需要满足什么条件？** **A5**：序列化是将 Java 对象转换为字节序列的过程，以便存储或传输。一个类要能被序列化，**必须实现 `java.io.Serializable` 这个标记接口**。

---

### **章节小练习**

**练习1：统计文本文件单词**

1. 在 D 盘 demo 目录下创建一个名为 `article.txt` 的文本文件，并写入一段英文文章。
    
2. 编写一个 Java 程序，使用 `BufferedReader` 按行读取该文件。
    
3. 统计文件中包含的总行数、总单词数（以空格为分隔符）和总字符数。
    
4. 将统计结果输出到控制台。
    

**练习2：简单的配置保存与加载**

1. 创建一个 `GameSettings` 类，包含 `playerName` (String), `volume` (int), `difficulty` (String) 三个属性。
    
2. 让 `GameSettings` 类实现 `Serializable` 接口。
    
3. 编写两个方法：
    
    - `void saveSettings(GameSettings settings, String filePath)`：将传入的 `settings` 对象序列化到指定文件。
        
    - `GameSettings loadSettings(String filePath)`：从指定文件反序列化，并返回一个 `GameSettings` 对象。
        
4. 在 `main` 方法中，创建一个 `GameSettings` 实例，设置一些值，调用 `saveSettings` 保存它；然后调用 `loadSettings` 将其加载到一个新的对象中，并打印新对象的内容以验证是否成功。
   
   

## **第六章：线程 (Thread)**

### **1. 核心概念：进程与线程**

- **进程 (Process)**：一个正在运行的**程序**就是一个进程。例如，你打开的浏览器、音乐播放器都是独立的进程。操作系统会为每个进程分配独立的内存空间，进程之间的数据是隔离的。
    
- **线程 (Thread)**：线程是进程内部的一条**执行路径**或**执行单元**。一个进程至少包含一个线程，即**主线程**。我们平时写的 `main` 方法就运行在主线程中。一个进程也可以包含多个线程，这些线程**共享该进程的内存空间**（如堆内存），但每个线程有自己独立的程序计数器和栈内存。
    

**通俗比喻**：一个工厂是一个**进程**，工厂里的每一条流水线就是一个**线程**。所有流水线共享工厂的资源（原料、电力），但每条流水线都在独立地执行自己的任务。

**多线程的优势**：

1. **提高资源利用率**：当一个线程因为等待 I/O（如读写文件、网络请求）而阻塞时，CPU 可以切换到其他线程去执行任务，避免空闲。
    
2. **提升用户体验**：在图形界面应用中，可以将耗时的操作（如下载）放在一个子线程中执行，而主线程（UI 线程）继续响应用户的点击、拖动等操作，避免界面“卡死”。
    
3. **发挥多核 CPU 性能**：在多核 CPU 上，多个线程可以真正地**并行 (Parallel)** 执行，从而加快计算密集型任务的处理速度。
    

**并发 (Concurrency) vs. 并行 (Parallelism)**

- **并发**：指在**一个时间段内**，多个任务都在向前推进。在单核 CPU 上，通过快速地在不同线程间切换，宏观上看起来像同时执行。
    
- **并行**：指在**同一时刻**，多个任务在多个 CPU 核心上**同时**执行。并行是并发的子集，是真正意义上的“同时工作”。
    

---

### **2. 线程的生命周期**

一个线程从创建到消亡会经历不同的状态，理解这些状态有助于我们调试和分析多线程程序。

1. **新建 (NEW)**：当一个 `Thread` 对象被创建后，它就处于新建状态。
    
2. **就绪 (RUNNABLE)**：调用线程的 `start()` 方法后，线程进入就绪状态。它已经准备好运行，正在等待 CPU 调度。
    
3. **阻塞 (BLOCKED)**：线程试图进入一个 `synchronized` 同步代码块，但该代码块的锁被其他线程持有时，该线程会进入阻塞状态。
    
4. **等待 (WAITING)**：线程调用了 `Object.wait()`、`Thread.join()` 或 `LockSupport.park()` 等方法后，会进入无限期等待状态，直到被其他线程唤醒（如通过 `notify()` 或 `unpark()`）。
    
5. **计时等待 (TIMED_WAITING)**：与 `WAITING` 类似，但有时间限制。调用 `Thread.sleep(ms)`、`Object.wait(ms)`、`Thread.join(ms)` 等方法后进入此状态。时间结束后会自动返回 `RUNNABLE` 状态。
    
6. **终止 (TERMINATED)**：线程的 `run()` 方法执行完毕或因异常退出后，线程进入终止状态，生命周期结束。
    

---

### **3. 创建和启动线程**

Java 中创建线程主要有两种方式，你的笔记中提到了三种具体写法，都非常正确。

**方式一：继承 `Thread` 类**

- **步骤**：创建一个类继承 `Thread`，并重写 `run()` 方法。
    
- **缺点**：Java 是单继承的，如果你的类已经继承了其他类，就不能再继承 `Thread`。
    

Java

```
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("通过继承 Thread 创建线程: " + Thread.currentThread().getName());
    }
}
```

**方式二：实现 `Runnable` 接口 (推荐)**

- **步骤**：创建一个类实现 `Runnable` 接口，并实现 `run()` 方法。然后将这个 `Runnable` 对象作为参数传递给 `Thread` 的构造函数。
    
- **优点**：
    
    1. **解耦**：将“任务”（`Runnable` 的 `run` 方法）与“执行者”（`Thread` 对象）分离。
        
    2. **灵活性**：你的任务类可以继承其他任何类。
        
    3. **资源共享**：多个线程可以共享同一个 `Runnable` 实例，方便实现资源共享。
        

Java

```
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("通过实现 Runnable 创建线程: " + Thread.currentThread().getName());
    }
}
```

**启动线程的代码示例：**

Java

```
public class CreateThreadDemo {
    public static void main(String[] args) {
        // 方式一启动
        MyThread t1 = new MyThread();
        t1.start(); // 注意：是调用 start()，而不是 run()!

        // 方式二启动
        MyRunnable myRunnable = new MyRunnable();
        Thread t2 = new Thread(myRunnable);
        t2.start();

        // 方式二的匿名内部类写法
        Thread t3 = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("通过 Runnable 匿名类创建: " + Thread.currentThread().getName());
            }
        });
        t3.start();

        // 方式二的 Lambda 表达式写法 (最简洁)
        Thread t4 = new Thread(() -> {
            System.out.println("通过 Lambda 表达式创建: " + Thread.currentThread().getName());
        });
        t4.start();
    }
}
```

**`start()` vs `run()`**

- `thread.start()`：启动一个**新线程**，并让这个新线程去执行 `run()` 方法。这是一个异步调用。
    
- `thread.run()`：**不启动新线程**，只是在当前线程中像调用普通方法一样同步地执行 `run()` 方法。
    

### **4. 常用线程方法**

- `static void sleep(long millis)`：让**当前**线程休眠指定的毫秒数，进入 `TIMED_WAITING` 状态。
    
- `void join()`：让**当前**线程等待，直到**调用 `join()` 的那个线程**执行完毕。你的串行执行示例就是对 `join()` 的完美应用。
    

---

### **5. 【补充】线程同步 (Thread Synchronization)**

这是多线程编程中最核心、最关键的部分。当多个线程共享并修改同一个数据时，如果没有适当的同步措施，就会导致**数据不一致**和**线程安全问题**，这种情况被称为**竞态条件 (Race Condition)**。

**问题演示：**

Java

```
class Counter {
    public int count = 0;
    public void increment() {
        count++;
    }
}

public class RaceConditionDemo {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();
        Runnable task = () -> {
            for (int i = 0; i < 10000; i++) {
                counter.increment();
            }
        };

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        t1.start();
        t2.start();

        t1.join(); // 等待 t1 执行完
        t2.join(); // 等待 t2 执行完

        // 理论上结果应该是 20000，但实际运行多次，结果通常都小于 20000
        System.out.println("Final count: " + counter.count); 
    }
}
```

**原因**：`count++` 操作并非原子性的，它包含“读取-修改-写入”三个步骤。多个线程可能同时读取到旧值，然后各自加一再写回，导致部分增加操作丢失。

**解决方案一：`synchronized` 关键字** `synchronized` 可以保证在同一时刻，只有一个线程可以进入被它修饰的代码块或方法，从而保证了操作的原子性。它通过获取对象的“内置锁”（也叫监视器锁）来实现。

- **同步方法**：
    
    Java
    
    ```
    // 锁对象是 this
    public synchronized void increment() {
        count++;
    }
    ```
    
- **同步代码块**（更灵活）：
    
    Java
    
    ```
    private final Object lock = new Object();
    public void increment() {
        // 只锁住需要同步的关键代码，性能更好
        synchronized (lock) { 
            count++;
        }
    }
    ```
    

将上面 `Counter` 类的 `increment` 方法用 `synchronized` 修饰后，最终结果将永远是 `20000`。

**解决方案二：`java.util.concurrent.locks.Lock`** `Lock` 接口提供了比 `synchronized` 更灵活的锁机制。

Java

```
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class SafeCounter {
    public int count = 0;
    private final Lock lock = new ReentrantLock();

    public void increment() {
        lock.lock(); // 获取锁
        try {
            count++;
        } finally {
            lock.unlock(); // 必须在 finally 块中释放锁
        }
    }
}
```

---

### **6. 线程池 (Thread Pool)**

频繁地创建和销毁线程会带来巨大的性能开销。线程池通过维护一组可复用的工作线程，来解决这个问题。

- **工作流程**：将任务（`Runnable` 或 `Callable`）提交给线程池，线程池会从池中选择一个空闲线程来执行任务。任务执行完毕后，线程不会销毁，而是返回池中等待下一个任务。
    
- **好处**：
    
    1. **降低资源消耗**：复用线程，避免了创建和销毁的开销。
        
    2. **提高响应速度**：任务来了可以直接执行，无需等待线程创建。
        
    3. **提高可管理性**：可以统一管理、监控和调优线程。
        

**`Executors` 工厂类（你的笔记示例很正确）：** `java.util.concurrent.Executors` 提供了一些便捷的静态工厂方法来创建线程池。

- `newFixedThreadPool(int n)`：创建一个**固定大小**的线程池。
    
- `newCachedThreadPool()`：创建一个**可缓存**的线程池。线程数会根据任务量动态调整，适合执行大量短期异步任务。
    
- `newSingleThreadExecutor()`：创建一个**单线程**的线程池，保证所有任务按顺序执行。
    
- `newScheduledThreadPool(int corePoolSize)`：创建一个支持**定时及周期性任务**执行的线程池。
    

**注意**：在大型生产环境中，通常不推荐直接使用 `Executors` 工厂方法，因为它们可能导致资源耗尽（如 `newCachedThreadPool` 的线程数无上限）。更推荐的做法是直接使用 `ThreadPoolExecutor` 的构造函数，手动指定所有参数，以获得更精细的控制。

---

---

## **第七章：反射 (Reflection)**

### **1. 什么是反射？**

反射是 Java 提供的一种**在运行时动态地分析和操作类**的能力。正常的 Java 代码在编译时就已经确定了要操作的类和方法，而反射机制允许程序在运行期间才去获取任意一个类的信息（如属性、方法、构造器），并能动态地创建对象、调用方法、修改属性，**即使这些成员是 `private` 的**。

### **2. 反射的基石：`Class` 对象**

要使用反射，首先必须获取目标类的**字节码对象**，即 `java.lang.Class` 类的实例。每个加载到 JVM 中的类，都有且只有一个与之对应的 `Class` 对象。

**获取 `Class` 对象的三种方式：**

Java

```
class User {}

public class GetClassObject {
    public static void main(String[] args) throws ClassNotFoundException {
        // 方式一：通过类名.class 获取 (最安全、性能最好)
        Class<User> clazz1 = User.class;

        // 方式二：通过对象.getClass() 获取
        User user = new User();
        Class<? extends User> clazz2 = user.getClass();

        // 方式三：通过 Class.forName("全限定类名") 获取 (最灵活，常用于框架)
        Class<?> clazz3 = Class.forName("com.example.User"); // 假设 User 在 com.example 包下

        System.out.println(clazz1 == clazz2 && clazz2 == clazz3); // true
    }
}
```

### **3. 反射的核心 API**

获取 `Class` 对象后，你就可以为所欲为了。

**核心步骤：**

1. **获取构造器 (`Constructor`) 并创建实例**
    
2. **获取方法 (`Method`) 并调用**
    
3. **获取字段 (`Field`) 并读写**
    

**注意**：

- `get...()` 系列方法（如 `getMethods()`, `getField()`）只能获取**公共的 (`public`)** 成员。
    
- `getDeclared...()` 系列方法（如 `getDeclaredMethods()`, `getDeclaredField()`）可以获取**所有声明的**成员，包括 `private`, `protected`, `default`。
    
- 当操作非 `public` 成员时，必须先调用 `member.setAccessible(true)` 来**解除访问限制**。
    

**综合示例：**

Java

```
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

class Student {
    private String name;
    public int age;

    public Student() {
        this.name = "Unknown";
        this.age = 0;
    }
    private Student(String name) {
        this.name = name;
        this.age = 18;
    }
    public void study(String subject) {
        System.out.println(name + " is studying " + subject);
    }
    private String getSecret() {
        return "This is a secret of " + name;
    }
}

public class ReflectionDemo {
    public static void main(String[] args) throws Exception {
        // 1. 获取 Class 对象
        Class<Student> studentClass = Student.class;

        // 2. 获取构造器并创建实例
        // 获取 public 无参构造器
        Constructor<Student> constructor = studentClass.getConstructor();
        Student student1 = constructor.newInstance();
        student1.study("Math");

        // 获取 private 有参构造器
        Constructor<Student> privateConstructor = studentClass.getDeclaredConstructor(String.class);
        privateConstructor.setAccessible(true); // 解除私有权限
        Student student2 = privateConstructor.newInstance("张三");

        // 3. 获取方法并调用
        // 获取 public 方法
        Method studyMethod = studentClass.getMethod("study", String.class);
        studyMethod.invoke(student2, "Java"); // 调用 student2 对象的 study 方法

        // 获取 private 方法
        Method getSecretMethod = studentClass.getDeclaredMethod("getSecret");
        getSecretMethod.setAccessible(true);
        String secret = (String) getSecretMethod.invoke(student2);
        System.out.println("Invoked private method, result: " + secret);

        // 4. 获取字段并读写
        // 获取 public 字段
        Field ageField = studentClass.getField("age");
        ageField.set(student2, 20); // 修改 student2 对象的 age 字段
        System.out.println("Modified public field, age: " + student2.age);

        // 获取 private 字段
        Field nameField = studentClass.getDeclaredField("name");
        nameField.setAccessible(true);
        nameField.set(student2, "李四");
        System.out.println("Modified private field, name: " + nameField.get(student2));
    }
}
```

### **4. 反射的优缺点与应用**

- **优点**：**极大的灵活性**。它使得程序可以在运行时动态加载和使用类，是许多高级框架（如 Spring、MyBatis）的实现基石。
    
- **缺点**：
    
    1. **性能开销大**：反射操作涉及大量的查找和检查，比直接调用慢得多。
        
    2. **破坏封装性**：可以无视 `private` 修饰符，访问和修改私有成员。
        
    3. **类型不安全**：编译器无法进行类型检查，错误只能在运行时暴露。
        
- **应用场景**：开发通用框架、注解处理器、动态代理、IDE 的自动提示等。**在日常的业务代码中，应尽量避免使用反射。**
    

### **5. 类加载器 (Class Loader)**

类加载器负责将 `.class` 文件从硬盘加载到 JVM 内存中，并生成对应的 `Class` 对象。Java 的类加载器采用了**双亲委派模型**：

1. **启动类加载器 (Bootstrap ClassLoader)**：最顶层，负责加载 Java 核心库（`rt.jar`）。
    
2. **平台类加载器 (Platform ClassLoader)**：负责加载 Java 平台标准版的扩展功能。
    
3. **应用程序类加载器 (App ClassLoader)**：负责加载我们自己写的、位于 `classpath` 下的类。
    

**委派流程**：当一个类加载器收到加载请求时，它会先把请求**委派给父加载器**去尝试加载，只有当父加载器找不到时，子加载器才会自己去加载。这保证了 Java 核心库的类不会被用户自定义的同名类所覆盖，确保了安全。

---

### **章节总结 (问答式)**

**Q1：实现 `Runnable` 接口和继承 `Thread` 类，哪种方式创建线程更好？为什么？** **A1**：实现 `Runnable` 接口更好。因为它将任务和执行者解耦，避免了 Java 单继承的限制，并且方便多个线程共享同一个任务实例。

**Q2：什么是线程安全问题？如何解决？** **A2**：线程安全问题是指多个线程并发访问和修改共享数据时，可能导致数据不一致或程序错误。主要解决方案是使用同步机制，如 `synchronized` 关键字或 `Lock` 接口，来保证在同一时间只有一个线程能访问关键代码段。

**Q3：为什么要使用线程池？** **A3**：为了复用线程，减少因频繁创建和销毁线程带来的性能开销，同时还能提高响应速度，并对线程进行统一管理和控制。

**Q4：什么是反射？它最大的优点和缺点是什么？** **A4**：反射是在运行时动态分析和操作类的能力。最大的优点是**灵活性**，使得编写通用框架成为可能。最大的缺点是**性能差**、**破坏封装**且**类型不安全**。

**Q5：`Class.forName("...")` 和 `MyClass.class` 获取 `Class` 对象有什么区别？** **A5**：`Class.forName` 是一个动态的、运行时的方法，它接收一个字符串参数，可以加载任意类，但可能抛出 `ClassNotFoundException`。`MyClass.class` 是一个静态的、编译时的操作，类型安全，不会抛出异常，性能也更好，但要加载的类必须在编译时就已知。

---

### **章节小练习**

**练习1：多线程银行取款**

1. 创建一个 `BankAccount` 类，包含一个 `balance` (余额) 属性和一个 `withdraw(amount)` (取款) 方法。
    
2. `withdraw` 方法需要是线程安全的。在方法内部，判断余额是否充足，如果充足则减去相应金额并打印成功信息，否则打印失败信息。
    
3. 在 `main` 方法中，创建一个初始余额为 1000 的共享 `BankAccount` 对象。
    
4. 创建两个线程，都去尝试从此账户取款 800 元。
    
5. 启动这两个线程，观察并确保最终只有一个线程能取款成功，账户余额正确地变为 200。
    

**练习2：通用属性拷贝工具**

1. 编写一个静态方法 `void copyProperties(Object source, Object destination)`。
    
2. 该方法使用**反射**，将 `source` 对象中所有**与 `destination` 对象同名且同类型**的**公共 (public)** 字段的值，拷贝到 `destination` 对象的对应字段中。
    
3. 创建两个不同的类 `Person` 和 `Student`，它们有一些同名的公共字段（如 `name`, `age`）。
    
4. 在 `main` 方法中，创建一个 `Person` 对象并赋值，再创建一个空的 `Student` 对象，调用 `copyProperties` 方法，然后打印 `Student` 对象的属性，验证拷贝是否成功。