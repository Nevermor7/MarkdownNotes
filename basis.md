# 计算机基础

## 原码反码补码

**计算机存储的都是补码，且最高位(最左边)为符号位，0正1负**，原码和反码都只是为了计算补码而提出的概念

正数：原码反码补码三码合一都相同

负数：反码 = 除符号位外原码取反（0变1，1变0），补码 = 反码 + 1；

**为什么1个byte表示十进制数的范围是-128~127？**

由于最高一位存储符号位，所以剩下7位来表示数值大小，能从0表示到127

所以能从-127表示到127，但由于+0和-0都代表0，重复了，所以，多出一个位子，放到负数，让-0代表-128。

所以byte的范围是-128~127。**byte类型里面-128和-0的原码补码都相同**

**已知负数补码只需对其各位取反加一即可得到原码。**

```
-128 = 1000 0000(补) -> 0111 1111(反) -> 1000 0000(原)
```



# 进制转换

## 二进制转十进制

如果是正数，符号位为0，则补码=原码，直接计算转十进制

```
00111011 -> 2^5 + 2^4 + 2^3 + 2^1 + 2^0 = 59
```

如果是负数，符号位为1，则要补码 - 1算出反码，再取反计算原码，最后由原码计算转十进制

```
10111011 -> 10111010 -> 11000101 -> -(2^6 + 2^2 + 2^0) = -69
```

**经典问题：int转byte**

```java
int i = 128; //底层存储为0000 0000 0000 0000 0000 0000 1000 0000
byte b = (byte) i; //强转后只保留最低的8位，即1000 0000(补) = -128

System.out.println(b); //-128
```

## 二进制转八进制

**从最低位起，三位并作一位**

```
00111011 -> 073（为了和十进制区分，八进制日常表示以0开头）
```

## 二进制转十六进制

**从最低位起，四位并作一位**

```
00111011 -> 0x3B（十六进制0x开头，10-15 用 A-F 代替）
```

## 十进制转二进制

**除二取余的逆**

```
13 ÷ 2 = 6 余 1
 6 ÷ 2 = 3 余 0
 3 ÷ 2 = 1 余 1
 1 ÷ 2 = 0 余 1
十进制  13 = 二进制原码 0000 1101 =  二进制补码 0000 1101
十进制 -13 = 二进制原码 1000 1101 -> 二进制反码 1111 0010 -> 二进制补码 1111 0011
```

**十进制负数转二进制**

```
-32
第一步：32（10）=00100000（2）
第二步：求反：11011111
第三步：加1:11100000
所以-32（10）=11100000（2）
```

拓展：十进制转N进制，除N取余的逆

# 变量

- byte
  
  - 1 byte = 8 bit  = 1 符号位 + 7 数值位，能表示2^8个数字，取值范围-128 ~ 127
  
- short
  
  - 1 short = 2 byte，能表示2^16个数字
  
- int
  
  - 1 int = 4 byte，21亿
  
- long
  
  - 1 long = 8 byte
  
- float
  
  - 1 float = 4 byte = 1位符号位 + 8位阶码 + 23位尾数 = 32位，float只占4个字节但是表示的数范围比占8个字节的long还要大，
  
- double
  
  - 1 double = 8 byte = 1位符号位 + 11位阶码 + 52位尾数 = 64位，
  
- char

  - 1 char = 2 byte，
  - char**有且只有**一个字符，不能把空字符赋值给char类型，与String区分开来

  ```java
  char c = ''; //语法报错
  ```

  - 将整数赋值给字符变量，实际赋的值是该整数对应的Unicode字符表中的字符，但是最大只能到65535（2Byte能表示的最大数值）

    **原理：java中int可以自动转成byte，short，char但是不能超出各自的取值范围**

  ```java
  char c1 = 98;
  System.out.println(c1);//b
  char c2 = 34433;
  System.out.println(c2);//蚁
  
  char c3 = 65535;//正常运行
  char c4 = 65536;//语法报错
  ```

- boolean

所有整数数值默认为int类型，所有小数数值默认为double类型。

long类型赋值时应该在数值后面加L，float类型赋值时应该在数值后面加F。浮点数一般用double



# 类型转换

## 自动类型转换（提升）

取值范围小的数据类型可以自动转换为取值范围大的数据类型

**运算过程中byte,short,char型的值将被自动提升为int型**

```java
byte b = 5;
short s1 = 3;
short s2 = b + s1; //语法报错：不能将int值赋给short型变量
int i = b + s1; //不报错

char c1 = 'a';
char c2 = 'b';
System.out.println(c1 + c2); //195
```

## 强制类型转换

``` java
double d = 12.9;
int i = (int) d; //小数部分直接截断而不是四舍五入
System.out.println(i); //12
```

# String

String可以与八种基本数据类型进行拼接运算，结果为String。

```java
char c = 'a';
int num = 10;
String str = "hello";
System.out.println(c + num + str); //107hello
System.out.println(c + str + num); //ahello10
System.out.println(c + (num + str)); //a10hello
System.out.println((c + num) + str); //107hello
System.out.println(str + num + c); //hello10a
```

# 运算符

## 除 /

```java
int num1 = 12;
int num2 = 5;
int result = num1 / num2;
System.out.println(result);//2

int result2 = num1 / num2 * num2;
System.out.println(result2);//10

double result3 = num1 / num2;
System.out.println(result3);//2.0 等价于double result3 = (double) 2;

double result4 = num1 / num2 + 0.0;
System.out.println(result4);//2.0

double result5 = num1 / (num2 + 0.0);
System.out.println(result5);//2.4

double result6 = (double)num1 / num2;
System.out.println(result6);//2.4
```

## 取模 %



**结果的符号和被模数相同**

```java
int m1 = 12;
int n1 = 5;
System.out.println(m1 % n1);//2
int m2 = -12;
int n2 = 5;
System.out.println(m2 % n2);//-2
int m3 = 12;
int n3 = -5;
System.out.println(m3 % n3);//2
int m4 = -12;
int n4 = -5;
System.out.println(m4 % n4);//-2
```

## 自增1 ++

前++：先自增1，再用自增完的结果参与运算

后++：前取自增前的数值参与运算，运算完后自增1

**自增运算不改变变量类型**

```java
byte b = 127;
b++;
System.out.println(b); //-128
//本质：128转byte
```

## 赋值运算符

= += -= *= /= %=

```java
int i = 1;
i *= 0.1;
System.out.println(i); // 0
```

## 位运算符

![image-20210119154249872](D:\Coding\notes\bitOperator.png)



交换两个变量的值

![image-20210119155847982](D:\Coding\notes\exchange.png)

# Switch case

```java
int number = 0;
switch(number){
	case 0:
		执行语句;
		break;
	case 1:
		执行语句;
		break;
	default:
		执行语句;
}
```



break关键字：进入当前分支执行结束后跳出switch，若不加break则执行结束后继续顺序执行下一个case中的语句，且无需匹配。

default可有可无，类似于if-else结构的else，一般在所有case之后。

switch case支持6种数据类型：byte	short	char	int	枚举类型（JDK1.5）	String（JDK1.7）。

## 多个case可以合并

```java
int score = 78;
switch(score / 10){
    case 0:
    case 1:
    case 2:
    case 3:
    case 4:
    case 5:
        System.out.println("不及格");
        break;
    case 6:
    case 7:
    case 8:
    case 9:
    case 10:
        System.out.println("及格");
        break;
}
```

# for循环


