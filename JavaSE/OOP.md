# 面向对象和面向过程的区别

1. 面向过程：强调的是功能行为，以函数为最小单位，考虑怎么做。
2. 面向对象：强调具备了功能的对象，以类/对象为最小单位，考虑谁来做。

属性 = 成员变量 = field = 域、字段

方法 = 成员方法 = 函数 = method

创建类的对象 = 类的实例化 = 实例化类

# 非static属性（成员变量） VS 局部变量

1. 相同点

   1. 定义的格式：数据类型  变量名  =  变量值
   2. 先声明，后使用。
   3. 都有其对应的作用域。

2. 不同点

   1. 在类中声明的位置不同

      属性：直接定义在类的一对{}内。

      局部变量：声明在方法内、方法形参、代码块内、构造器形参、构造器内的变量。

   2. 权限修饰符的不同

      属性：可以在声明属性时指明其权限，常用的权限修饰符：private、public、缺省、protected。

      局部变量：不可以使用权限修饰符。

   3. 默认初始化值不同

      属性：根据类型都有默认初始化值。

      ​		整型：0

      ​		浮点型：0.0

      ​		字符型：\u0000

      ​		布尔型：false

      ​		引用数据类型：null

      局部变量：没有默认初始化值，所以调用局部变量之前必须先显式赋值。

      ​		特别的：形参在调用时赋值。

   4. 在内存中加载的位置不同

      属性：加载到堆空间中

      局部变量：加载到栈空间


# return

使用范围：方法体中

作用：①结束方法，return；

​		   ②结束方法并返回数据，return 数据；

return关键字后不可有执行语句。

# 万事万物皆对象

1. 在Java语言的范畴内，我们都将功能、结构等封装到类中，通过类的实例化，来调用具体的功能结构。

   ​	> Scanner，String等

   ​	> 文件：File

   ​	> 网络资源：URL

2. 涉及到Java语言与前端HTML、后端的数据库交互时，前后端的结构在Java层面交互时都体现为类、对象。

# 方法

## 方法的重载

1. 同一个类中，方法名相同，参数列表不同。（参数个数或类型或顺序不同）
2. 跟方法的权限修饰符、返回值类型、形参参数名、方法体无关。
3. 若找不到与传入参数匹配的方法则匹配自动类型提升的方法。

## 可变个数的形参

格式

```java
public void show(String ... strs){
    for (int i = 0; i < strs.length; i++) {
        System.out.println(strs[i]);
    }
}
```



1. 调用可变个数形参的方法时，传入的参数个数可以是：**0个**、1个、2个。。。n个。
2. `public void show(String ... strs){}`与`public void show(String[] strs){}`不能共存。编译器认为是同一个方法。
3. 可变个数的形参只能声明在参数列表的末尾，故一个方法只能声明一个可变形参。

## 方法参数的值传递机制

1. 形参：方法定义时，声明在小括号内的参数。

   实参：方法调用时，实际传递给形参的数据。

2. 值传递机制：

   如果参数是基本数据类型，此时实参赋给形参的是实参真实存储的数据值。

   如果参数是引用数据类型，此时实参赋给形参的是实参存储数据的地址值。

> 值传递(pass by value) 是指在调用函数时将实际参数**复制**一份传递到函数中，这样在函数中如果对**参数**进行修改，不会影响到实际参数。
>
> 引用传递(pass by reference) 是指在调用函数时将实际参数的地址**直接**传递到函数中，那么在函数中对**参数**所进行的修改将影响到实际参数。

当我们在main中创建一个User对象的时候，在堆中开辟一块内存，其中保存了name和gender等数据。然后hollis持有该内存的地址，假设为`0x123456`。当尝试调用pass方法，并且hollis作为实际参数传递给形式参数user的时候，会把这个地址`0x123456`交给user，这时，user也指向了这个地址。然后在pass方法内对参数进行修改的时候，即`user = new User();`会重新开辟一块假设地址为`0X456789`的内存，赋值给user。后面对user的任何修改都不会改变内存`0x123456`的内容。

上面这种传递是什么传递？肯定不是引用传递，如果是引用传递的话，在`user = new User();`的时候，实际参数的引用也应该改为指向`0X456789`，但是实际上并没有。

通过概念我们也能知道，这里是把实际参数的引用的地址复制了一份，传递给了形式参数。所以，上面的参数其实是值传递，把实参对象引用的地址当做值传递给了形式参数。

Java引用和C/C++指针的最大区别在于，引用的地址必指向对象，因为都是先在堆中new再把地址传给栈中的引用变量。而指针的地址指向的是一块内存空间，不考虑这块内存里存放的内容。指针可以进行算术运算，而引用不可以。

```java
public static void main(String[] args) {
    ParamTest pt = new ParamTest();
    User hollis = new User();
    hollis.setName("Hollis");
    hollis.setGender("Male");
    pt.pass(hollis);
    System.out.println("print in main , user is " + hollis); // print in main , user is User{name='Hollis', gender='Male'}
}
public void pass(User user) {
    user = new User();
    user.setName("hollischuang");
    user.setGender("Male");
    System.out.println("print in pass , user is " + user); // print in pass , user is User{name='hollischuang', gender='Male'}
}
```

常见问题

```java
int[] arr = {1, 2};
// 错误写法
swap(arr[0], arr[1]);
// 正确写法
swap(arr, 0, 1);
// 错误
public void swap(int i, int j){
    int temp = i;
    i = j;
    j = temp;
}
// 正确
public void swap(int[] arr, int i, int j){
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

```java
// 面试题一
public class Test {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;
        method(a, b); // 需要在method方法被调用后，仅打印出a=100,b=200.请写出method方法的代码。
        System.out.println("a="+a);
        System.out.println("b="+b);
    }
    // 方法一
    public static void method(int a, int b) {
        a = a * 10;
        b = b * 20;
        System.out.println("a="+a);
        System.out.println("b="+b);
        System.exit(0); // 终止当前运行的Java虚拟机。 该参数作为状态代码; 按照惯例，非零状态码表示异常终止。
    }
    // 方法二
    public static void method(int a, int b) {
        PrintStream ps = new PrintStream(System.out){
            @Override
            public void println(String x){
                if ("a=10".equals(x)) {
                    x = "a=100";
                }else if ("b=10".equals(x)) {
                    x = "b=200";
                }
                super.println(x);
            }
        };
        System.setOut(ps);
    }
}
```

```java
// 面试题二
// 定义一个int型数组，让数组的每个位置上的值除以首位元素，得到的结果作为该位置上的新值。遍历新数组。
int[] arr = new int[]{12, 3, 3, 36, 48, 72, 432};
// 错误写法
for (int i = 0; i < arr.length; i++) {
    arr[i] = arr[i] / arr[0];
    System.out.print(arr[i]+"\t"); // 1  3  3  36  48  72  432
}
// 正确写法一
for (int i = arr.length - 1; i >= 0; i--) {
    arr[i] = arr[i] / arr[0];
    System.out.print(arr[i]+"\t"); // 1  0.25  0.25  3  4  6  36
}
// 正确写法二
int temp = arr[0];
for (int i = 0; i < arr.length; i++) {
    arr[i] = arr[i] / temp;
    System.out.print(arr[i]+"\t"); // 1  0.25  0.25  3  4  6  36
}
```

```java
// 面试题三
// 问控制台分别打印什么内容？
int[] arr1 = new int[]{1, 2, 3};
System.out.println(arr1); // [I@15db9742	地址值
char[] arr2 = new char[]{'a', 'b', 'c'};
System.out.println(arr2); // abc
// 原因：打印arr1调用的是java.io.PrintStream.println(Object x);打印arr2调用的是java.io.PrintStream.println(char[] x);
// 字符串的底层就是字符数组，所以打印字符数组会和打印字符串一样直接输出内容
```

## 递归方法

1. 递归方法：在一个方法的方法体内调用它本身。
2. 方法递归包含一种隐式的循环，它会重复执行某段代码，但这种重复执行无须循环控制。递归一定要向已知方向递归，否则会变成无穷递归，类似于死循环。

```java
// 计算1到100之间所有自然数的和
public static void main(String[] args) {
    System.out.println(getSum(100));
}
public static int getSum(int n) {
    if (n == 1) {
        return 1;
    } else {
        return n + getSum(n - 1);
    }
}
```

```java
// 已知有一个数列：f(0) = 1, f(1) = 4, f(n+2) = 2 * f(n+1) + f(n),其中n是大于0的整数，求f(10)的值。
public static void main(String[] args) {
    System.out.println(f(10));
}
public static int f(int n) {
    if (n == 0) {
        return 1;
    } else if (n == 1) {
        return 4;
    } else {
        return 2 * f(n-1) + f(n-2);
        return f(n+2) - 2 * f(n+1); // 错误写法，已知值为f(0),f(1)，应该向这两个值靠近，而不是向f(n+2)和f(n+1)
    }
}
```

```java
// 斐波那契数列
// 输入一个数据n，计算斐波那契数列(Fibonacci)的第n个值，并将整个数列打印出来。
// 1  1  2  3  5  8  13  21  34  55  .....
// 规律：从第三个数起任意一个数等于前两个数之和，f(n) = f(n-1) + f(n-2)
public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    System.out.println("请输入想要获取斐波那契数列的第几个数：");
    int i = scanner.nextInt();
    System.out.println("斐波那契数列的第" + i + "个数为：" + f(i));
    for (int j = 1; j <= i; j++) {
        System.out.print(f(j) + "\t");
    }
}
public static int f(int n) {
    if (n == 1 || n == 2) {
        return 1;
    } else {
        return f(n-1) + f(n-2);
    }
}
// 爬楼梯问题实际上也是斐波那契数列问题
// 假设你正在爬楼梯。需要 n 阶你才能到达楼顶。每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？注意：给定 n 是一个正整数。
// 第n个台阶只能从第n-1或者n-2个上来。到第n-1个台阶的走法 + 第n-2个台阶的走法 = 到第n个台阶的走法，已经知道了第1个和第2个台阶的走法，一路加上去。
```

```java
// 汉诺塔问题
// 从左到右有A、B、C三根柱子，其中A柱子上面有从小叠到大的n个圆盘，现要求将A柱子上的圆盘移到C柱子上去，期间只有一个原则：一次只能移到一个盘子且大盘子不能在小盘子上面，求移动的步骤和移动的次数。
public class TowersOfHanoi {
    static int m = 0;//标记移动次数

    //实现移动的函数
    public static void move(int disks, char N, char M) {
        System.out.println("第" + (++m) + " 次移动 : " + " 把 " + disks + " 号圆盘从 " + N + " ->移到->  " + M);
    }

    //递归实现汉诺塔的函数
    public static void hanoi(int n, char A, char B, char C) {
        if (n == 1)//圆盘只有一个时，只需将其从A塔移到C塔
            TowersOfHanoi.move(1, A, C);//将编b号为1的圆盘从A移到C
        else {//否则
            hanoi(n - 1, A, C, B);//递归，把A塔上编号1~n-1的圆盘移到B上，以C为辅助塔
            TowersOfHanoi.move(n, A, C);//把A塔上编号为n的圆盘移到C上
            hanoi(n - 1, B, A, C);//递归，把B塔上编号1~n-1的圆盘移到C上，以A为辅助塔
        }
    }

    public static void main(String[] args) {
        Scanner imput = new Scanner(System.in);
        char A = 'A';
        char B = 'B';
        char C = 'C';
        System.out.println("******************************************************************************************");
        System.out.println("这是汉诺塔问题（把A塔上编号从小号到大号的圆盘从A塔通过B辅助塔移动到C塔上去");
        System.out.println("******************************************************************************************");
        System.out.print("请输入圆盘的个数：");
        int disks = imput.nextInt();
        TowersOfHanoi.hanoi(disks, A, B, C);
        System.out.println(">>移动了" + m + "次，把A上的圆盘都移动到了C上");
        imput.close();
    }
}
```

```java
// 快速排序
// 1.在数组中选一个基准数（通常为数组第一个）;
// 2.将数组中小于基准数的数据移到基准数左边，大于基准数的移到右边;
// 3.对于基准数左、右两边的数组，不断重复以上两个过程，直到每个子集只有一个元素，即为全部有序。
private static void swap(int[] data, int i, int j) { // 定义一个方法用与交换数组下标为i和j的元素
    int temp = data[i];
    data[i] = data[j];
    data[j] = temp;
}
private static void subSort(int[] data, int start, int end) {
    if (start < end) {
        int base = data[start];
        int low = start;
        int high = end + 1;
        while (true) {
            while (low < end && data[++low] - base <= 0) {
            }
            while (high > start && data[--high] >= 0) {
            }
            if (low < high) {
                swap(data, low, high);
            } else {
                break;
            }
        }
        swap(data, start, high);
        subSort(data, start, high - 1);
        subSort(data, high + 1, end);
    }
}
public static void quickSort(int[] data) {
    subSort(data, 0, data.length - 1);
}
```

# 面向对象三大特征

## 封装

程序设计追求"高内聚、低耦合"

> 高内聚：类的内部数据操作细节自己完成，不允许外部干涉。
>
> 低耦合：仅对外暴露少量的方法用于使用。

隐藏对象内部的复杂性，只对外公开简单的接口，便于外界调用，从而提高系统的可拓展性、可维护性。通俗来说，把该隐藏的隐藏起来，该暴露的暴露出来。这就是封装性的设计思想。

封装性的体现（举例）：

1. 将类的属性私有化，同时提供公共的方法获取和设置属性的值。
2. 不对外暴露的私有化方法。
3. 单例模式中构造器私有化。
4. .....

### 权限修饰符

| 修饰符    | 类内部 | 同一个包 | 不同包的子类 | 同一个工程 |
| --------- | ------ | -------- | ------------ | ---------- |
| private   | Yes    |          |              |            |
| (缺省)    | Yes    | Yes      |              |            |
| protected | Yes    | Yes      | Yes          |            |
| public    | Yes    | Yes      | Yes          | Yes        |

若想跨工程调用，则必须拷贝代码或者导入jar包。

4种权限修饰符都可以用来修饰类的内部结构：属性、方法、构造器、内部类。**修饰类只能缺省或public**

### 构造器（构造方法）

作用：1. 创建对象     2. 初始化对象的属性

定义构造器的格式：权限修饰符	类名（形参列表）{ }

构造器可以重载，一旦显式地定义了类的构造器，系统就不再提供默认的空参构造器。一个类中，至少会有一个构造器（人为定义或默认）。

默认构造器的权限和类权限相同。

### 属性赋值的先后顺序

①默认初始化

②显式初始化

③构造器初始化

④通过"对象.方法" 或 "对象.属性" 赋值

```java
public class UserTest {
    public static void main(String[] args) {
        User u = new User();
        System.out.println(u.age); // 1		说明显式初始化在默认初始化之后
        User u1 = new User(2);
        System.out.println(u1.age); // 2	说明构造器初始化在显式初始化之后
        u1.setAge(3);
        System.out.println(u1.age); // 3	说明setter方法赋值在构造器初始化之后
    }
}
class User {
    String name;
    int age = 1;
    public User() {
        
    }
    public User(int age) {
        this.age = age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
```

### JavaBean

JavaBean是一种Java语言写成的可重用组件。

JavaBean是指符合以下标准的Java类：

> 类是公共的
>
> 有一个无参的公共的构造器
>
> 有属性，且有对应的get、set方法

### this

1. this可以用来修饰或调用：属性、方法、构造器。

2. `this.属性`或`this.方法`时，this理解为"当前对象" 或 "当前正在创建的对象"(构造器中)。

3. this调用构造器，`this();`表示调用无参构造器，`this(参数类型 参数名);`表示调用有参构造器。**只能调用其他构造器，不能形成递归调用（闭环）**。

   `this(形参列表);`必须声明在第一行，所有一个构造器只能使用一次`this(形参列表);`

## 继承

### 继承性的好处

① 减少了代码的冗余，提高了代码的复用性。

② 便于功能的拓展。

③ 为多态性的使用提供了前提。

### 继承的格式

class A extends B { }

A：子类、派生类、subclass

B：父类、超类、基类、superclass

一旦子类A继承父类B之后，子类A就获取了父类B中声明的所有属性和方法。**私有的结构也会继承只是不能直接访问**。

extends：延展、扩展。子类的功能一般都比父类更强大。

### Java中关于继承性的规定

1. 一个类可以被多个子类继承。
2. 单继承性：一个类只能有一个父类。
3. 子父类是相对的概念。可以有多层继承。C继承B，B继承A，称B是C的直接父类，A是C的间接父类。
4. 子类继承父类之后，就获取了直接父类和所有间接父类中声明的属性和方法。
5. 所有Java类（除`java.lang.Object`类之外）都直接或间接地继承于`java.lang.Object`类。

### 方法的重写（override / overwrite）

1. 定义：子类继承父类以后，可以对父类中同名同参数的方法，进行覆盖操作。

2. 应用：重写以后，当创建子类对象以后，通过子类对象调用父类中的同名同参数方法时，实际执行的是子类重写父类的方法。

3. 规定：

   ​			方法的声明：  `权限修饰符`	`返回值类型`	`方法名`（`形参列表`）	`throws`	`异常的类型` {

   ​											// 方法体

   ​									}

   ​			约定俗成：子类中的叫重写的方法，父类中的叫被重写的方法。

   ​	① 子类重写的方法的方法名和形参列表与父类被重写的方法的方法名和形参列表相同。

   ​	② 子类重写的方法的权限修饰符不小于父类被重写的方法的权限修饰符。

   ​			> 特殊情况：子类不能重写父类中声明为private权限的方法。

   ​	③ 返回值类型：

   ​			> 父类被重写的方法的返回值类型是void，则子类重写的方法的返回值类型只能是void。

   ​			> 父类被重写的方法的返回值类型是A类型，则子类重写的方法的返回值类型可以是A类或A类的子类。

   ​			> 父类被重写的方法的返回值类型是基本数据类型，则子类重写的方法的返回值类型必须是相同的基本数据类型。

   ​	④ **子类重写的方法抛出的异常类型不大于父类被重写的方法抛出的异常类型**。

   ​	⑤ **子类和父类中同名同参数的方法要么都声明为非static的（是重写），要么都声明为static的（不是重写）**。

   ```java
   public class Test {
       public static void main(String[] args) {
           /**
           * 结论：
           * 静态方法可以被继承，但是不能被覆盖，即不能重写。
           * */
           Son.staticMethod(); // 运行结果：Father staticMethod
       }
   }
   class Father {
       public static void staticMethod() {
           System.out.println("Father staticMethod");
       }
   }
   class Son extends Father {
   }
   ```

   ```java
   public class Test {
       public static void main(String[] args) {
           Father.staticMethod(); // 运行结果：Father staticMethod
           /**
            * 结论：
            * 类执行了自己申明的静态方法。
            * 该子类实际上只是将父类中的同名静态方法进行了隐藏，而非重写。
            * */
           Son.staticMethod(); // 运行结果：Son staticMethod
           Father father = new Son();
           /**
            * 结论：
            * 父类引用指向子类对象时，只会调用父类的静态方法。
            * 父类和子类中含有的其实是两个没有关系的方法，它们的行为也并不具有多态性。
            * */
           father.staticMethod(); // 运行结果：Father staticMethod
       }
   }
   class Father {
       public static void staticMethod() {
           System.out.println("Father staticMethod");
       }
   }
   class Son extends Father {
       public static void staticMethod() {
           System.out.println("Son staticMethod");
       }
   }
   // 1. 在Java中静态方法可以被继承，但是不能被覆盖，即不能重写。
   // 2. 如果子类中也含有一个返回类型、方法名、参数列表均与之相同的静态方法，那么该子类实际上只是将父类中的该同名方法进行了隐藏，而非重写。
   // 3. 父类引用指向子类对象时，只会调用父类的静态方法。所以，它们的行为也并不具有多态性。
   // 4. 我们应该直接使用类名来访问静态方法，而不要使用对象引用来访问。
   ```

### super



## 多态
