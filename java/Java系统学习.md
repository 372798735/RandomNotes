# Java系统学习
## 第一阶段：Java核心语法与面向对象
### 定义
前端转后端的核心转变：Java是编译型、静态类型(相当于 TypeScript，在编译时就被严格检查，不过java比TS更严格： TS有类型推断和any,Java几乎必须显式声明)、基于类的纯面向对象语言，而JS是动态解释型、基于原型。
### Java基础语法
#### 基本类型和引用类型
注意：
1. 数字类型：数值小于 21 亿能用 int，优先用int（省内存），数值大于21亿(时间戳、唯一ID)
2. 字符串：单引号 char 只能一个字，基本类型；双引号 String 随便多少字，是一个类，日常开发基本用这个字符串。
```java
/** 基本类型(保存在栈中) */
int age = 25 // 整数
long big = 100L // 长整型
double price = 99.9 // 浮点
boolean isOk = true // 只有true/false
char grade = 'A'  // 单引号字符

/** 引用类型(存放在堆中，变量存地址) */
String name = "张三" // String 是类
int[] arr = {1,2,3} // 数组是对象
final double PI = 3.13 // 常量 不可重新赋值
```
3. 数组-引用类型
注意：
Java 数组一旦创建，长度不可变，没有类似 JS 的 push/pop/shift/unshift等操作数组的方法
```java
/**
 * 声明并初始化
 * int[]：声明一个变量 arr1，类型是 [int 数组]
 * new int[5]: 在堆内存中分配一块连续空间，能存放5个int值，每个元素被初始化为默认值0
*/ 
int[] arr1 = new int[5];

/**
 * 数组静态初始化(直接在声明时给出具体值)
 * 等价写法 int[] arr2 = new int[] {1,2,3}
 * 前端类比：let arr2 = [1,2,3] // 但是前端的JS数组长度可变，类型可混
 */
int[] arr2 = {1,2,3}
/**
 * 字符串数组 
 */
String[] names = new String[] {"Tom", "Jerry"}

/**
 * 增强版数组 ArrayList<String>是一个自带动态扩容和增删方法的“高级数组”，
 * 底层仍用数组实现，但通过封装提供了更高的灵活性
 */

/**
 * 声明一个变量，类型时 ArrayList<String>，表示这个列表只能存放String类型的对象
 * new ArrayList<>() 创建对象：在堆上分配一个 ArrayList 实例。
 * 菱形语法（<>）:编译器自动推断右边泛型类型为String, 如果一开始确定大概元素数量，可以写 new ArrayList<>(10) 减少扩容开销
 */
ArrayList<String> list = new ArrayList<>();
// 添加元素：将字符串"hello"放入ArrayList中。
list.add("hello")
// 删除元素
// 按索引删除
remove(index)
// 按对象删除   
remove(object)
// 例子：
ArrayList<String> list = new ArrayList<>(Arrays.asList("A", "B", "A"))
list.remove("A") // 删除第一个 "A"
// 获取长度
list.size()
```
#### 方法(函数)
注意：
- 参数个数必须匹配，否则编译错误
- 无法在方法内部定义方法(局部类很麻烦，一般不用)
```java
// 方法必须在类中，格式：[修饰符] 返回值类型 方法名(参数列表)
public int add(int a, int b){
    return a + b
}

// 无返回值要用 void
public void sayHello(String name) {
    System.out.printIn("Hello " + name)
}

// 可变参数(类似 JS 的 扩展运算符 ...rest)
public void printAll(String ...args) {
    for(String s:args)
    System.out.printLn(s)
}
```
### 面向对象基础(与TS相似但更严格)
1. 类与对象
总结：
- 属性默认不加修饰符为包可见(不推荐)， 比如：package com.demo; 同一个包下的类可以访问该类的值，
  但包外的类(即使继承了该类)不能访问。
- 每个文件最多一个 public 类，且文件名必须与类名一致
```java
// 定义类（文件名为 User.java）
public class User {
   // 属性(字段)通常 private
   private String name;
   private int age;
   // 构造方法（无返回值，名称与类相同）
   public User(String name, int age) {
    this.name = name;
    this.age = age;
   }

   // 方法
   public void introduce() {
     System.out.printIn(name + "，年龄" + age);
   }

   public String getName() {
    return name;
   }
   public void setName(String name) {
      this.name = name;
   }
}
// 使用
User u = new User("小明", 18);
u.introduce(); // 小明 ,年龄18
```

2. 封装(访问修饰符)
最佳实践：属性一律 private, 提供 public getter/setter
|   修饰符   |    同类    |    同包    |    子类    |    任何地方 |
|---------- |------------|------------|------------|------------|
| private   |     √      |     ×      |     ×      |     ×      | 
| (default) |     √      |     √      |     ×      |     ×      |
| protected |     √      |     √      |     √      |     ×      |
| (default) |     √      |     √      |     √      |     √      |

3. 继承
```java
// 父类
class Animal {
    protected String name;
    public void eat() {
        System.out.printIn("吃东西")；
    }
}
// 子类用 extends 继承
class Dog entends Animal {
    public void bark(){
        System.out.printIn("汪汪");
    }
    // 重写方法(建议加 @Override 注解)
    @Override
    public void eat() {
        System.out.printIn("狗吃骨头")
    }
}
// 调用
Dog d = new Dog();
d.eat(); // 狗吃骨头
```
4. 多态
多态条件：继承 + 方法重写 + 父类引用指向子类对象。
```java
Animal a = new Dog(); // 父类引用指向子类对象
a.eat(); // 调用 Dog 的 eat(动态绑定)
// a.bark()   // 编译错误，五类没有 bark
```

5. 接口(Interface)
总结：
- java接口不能有实例属性
- 实现用 implements, 多实现用逗号分隔
- java 8 后可以有默认方法和静态方法
- 普通具体类：必须实现接口中所有抽象方法
- 接口定义了"最低要求"，而不是"全部限定"。实现类必须满足接口，但依然可以自由扩展。
```java
// 定义接口（类似 TS 的 interface）
interface Flyable {
    void fly(); // 抽象方法，默认 public abstract
    default void land() { // 默认方法
        System.out.print("降落");
    }
    static void staticMethod() {} // 静态方法
}

// 实现接口（可以多实现）
class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.printIn("飞鸟");
    }
}
// 使用
Flyable f = new Bird();
f.fly() // 飞鸟
```

6. 抽象类
总结：
- 抽象类可以有实例变量、非抽象方法，单继承
- 接口更轻量，多实现，常用于定义能力
```java
abstract class Shape {
    protected String color;
    // 抽象方法(没有方法体)
    public abstract double area();
    // 非抽象方法
    public void setColor(String color){
        this.color = color;
    }
}

class Circle extends Shape {
    private double radius;
    public Circle(double radius){
        this.radius = radius;
        @Override
        public double area(){
            return Math.PI * radius * radius;
        }
    }
}
```

## 第二阶段：数据库与MyBatis
### 常用数据库有哪些
数据库分为关系型数据库和NoSQL非关系型数据库

关系型数据库
|   数据库   |    特点    |    典型场景    |
|---------------|------------|------------|
| MySQL | 开源、轻量、社区活跃 | Web应用，中小型项目 |
| PostgreSQL | 功能强大、标准兼容性好，支持JSON等 | 复杂查询、数据一致性要求高 |
| Oracle | 商业数据库，性能强悍 | 金融、电信等大型企业级系统 |
| 达梦数据库(Dameng Database) | 国产，高度兼容Oracle,安全性高 | 党政军、金融、能源等信创工程替代 Oracal 首选 |

非关系型数据库
|   数据库   |    类型    |    特点    |  典型场景  |
|---------------|------------|------------|------------|
| Redis | 键值存储/缓存 | 内存速度，丰富数据结构 | 缓存、会话管理、计数器 |
| MongoDB | 文档型 | 类JSON存储，动态模式 | 日志、内容管理、大数据量 |

## 第三阶段：Spring Boot核心
## 第四阶段: 常用工具与中间件
## 第五阶段：微服务与进阶