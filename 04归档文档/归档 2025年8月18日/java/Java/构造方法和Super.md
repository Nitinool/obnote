
##### Super使用场景

如果父类存在无参构造方法（如 `public Parent()`），则子类的构造方法 ​**可以不用显式调用 `super`**。编译器会自动插入 `super()`。
```java
class Parent {
    public Parent() {} // 无参构造方法
}

class Child extends Parent {
    public Child() { 
        // 编译器自动插入 super()
    }
}
```


如果父类的所有构造方法都带有参数（如 `public Parent(String name)`），则子类 ​**必须显式调用 `super(...)`**，否则编译器会报错：
```java
class Parent {
    public Parent(String name) {} // 没有无参构造方法
}

class Child extends Parent {
    public Child() {
        super("默认名称"); // 必须显式调用父类构造方法
    }
}
```

即使父类有无参构造方法，子类也可以显式调用父类的其他构造方法：
```java
class Parent {
    public Parent() {} 
    public Parent(String name) {}
}

class Child extends Parent {
    public Child() {
        super("自定义名称"); // 显式调用父类的带参构造方法
    }
}
```

##### 构造方法

1.作用
- 构造方法是**与类同名且无返回值**的特殊方法，用于**初始化对象**的属性和状态。
- 每个类至少有一个构造方法（如果没有显式定义，编译器会自动生成无参构造方法）。

```java
public class Dog {
    public Dog() { // 无参构造方法 }
    public Dog(String name) { // 带参构造方法 }
}
```

- **初始化对象**：在对象创建时分配内存并设置初始值。

2.规则
- 不能有返回值
- 必须与类名相同

3.构造方法的重载
通过**参数列表不同**​（类型、数量、顺序）区分多个构造方法。
```java
public class Student {
    public Student() { // 无参构造 }
    public Student(String name) { // 带1个参数 }
    public Student(String name, int age) { // 带2个参数 }
}

Student s1 = new Student();       // 调用无参构造
Student s2 = new Student("Alice"); // 调用带1个参数的构造
```

4.super和this在构造方法中的使用
```java
public class Animal {
    public Animal(String name) { // 父类无无参构造
        System.out.println("Animal: " + name);
    }
}

public class Dog extends Animal {
    public Dog(String name) {
        super(name); // 调用父类构造方法
        System.out.println("Dog: " + name);
    }
}
```

示例代码
```java
public class Employee {
    private String name;
    private double salary;

    // 无参构造方法
    public Employee() {
        this("Unknown", 0.0); // 调用带参构造
    }

    // 带参构造方法
    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    // Getter 方法
    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }

    public static void main(String[] args) {
        Employee e1 = new Employee(); // 调用无参构造 → 实际调用带参构造
        Employee e2 = new Employee("Alice", 5000); // 调用带参构造

        System.out.println(e1.getName()); // 输出：Unknown
        System.out.println(e2.getSalary()); // 输出：5000.0
    }
}
```