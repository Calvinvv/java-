
# 1、Java程序编译出来的.class文件叫做二进制字节码文件


Java 编译器将 `.java` 源代码编译为 `.class` 文件，生成的字节码是平台无关的。字节码可以通过 Java 虚拟机（JVM）在不同的操作系统和硬件平台上执行，这使得 Java 程序具有很强的跨平台能力。JVM 在执行字节码时，会做很多优化，比如即时编译（JIT）和垃圾回收等，以提高程序的执行效率。

通过字节码，Java 程序避免了直接与操作系统和硬件进行交互的问题，确保了程序在不同环境中的一致性。然而，字节码的执行效率依赖于 JVM 的实现和系统资源，因此性能优化是 Java 开发中的一个重要课题。


# 2、javac.exe，java.exe命令的含义和用法必须掌握

> `javac.exe` 是 Java 编译器，用于将 `.java` 源代码编译成 `.class` 字节码文件，这个字节码可以被 Java 虚拟机（JVM）执行。`java.exe` 是启动 Java 程序的命令，它运行 JVM，加载 `.class` 文件并将其执行。`javac.exe` 和 `java.exe` 的使用是理解 Java 编程模型的基础，掌握它们有助于更好地理解编译与执行的过程，同时也能更高效地配置和调试 Java 开发环境。

# 3. 子类方法覆盖父类方法时要求哪些必须与父类方法相同

新的方法必须与父类中的方法有相同的名字，相同的参数列表，但在它们的方法体内必须有不同代码，这就是方法的覆盖

覆盖的前提条件是继承，没有继承就谈不上覆盖



# 4. 构造方法的特点
它与类同名
它没有返回值
它没有返回类型，甚至void
它经常被用来设置成员变量的初始值‘

# 5. 线程wait/notify方法的含义、用法，java创建线程的两种方法必须掌握，代码必须会写，参考课上讲的线程demo和PPT

`wait()` 和 `notify()` 是 Java 中用于线程间同步和通信的基本工具，
它们是 `Object` 类的方法，每个线程都可以在任何对象的锁上调用这两个方法。

## `wait()` 
`wait()` 使线程进入被操作对象的等待池中等待，并释放该对象的锁，直到其他线程调用相应的 `notify()` 或 `notifyAll()` 方法将该线程唤醒。
- 当条件未满足时调用，使线程进入被操作对象的等待池中等待
- 释放线程持有的锁，因此不会产生死锁
- 必须放在synchronized方法中
- 必须放在try块中，捕获InterruptedException异常


## `notify()` 
`notify()` 方法使线程从被操作对象的等待池离开.会唤醒一个正在等待该对象锁的线程，而 `notifyAll()` 则唤醒所有等待线程。

## 创建线程
### 创建线程的方法1 — 继承java.lang.Thread类
```java
public class MyThread extends Thread{
    public MyThread(String name){
        super(name); //1.执行父类Thread(String name)构造器，为当前线程设置名字了
    }
    @Override
    public void run() {
        //2.currentThread() 哪个线程执行它，它就会得到哪个线程对象。
        Thread t = Thread.currentThread();
        for (int i = 1; i <= 3; i++) {
            //3.getName() 获取线程名称
            System.out.println(t.getName() + "输出：" + i);
        }
    }
}
```
Thread类的构造方法
public Thread( ) 
public Thread(Runnable target)
public Thread(String name)

覆盖Thread类的run方法，当该线程处于运行态时，就执行run方法的内容

main方法也是一个独立运行的线程

### 创建线程的方法2 — 实现java.lang.Runnable接口

```java
// 1. 实现 Runnable 接口
class MyRunnable implements Runnable {
	public MyThread(String name){
	        super(name); //1.执行父类Thread(String name)构造器，为当前线程设置名字了
    }
    @Override
    public void run() {
        // 线程执行的任务
        for (int i = 1; i <= 5; i++) {
            System.out.println(Thread.currentThread().getName() + " 正在执行，i = " + i); 
        }
    }
}

```
Runnable接口中只有一个run方法

public abstract void run();


# 6、Thread.sleep函数必须放在try块中，catch哪一个异常类
`Thread.sleep()` 可能会抛出 `InterruptedException` 异常



# 7、8种基本数据类型和它们的包裹类，包裹类中常用的方法必须掌握

| 数据类型 |   关键字   | 内存占用 |               取值范围                |
| :--: | :-----: | :--: | :-------------------------------: |
|  整数  |  byte   |  1   |    负的2的7次方 ~ 2的7次方-1(-128~127)    |
|      |  short  |  2   | 负的2的15次方 ~ 2的15次方-1(-32768~32767) |
|      |   int   |  4   |        负的2的31次方 ~ 2的31次方-1        |
|      |  long   |  8   |        负的2的63次方 ~ 2的63次方-1        |
| 浮点数  |  float  |  4   |    1.401298e-45 ~ 3.402823e+38    |
|      | double  |  8   |  4.9000000e-324 ~ 1.797693e+308   |
|  字符  |  char   |  2   |              0-65535              |
|  布尔  | boolean |  1   |            true，false             |
#### 需要记忆以下几点
byte类型的取值范围：
-128 ~ 127
int类型的大概取值范围：
-21亿多 ~ 21亿多
整数类型和小数类型的取值范围大小关系：
double > float > long > int > short > byte




### 1. 装箱和拆箱（包装类中的基本数据类型值）
每个包装类都提供了获取其包装的基本类型值的方法。这些方法适用于所有的包装类，并且都遵循类似的命名约定，方便记忆。
- **`X.XValue()`**：返回包装类中的基本数据类型值。
- **`X.valueOf(x)`**：将基本类型转换为对应的**包装类对象**。

例如：
- `Integer s;  s.intValue()`：返回该 `Integer` 对象的 `int` 值。
- `Integer.valueOf(String s)`:返回`Integer` 对象。
### 2. 字符串转换
这类方法大多数都具有类似的模式，即将 `String` 类型转换为对应的基本类型或包装类的对象，适用于所有数据类型。
- **`parseXxx(String s)`**：将字符串解析为**基本数据类型**。
- `Integer.valueOf(String s)`:将字符串解析为**对象**。
- `toString(x)` 和 `Xxx.toString()` 用于转换为字符串并返回。
### 3. 最大值最小值
`Float.MAX_VALUE`：表示 `float` 类型的最大值 `3.4028235e38`。
`Float.MIN_VALUE`：表示 `float` 类型的最小值 `1.4e-45`。
### 3. **比较方法**（用于比较包装类对象的大小）

Java 的包装类大多继承了 `Comparable<T>` 接口
- **`compareTo(T other)`**：比较两个包装类对象的大小。
例如：
- `Integer.compareTo(Integer other)`
- `Double.compareTo(Double other)`

对于 `Boolean` 和 `Character`，它们也提供了类似的比较方法：
- `Boolean.compareTo(Boolean other)`：比较两个 `Boolean` 对象的值（`true` 比 `false` 大）。
- `Character.compareTo(Character other)`：比较两个字符的 Unicode 值。
### 4. **字符操作方法**（仅 `Character` 类）
- **`isXxx(char ch)`**：判断字符是否符合某种特征（例如数字、字母、空格等）。
    - `Character.isDigit(char ch)`：检查是否是数字。
    - `Character.isLetter(char ch)`：检查是否是字母。
    - `Character.isWhitespace(char ch)`：检查是否是空白字符。
- **`toXxx(char ch)`**：转换字符的大小写。
    - `Character.toLowerCase(char ch)`：转换为小写字符。
    - `Character.toUpperCase(char ch)`：转换为大写字符。

这些方法特定于字符处理，因此单独记忆。

### 5. **进制转换方法**（主要用于整数类型的包装类）

- **`toBinaryString(int i)`**：将整数转换为二进制字符串。
- **`toHexString(int i)`**：将整数转换为十六进制字符串。
- **`toOctalString(int i)`**：将整数转换为八进制字符串。
### 6. **特殊值检查方法**（检查是否为特殊值）

部分包装类提供了用于检查特殊值（例如 `NaN`，`null` 等）的工具方法。主要是 `Float` 和 `Double` 类型。
- **`isNaN()`**：检查当前值是否是 `NaN`（Not-a-Number）。
例如：
- `Float.isNaN(float f)`
- `Double.isNaN(double d)`





# 8、String类的常用方法必须掌握，看录课讲解

### 1. **`length()`**
- **功能**：返回字符串的长度。
  ```java
  int len = str.length();  // 获取字符串的长度
  ```
### 2. **`charAt(int index)`**
- **功能**：返回指定索引位置的字符。
  ```java
  char ch = str.charAt(1);  // 获取索引为 1 的字符
  ```
### 3. **`substring(int beginIndex)`**
- **功能**：返回从指定索引开始到字符串末尾的子字符串。
  ```java
  String sub = str.substring(2);  // 从索引 2 开始到字符串末尾
  ```
### 4. **`substring(int beginIndex, int endIndex)`**
- **功能**：返回从 `beginIndex` 到 `endIndex`（不包括 `endIndex`）之间的子字符串。
  ```java
  String sub = str.substring(1, 4);  // 获取从索引 1 到 4 的子字符串
  ```
### 5. **`indexOf(String str)`**
- **功能**：返回指定子字符串第一次出现的位置。
  ```java
  int index = str.indexOf("l");  // 查找字符 "l" 的首次出现位置
  ```
### 6. **`indexOf(String str, int fromIndex)`**
- **功能**：从指定索引 `fromIndex` 开始，返回指定子字符串第一次出现的位置。
  ```java
  int index = str.indexOf("l", 3);  // 从索引 3 开始查找 "l"
  ```
### 7. **`lastIndexOf(String str)`**
- **功能**：返回指定子字符串最后一次出现的位置。
  ```java
  int index = str.lastIndexOf("l");  // 查找字符 "l" 的最后出现位置
  ```
### 8. **`toUpperCase()`**
- **功能**：将字符串转换为大写字母。
  ```java
  String upper = str.toUpperCase();  // 转为大写
  ```
### 9. **`toLowerCase()`**
- **功能**：将字符串转换为小写字母。
  ```java
  String lower = str.toLowerCase();  // 转为小写
  ```
### 10. **`trim()`**
- **功能**：去除字符串两端的空白字符。
  ```java
  String trimmed = str.trim();  // 去除前后空格
  ```
### 11. **`replace(char oldChar, char newChar)`**
- **功能**：替换字符串中的字符。
  ```java
  String replaced = str.replace('l', 'x');  // 替换所有 'l' 为 'x'
  ```
### 12. **`replaceAll(String regex, String replacement)`**
- **功能**：使用正则表达式替换字符串中的匹配部分。
  ```java
  String replaced = str.replaceAll("[0-9]", "#");  // 替换所有数字为 "#"
  ```
### 13. **`split(String regex)`**
- **功能**：根据正则表达式分割字符串并返回数组。
  ```java
  String[] fruits = str.split(",");  // 用逗号分割字符串
  ```
### 14. **`startsWith(String prefix)`**
- **功能**：判断字符串是否以指定前缀开始。
  ```java
  boolean result = str.startsWith("He");  // 判断是否以 "He" 开头
  ```
### 15. **`endsWith(String suffix)`**
- **功能**：判断字符串是否以指定后缀结束。
  ```java
  boolean result = str.endsWith("lo");  // 判断是否以 "lo" 结尾
  ```
### 16. **`contains(CharSequence sequence)`**
- **功能**：判断字符串是否包含指定的子字符串。
  ```java
  boolean result = str.contains("ell");  // 判断是否包含 "ell"
  ```
### 17. **`equals(Object anObject)`**
- **功能**：比较两个字符串的内容是否相等。
  ```java
  boolean result = str1.equals(str2);  // 比较两个字符串是否相等
  ```
### 18. **`equalsIgnoreCase(String anotherString)`**
- **功能**：忽略大小写比较两个字符串是否相等。
  ```java
  boolean result = str1.equalsIgnoreCase(str2);  // 忽略大小写比较
  ```
### 19. **`valueOf(Object obj)`**
- **功能**：返回对象的字符串表示。
  ```java
  String str = String.valueOf(123);  // 将对象转换为字符串
  ```
### 20. **`format(String format, Object... args)`**
- **功能**：使用指定格式生成格式化字符串。
  ```java
  String formatted = String.format("Hello, %s!", "World");  // 格式化字符串
  ```
### 21.`concat(String str)
```java
public String concat(String str)
```

### 总结：
这些方法提供了字符串的多种操作，如查找、替换、分割、转换大小写、比较、格式化等。掌握这些方法能有效地进行字符串处理。




# 9、jdbc编程中的Connection、Statement、ResultSet是什么，怎么用，必须掌握，课上讲的代码必须会写

```java
 public static void main(String args[]){
    //所有与JDBC相关接口和类的方法都要抛出SQLException异常
    //所以这些语句必须包含在try块中
    try {
      //加载JDBC-ODBC桥驱动程序
      Class.forName("com.mysql.jdbc.Driver");
      //连接ODBC数据源sqlserver，用户名和密码
      Connection conn = DriverManager.getConnection
                     ("jdbc:mysql://地址:3306/数据库", "用户名", "密码");
      //由数据库连接对象conn生成数据库语句对象stmt
      Statement stmt = conn.createStatement();
      //调用数据库语句对象的executeQuery方法
      //执行SQL语句select * from student，即从student表中读出所有学生
      ResultSet results = stmt.executeQuery("select * from student");

```

# 10、super，this关键字的用法
## this

**区分实例变量和局部变量/方法参数：** 在方法或构造器中，局部变量或参数的名称可以与实例变量同名，`this` 关键字可以帮助区分它们。

**调用当前类的构造函数：** `this()` 语法可以在一个构造函数中调用另一个构造函数。这条语句必须是第一句

## super

父类的构造方法不能被子类继承，但可以被子类的构造方法调用

我们经常用父类的构造方法创建子类对象中来自于父类的那部分数据

super关键字代表当前对象的父类对象，可以被用来调用父类的构造方法，且这条语句必须是第一句

- `super` 主要用于以下情况：
    1. **访问父类的实例变量：** 当子类和父类有相同的实例变量时，`super` 可以用来访问父类的实例变量。
    2. **调用父类的方法：** `super` 可以用来调用父类的方法，尤其是在子类重写了父类的方法时，`super` 可以帮助访问父类的原始方法。
    3. **调用父类的构造函数：** 在子类构造函数中，`super()` 用于调用父类的构造函数。



# 11、抽象方法和抽象类的特点、及其关系，看PPT课件的讲解

- abstract修饰符可以修饰类和方法
- 抽象类不能被new实例化
- 抽象方法不能是final，static，private，也不能是构造方法
- 抽象类经常包含抽象方法，但也可以不包含方法
- 抽象方法没有方法体


## 在以下三种情况下，某个类必须定义成抽象类：
- 当一个类中有抽象方法时
- 当该类为一个抽象类的子类，并且没有为所有抽象方法提供实现细节或方法体时
	- （抽象类的子类要覆盖抽象类中的抽象方法，否则它仍然是抽象的）
- 当一个类实现一个接口，并且没有为所有抽象方法提供实现细节或方法体时

# 12、接口和abstract类都不能直接用new实例化

现实生活中我们经常遇到这样一种情况：几个类并没有继承关系，彼此毫不相关，但它们却有类似的行为描述，这时我们可以定义一个接口，其中包含这个行为，并让这些类实现这个接口，实现其中的行为

接口被用来定义一组类要实现的方法

Java 接口（interface）是抽象方法和常量的集合，换言之，在接口中只能定义abstract方法和final变量

接口中的方法默认是public abstract类型，方法不能有定义（方法体）
接口中的常量默认是public static final类型


类实现接口:
在类头中`implements + 接口名
` class Dog implements Speaker, Moveable`
提供接口中的每个抽象方法的具体实现


接口也可以像类一样继承
子接口继承父接口中的所有的抽象方法
与类继承规则不同的是：一个接口不仅可以继承一个父接口，它还可以继承多个父接口，是多继承
实现子接口的类必须父接口和子接口中的所有方法





# 13、final关键字的用法

final 修饰符可应用于类、方法和变量
- final类不能被继承

- final 方法
	- final方法不能被子类覆盖
	- 如果一个类为 final 类，那么它的所有方法都为隐式的 final 方法

- final 变量
	- 一个变量可以声明为final，这样做的目的是阻止它的内容被修改
	- 声明final变量后，只能被初始化一次，然后就不能对其值进行修改，final变量就是常量







# 14、继承与public、protected、private的关系，这几种类的成员的继承性是怎么样的，看录课讲解

在 Java 中，常用的访问修饰符有：

- **`public`**：公共的，可以被任何类访问（包括子类和非子类）。
- **`protected`**：受保护的，只能被
	- **1. 同一个包中的类**
	- **2. 所有子类访问。**
- **`private`**：私有的，只能被本类访问，子类和其他类无法直接访问。
- **`默认（包私有）`**：如果没有显式声明访问修饰符，默认为包私有，表示只能被同一个包中的类访问。

继承：
public和protected都可以被子类继承并访问，而private会被继承，但这些成员对子类是不可见的。如果子类想访问这些成员，必须通过父类提供的公共方法或保护方法来间接访问。




# 15、Object类是所有Java类的父类

在Java中，所有的类（不包括基本数据类型）都是从某个类派生的。`Object`类是所有Java类的最顶层父类，也就是说，所有类都隐式或显式地继承自`Object`类。

- 如果一个类没有显式声明继承自其他类，它会默认继承自`Object`类


# 16、类的继承是单一继承，接口的继承可以是多继承，类可以implements多个接口，也属于多继承

单一继承的最大目的是 **避免继承时出现方法冲突或不明确的继承关系**。如果允许一个类继承多个类，子类将会继承多个父类的相同方法，这样会导致编译器无法判断使用哪个父类的方法（即“菱形继承”问题）。为了避免这种情况，Java只允许类单继承。


而接口因为大多时仅包含抽象方法而不需要考虑菱形继承问题

Java 8 引入了接口的默认方法（`default`），允许接口提供方法的默认实现。虽然这种特性使得接口的功能更加灵活，但它引发了一个问题：**多继承的菱形继承问题**（Diamond Problem）。

这些规则如下：

1. **优先使用类的方法**：如果子类实现了接口的方法（无论是继承的接口还是默认方法），子类的方法优先级高于接口的默认方法。因此，子类可以重写接口的默认方法，从而避免冲突。
    
2. **冲突的情况下显式指定调用**：如果子类继承了多个接口，这些接口都提供了相同的方法的默认实现，并且子类没有重写这个方法，编译器会报错，要求子类明确指定该方法的实现来自哪个接口。子类可以使用 `super` 关键字来指定具体的接口。
    
3. **接口的继承**：当接口继承多个接口时，它继承的是方法的声明而不是实现。默认方法仅仅是接口定义的一种行为，并且只有在实现该接口的类没有重写该方法时才会使用。


# 17、多态可以是父类的引用变量引用子类的对象，也可以是接口的引用变量引用实现类的对象，参考换武器的例子

### 多态的实现机制

- **方法重写（Overriding）**：子类重新定义父类的方法，实现特定功能。
- **方法调用时的动态绑定**：在程序运行时，JVM会根据对象的实际类型来确定调用哪个方法，而不是根据引用的类型。


Java中的多态是通过**方法重载**和**方法重写**两种机制实现的



### 多态的类型
在Java中，多态可以分为两种类型：

#### 1. 编译时多态（静态多态）
通过方法的重载（Method Overloading）来实现。这是编译器在编译时就决定调用哪个方法，因此被称为静态多态。
   ```java
   class Calculator {
       // 加法方法重载
       public int add(int a, int b) {
           return a + b;
       }
       public double add(double a, double b) {
           return a + b;
       }
   }
   ```

#### 2. **运行时多态（动态多态）**
   - 通过方法的重写（Method Overriding）来实现。
   - **java变量的声明类型和实例类型可以不一样**，比如:`Animal animal1 = new Dog();
   - 因为在继承关系中，子类可以重写父类的方法，动态多态就是通过父类引用指向子类对象，在运行时决定调用哪个类的方法。
   - 运行时多态是通过**动态绑定**实现的，具体调用哪个方法是在程序运行时决定的。
   ```java
   class Animal {
       // 父类方法
       public void sound() {
           System.out.println("动物发出声音");
       }
   }
   class Dog extends Animal {
       // 子类重写父类方法
       @Override
       public void sound() {
           System.out.println("狗汪汪叫");
       }
   }
   class Cat extends Animal {
       // 子类重写父类方法
       @Override
       public void sound() {
           System.out.println("猫咪喵喵叫");
       }
   }
   public class Main {
       public static void main(String[] args) {
           Animal animal1 = new Dog();
           Animal animal2 = new Cat();

           animal1.sound();  // 调用 Dog 类的 sound() 方法
           animal2.sound();  // 调用 Cat 类的 sound() 方法
       }
   }
   ```

   这个例子中，虽然 `animal1` 和 `animal2` 的声明类型是 `Animal`，但是它们分别指向 `Dog` 和 `Cat` 类的实例。根据对象的实际类型，调用不同的 `sound` 方法。这就是典型的运行时多态。


Java允许将子类类型赋给父类类型的引用，这种转换叫做**向上转型**。向上转型是安全的，因为子类对象总是包含父类的所有成员。

- **向上转型**：子类对象可以赋给父类变量。
- 向下转型：同时，如果父类引用实际指向的是子类对象`dog`，则这个父类引用也可以通过显性转换 转换成 子类引用`dog`,如果向下转型时不对应，会报错
```java
Animal animal = new Dog(); // 向上转型，合法 
Dog dog = (Dog) animal; // 向下转型，合法，前提是animal确实指向一个Dog对象
```


接口也能当成父类来使用


# 18、从键盘输入Scanner类必须会用

在Java中，使用 `Scanner` 类可以从键盘输入数据。`Scanner` 类是 Java 标准库中的一个工具类，位于 `java.util` 包中，提供了多种方法来读取不同类型的输入（如字符串、整数、浮点数等）。通过 `Scanner` 类，程序可以与用户进行交互，接收键盘输入并处理。

### 1. **如何使用 `Scanner` 类**

#### 1.1 **导入 Scanner 类**
在使用 `Scanner` 类之前，首先需要导入 `java.util.Scanner` 类：

```java
import java.util.Scanner;
```

#### 1.2 **创建 Scanner 对象**
`Scanner` 类通过构造函数来初始化，通常将标准输入流（`System.in`）传递给 `Scanner` 对象，以便它能够从键盘读取输入。

```java
Scanner scanner = new Scanner(System.in);
```

- `System.in` 是指向键盘输入的标准输入流，它会连接到 `Scanner` 类，使其可以从控制台读取数据。
- 其他常见使用场景：
	- **从键盘读取输入：** `new Scanner(System.in)` 是最常见的用法，用于与用户交互并获取输入。
	- **从文件读取数据：** `new Scanner(new File("file.txt"))` 用于读取文件中的文本数据。
	- **从字符串读取数据：** `new Scanner("Hello World!")` 用于从字符串中读取分隔的数据。
	- **从输入流读取数据：** 可以从 `ByteArrayInputStream`、`FileInputStream` 等输入流对象中读取数据。

#### 1.3 **读取用户输入**
`Scanner` 类提供了多个方法，用来读取不同类型的输入，如 `nextLine()`、`nextInt()`、`nextDouble()` 等。

- `nextLine()`：读取一行字符串（包括空格）。
- `next()`：读取下一个单词（以空格为分隔符）。
- `nextInt()`：读取一个整数。
- `nextDouble()`：读取一个浮点数。
- `nextBoolean()`：读取一个布尔值（`true` 或 `false`）。

#### 1.4 **关闭 Scanner 对象**
使用完 `Scanner` 后，应当调用 `scanner.close()` 来关闭 `Scanner` 对象，释放资源。注意，关闭 `Scanner` 后，你不能再通过它读取数据。

```java
scanner.close();
```

### 3. **常见的 `Scanner` 方法**

| 方法              | 说明                       |
| --------------- | ------------------------ |
| `next()`        | 读取下一个单词（空格分隔的字符串）。       |
| `nextLine()`    | 读取整行，包括空格。               |
| `nextInt()`     | 读取整数。                    |
| `nextDouble()`  | 读取浮点数。                   |
| `nextBoolean()` | 读取布尔值（`true` 或 `false`）。 |
| `hasNext()`     | 如果有下一个输入，返回 `true`。      |
| `hasNextInt()`  | 如果下一个输入是整数，返回 `true`。    |
| `hasNextLine()` | 如果有下一行输入，返回 `true`。      |

### 4. **注意事项**
- **换行符问题：** 当使用 `nextInt()`、`nextDouble()` 等方法读取数值后，输入流中会留下一个换行符（`\n`），这会导致接下来的 `nextLine()` 方法直接读取到一个空字符串。因此，在读取数值后，通常需要调用一次 `scanner.nextLine()` 来清空输入缓冲区。
  ```java
  int num = scanner.nextInt();
  scanner.nextLine();  // 读取并丢弃掉换行符
  String line = scanner.nextLine();  // 现在可以安全读取一行文本
  ```
- **异常处理：** `Scanner` 在读取数据时可能会遇到非法输入，比如用户输入了一个字符串而不是数字。可以使用 `try-catch` 语句来捕获并处理这些异常，避免程序崩溃。例如：
  ```java
  try {
      int number = scanner.nextInt();
  } catch (Exception e) {
      System.out.println("输入无效，请输入一个整数！");
  }
  ```
- **Scanner 对象的关闭：** 在使用完 `Scanner` 后，最好调用 `scanner.close()` 来关闭对象。但需要注意，关闭 `Scanner` 后，如果你还需要通过 `System.in` 读取其他数据，可能会出问题，因为 `System.in` 被关闭了。所以一般情况下，在程序结束时再关闭 `Scanner`。

# 19、io流类的四大根类是什么

## InputStream是所有字节输入流的父类
子类名为`XXXInputStream

#### 关键方法：
- `int read()`：从输入流中读取一个字节并返回。
- `int read(byte[] b)`：从输入流中读取多个字节到数组中。
## OutputStream是所有字节输出流的父类
子类名为`XXXOuputStream
#### 关键方法：
- `void write(int b)`：写入一个字节。
- `void write(byte[] b)`：写入一个字节数组。
- `void write(byte[] b, int off, int len)`：从指定的字节数组中，写入指定范围的字节。
## Reader是所有文本输入流的父类
子类名为`XXXReader
#### 关键方法：
- `int read()`：读取一个字符。
- `int read(char[] cbuf)`：读取字符到数组中。
## Writer是所有文本输出流的父类
子类名为`XXXWriter
#### 关键方法：
- `void write(int c)`：写入一个字符。
- `void write(char[] cbuf)`：写入一个字符数组。
- `void write(char[] cbuf, int off, int len)`：写入字符数组的指定部分。

## 与标准输出流
- **`System.in`** 是 `InputStream` 类型，它继承了 `InputStream` 类的基本方法（如 `read()`）。
- **`System.out`** 和 **`System.err`** 都是 `PrintStream` 类型，而 `PrintStream` 是 `OutputStream` 的一个子类，它提供了更多的便捷方法（如 `println()`、`print()`），可以直接将数据输出到控制台。

它们实际上是 **字节流**（因为 `InputStream` 和 `OutputStream` 都是字节流类），这意味着它们按字节而非字符的方式处理数据。因此，如果需要进行字符输入输出（如读取文本文件），我们通常需要将它们转换为字符流，如使用 `InputStreamReader` 和 `OutputStreamWriter` 来处理字符数据。



# 20、FileReader和FileWriter类必须会用，代码必须会写，看课上写的例题，也要看录课讲解

## FileReader
```java
FileReader fr = null; 
try { 
// 创建 FileReader 对象，用于读取文件
fr = new FileReader("example.txt"); 
int charRead; 
// 逐个字符读取文件，直到文件结束 
while ((charRead = fr.read()) != -1)
{
	System.out.print((char) charRead); // 输出字符
}
```




## FileWriter
```java
FileWriter fw = null; 

try {// 创建 FileWriter 对象，用于向文件写入数据 
fw = new FileWriter("example.txt");

// 向文件写入单个字符 fw.write('H'); 
fw.write('e'); 
fw.write('l');

// 向文件写入字符串
fw.write("World!"); 
fw.write("\n"); 

// 向文件写入字符数组 
char[] chars = {'J', 'a', 'v', 'a'}; 
fw.write(chars);

```

# 21、所有讲过的常用的类都属于哪些包，必须要知道
