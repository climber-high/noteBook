### Java基本类型

## 变量声明及初始化
```
变量：就是指代在内存中开辟的存储空间，用于存放运算过程中需要用到的数据
int a=5;
int b=6;
int c=a+b;
```

>1.声明

	一条语句中声明多个同类型变量
	int a=1,b=1;
	//声明了两个整形变量，分别赋值为1和2。

	int c,d=3;
	//声明了两个整形变量,d赋初值为3，c没有赋初值


>2.初始化 :第一次赋值

	变量类型 变量名称 = 初始值
	public static void main(String [] args){
		int sum = 0;
		int a = 5;
		int b = 6;
		sum = a + b;
		System.out.println(sum);
	}
	

>3.使用

	1.对于变量的使用就是对它所存的那个数的使用
	int a = 5;  声明整形变量a并赋值为5
	
	int b =a+10; 取出a的值5，加10后，再赋值给变量b
	
	2.变量的使用必须与数据类型匹配
	int a=25;  int必须赋值整数
	
	3.变量在用之前必须声明并初始化
	

>4.命名：Java中的变量的命名必须符合Java标识符的规则

	1）可以以字母、数字、“_”和“$”符组成；
	
	2）首字符不能以数字开头；
	
	3）中文可以作为变量名，但不提倡使用；
	
	4）Java大小写敏感，命名变量时需要注意；
	
	5）不能使用Java保留字（一些Java语言规定好的，有特殊含义的字符），如：int、if、for、break等。
	

## 8种基本数据类型
*int类型*(整数型) ，*long类型*(长整型)，*double类型*(双精度浮点类型)，*boolean类型*(布尔型)，*char类型*(字符型)。最常用5种数据类型

*byte* (字节类型)，*float* 浮点类型（单精度,作兼容），*short*（ 短整型,作兼容）3种不常用


>1.整数类型：**int**,**long**,short,byte(4种)

>2.浮点类型：**double**,float(2种)

>3.**char**

>4.**boolean**


| 类型名称 | 字节空间 | 最大表达范围 |
|:-----:|:-----:|:-----:|
| int(整型) | 4个字节 | -21多个亿到21多个亿 2147483647   -2147483648|
| long(长整型) | 8个字节 | 很大很大 |
| double(双精度浮点型) | 8个字节 | 很大很大  有效数字16位，包括点|
| boolean(布尔型) | 1个字节 |  |
| char(字符型) | 2个字节 |  |
| short(短整型) | 2个字节 | (很少使用) |
| float(单精度浮点类型) | 4个字节 | (很少使用)有效数字8位，包括点 ，超出会四舍五入|
| byte(字节类型) | 1个字节 | (很少使用) |

## int(整型)
```
1.整数直接量默认为int类型，但不能超范围，超范围则编译错误
2.两个整数相除，结果还是整数，小数位无条件舍弃
3.整数运算时，超范围则发生溢出，溢出是需要避免的

System.out.println(5/2);    2
System.out.println(2/5);    0
System.out.println(5/2.0);  2.5
System.out.println(5.0/2);  2.5

int a=2147483647;
a=a+1;
System.out.println(a);
int类型最大值加1，会变成最小值-2147483648。

```

## long(长整型)
```
1.如果要表示long直接量，需要以L或l结尾
long a=1000000000*2*10L;  
System.out.println(a);  200亿

long a=1000000000*3*10L;  
System.out.println(a);  不是300亿（溢出）

long a=1000000000L*3*10;  
System.out.println(a);  300亿

2.如果运算时有可能溢出，那么L加在第一个数的后面

3.System.currentTimeMillis()用于获取1970年一月一号到现在的毫秒数

long time=System.currentTimeMillis();
System.out.println(time);

4.比long大的**非基本数据类型BigInteger**

```

## double(双精度浮点型)
```
1.浮点数直接量默认是double类型,若想表示float，需在**数字后加F和f**

2.double运算时有可能会出现舍入**误差**,精确运算的场合,不能使用double和float,要用**非基本数据类型BigDecimal**

double m=3.0,n=2.9;
System.out.println(m-n);
结果0.1会产生误差0.10000000000000009
```

## boolean(布尔型)
```
只能赋值为true和false

boolean b=true;  
true为布尔型的直接量
```

## char(字符型)
```
1.采用Unicode编码格式，一个字符对应一个码，表现的形式是字符char,实质上是码int
ASCII码('a'--97 'A'--65 '0'--48)
2.字符必须放在单引号中，只能有一个
3.特殊符号需要通过\来转译

char c='f';  声明字符型变量c并赋值为f
```


## float(单精度浮点类型)
```
float直接量需在数字后面加F或f
float a=3.14F;
```


## 基本数据类型转换(两种方式)
```
基本数据类型从小到大依次为
byte → short → int → long → float → double  
				↑
		char → 

1.自动类型转换：从小类型到大类型（隐式类型转换）
2.强制类型转换：从大类型到小类型


int a=5;
long b=a; //自动类型转换

int c=(int)b; //强制类型转换

long d=5;  //自动类型转换
double e=5; //自动类型转换

long f=10000000000L;
int g=(int)f;  //强制类型转换有可能溢出

double h=56.789123;
int i=int(h);   //强制类型转换有可能丢失精度

两点规则：
1.整数直接量可以直接赋值给byte,short,char，但不能超出范围
byte b1=5;

2.byte,short,char型数据参与运算时，先一律转换为int再运算

byte b1=5;
byte b2=6;
byte b3=(byte)(b1+b2);

System.out.println('2'+'2');  //输出结果为100
```
