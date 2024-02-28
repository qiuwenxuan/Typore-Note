

# -----java入门-----

换行“\n”

换格"\t"

## 一,语法基础

### 1.JDK与JRE

`JDK=JRE(Java系统类库+JVM)+编译运行等开发工具`

| ![img](https://img-blog.csdn.net/20160704103050112?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center) |
| ------------------------------------------------------------ |

### 2.第一个java程序

1. 步骤一：新建HellloWorld.txt并重命名为HelloWorld.java；
2. 步骤二:（编辑）：在HelloWorld.java中编辑代码；
3. 步骤三：打开cmd命令行窗口，跳转到HelloWorld.java所在位置；
4. 步骤四（编译）：命令行输入javac HelloWorld.java(文件名,需要后缀)回车，会发现在HelloWorld.java所在文件夹出现了一个HelloWorld.class字节码文件；
5. 步骤五（运行）：命令行输入java HelloWorld(类名)回车，成功打印出“HelloWorld!”，运行成功。

```java
  public class HelloWorld{
    	public static void main(String[] args){
    		System.out.println("HelloWorld!");
    	}
    }
```

| ![image-20211118202426953](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211118202426953.png) |
| ------------------------------------------------------------ |

### 3.java标识符命名方法

1)java标识符命名规则

1. 必须由字母、数字、下划线及美元符号组成；
2. 不能以数字开头；
3. 不能与关键字冲突；
4. 不能和java类库的类名冲突；
5. 应该使用有意义的名称。

2)java命名规范

1. 类名,项目名遵从大驼峰原则
2. 方法名,属性名,参数名遵从小驼峰原则
3. 包名全部小写,并用"."隔开
4. 常量名字母全部大写 例: MAXSIZE=0；

### 4.自动与强制类型转换

1.自动类型转换

1）由字节小的往字节大的转（向上转型：小变大）

​	例：int a=10;

​			long b=a;

2）byte short int 之间两两转int

3）整数往小数转

2.强制类型转换

1）由字节大的往字节小的转（向下转型：大变小）

​	例：long a=10.0;

​			int b=(int) a;（注意：由浮点型转换为整数型，并不会向小数位四舍五入，而是直接取整数部分，因此浮点型转向下转型容易丢失精度）

### java基本数据类型

java基本数据类型：整数类型（byte、short、int、long）,浮点类型（float、double），（字符类型）char，string，（布尔类型）boolean

![image-20240227154114438](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240227154115.png)

`基本类型的字节空间和应用场景`

| 类型名称 | 字节空间 | 应用场景               | 类型范围                                                  | 默认值                                               |
| -------- | -------- | ---------------------- | --------------------------------------------------------- | ---------------------------------------------------- |
| byte     | 1Byte    | 字节数据               | -2^7~2^7-1<br />-128~127                                  | 0                                                    |
| short    | 2Byte    | 短整数                 | -2^15~2^15-1<br />-32768~32767                            | 0                                                    |
| int      | 4Byte    | 普通整数               | -2^-31~2^31-1<br />-2147483648~2147483647                 | 0                                                    |
| long     | 8Byte    | 长整数                 | -2^63~2^63-1<br />-9223372036854775808~223372036854775807 | 0                                                    |
| float    | 4Byte    | 浮点数                 | -2^-31~2^31-1<br />-2147483648~2147483647                 | 0.0f                                                 |
| double   | 8Byte    | 双精度浮点数           | -2^63~2^63-1<br />-9223372036854775808~223372036854775807 | 0.0d                                                 |
| char     | 2Byte    | 单字符                 | -2^15~2^15-1<br />-32768~32767                            | '\u0000'（即 Unicode 码值为 0 的字符，也就是空字符） |
| boolean  | 1Byte    | 逻辑变量（true/false） | -2^7~2^7-1<br />-128~127                                  | false                                                |

`注意事项：`

int类型:

1.int类型，整数的默认类型为int类型，如果直接写出的整数超过了int的表达范围，编译报错；

如 int one = 80000000000000000000000;//超出范围，编译错误

除了常见的十进制之外，整数也常常写为16进制(0X/0x开头)或8进制(0开头)的形式

```java
int two = 529;
int three = 0x347a;
int four = 030;
```

2.两个整数相除，会舍弃小数部分，使结果也是整数

```java
int date1 = 49;
int date2 = 79;

int result = date2 / date1;
System.out.println(result);

result = 1
```

3.整数运算的溢出：两个整数进行运算时，其结果可能会超过整数的范围而溢出。正数过大而产生的溢出，结果为负值；负整数过大而产生的溢出，结果为正数

```java
int number=2147483647;

int score=-2147483648;

number= number+1;//结果为-2147483648

score = score -1; //结果为2147483647
```



long类型:

如果使用long类型,需要用L或l结尾

```java
long gg = 44;//错误

long gg = 44L;//正确
```



浮点类型:

浮点类型包括float和double,默认浮点类型为double,声明float类型需要强转,如:

```java
float a = 1.3; // 编译报错,需要强转
float a = 1.3f;// 正确
```

但是整数放在float变量中不需要强转

```java
float a = -3;
float b = 0x0123
    
int c = 'A'; // char和int类型可以相互转换
char d = 65;
System.out.println(c);
System.out.println(d);
```

`(可以理解成,字节大的向字节小的类型转换,需要强转声明,字节相同的类型可以直接转)`

float默认值为0.0f

double默认值为0.0d

double类型的精度值为float类型的两倍,一般场合使用double类型



字符类型:

char字符变量赋值,有以下三种情况:

```java
char a = 'A' // 变量实际存储的是该字符的Unicode编码,一个char变量只能存储一个字符
char b = 705 // 范围在0~65535之间的整数,表示该整数值对应的Unicode字符
char c = '\u0031' // Unicode字符的16进制形式
```

### 特殊的字符:`转义字符`

| 字符 | 代表的含义   |
| ---- | ------------ |
| \n   | 换行 (0x0a)  |
| \r   | 回车 (0x0d)  |
| \f   | 换页符(0x0c) |
| \b   | 退格 (0x08)  |
| \s   | 空格 (0x20)  |
| \t   | 制表符       |
| \"   | 双引号       |
| \'   | 单引号       |
| \\   | 反斜杠       |

### 基本类型之间的相互转换

![image-20240227220803530](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240227220807.png)

java当中的基本类型之间的转换可分为`自动转换`和`强制转换`

1. 自动(隐式)类型转换
   - 有java编译器自动完成,通常发生在将一个小范围数据类型转换为一个大范围数据类型.如int转换为long类型的变量,float转化后为double类型的变量
2. 强制类型转换
   - 需要显示的使用强制类型转换符`()`,即将较大的范围的数据类型转为较小的范围的数据类型.例如:将一个double类型转为float,将long转为int类型

```java
自动类型转换
int a = 10;
float b = 10.1f;

long c = a;
double d = b;

System.out.println(c + " " + d); // 10 10.100000381469727

强制类型转换            
long a = 10;
double b = 10.2;

int c = (int)a;
float d = (float) b; // 10 10.2
```





### 5.特殊自增自减运算符

1）a++ = a（先带入运算再+1）

2)++a = a+1(先+1再带入·运算)

3)a+=b  --> a=a+b;

### 6.选择程序结构

1)if 选择结构

```java
int x = 10;
if (x > 0) {
    System.out.println("x是正数");
} else if (x < 0) {
    System.out.println("x是负数");
} else {
    System.out.println("x是0");
}
```



2)switch选择结构

格式:

```java
int day = 3;
        switch (day) {
            case 1:
                System.out.println("星期一");
                break;
            case 2:
                System.out.println("星期二");
                break;
            case 3:
                System.out.println("星期三");
                break;
            case 4:
                System.out.println("星期四");
                break;
            case 5:
                System.out.println("星期五");
                break;
            case 6:
                System.out.println("星期六");
                break;
            case 7:
                System.out.println("星期日");
                break;
            default:
                System.out.println("未知");
        }
```

关键字：==switch  case  break  default==



### 7.循环程序结构

（循环结构的四个要素：==初始值、判断、具体循环操作、条件迭代==）

循环结构=（循环条件+循环操作）

1)while循环

​	先执行,再判断;

2)do-while循环

​	先判断,满足条件了再执行;

3)for循环

基本格式：

```java
for(①条件初始值;②条件判断;④条件迭代){
	③具体循环操作;
}
for(int i = 0; i >= 10, i++){
    sum+=i;
}
```

执行顺序：1--2--3--4--2--3--4--2--3--4

-->例:java代码(九九乘法表)

```java
for (int i = 1; i <= 9; i++) {
    for (int j = 1; j <= i; j++) {
        System.out.print(j + "*" + i + "=" + i * j + " ");
    }
    System.out.println();
}

1*1=1 
1*2=2 2*2=4 
1*3=3 2*3=6 3*3=9 
1*4=4 2*4=8 3*4=12 4*4=16 
1*5=5 2*5=10 3*5=15 4*5=20 5*5=25 
1*6=6 2*6=12 3*6=18 4*6=24 5*6=30 6*6=36 
1*7=7 2*7=14 3*7=21 4*7=28 5*7=35 6*7=42 7*7=49 
1*8=8 2*8=16 3*8=24 4*8=32 5*8=40 6*8=48 7*8=56 8*8=64 
1*9=9 2*9=18 3*9=27 4*9=36 5*9=45 6*9=54 7*9=63 8*9=72 9*9=81 
```



4)break和contiue的区别

- break是跳出该循环；
- 而continue是跳出本次循环，转而实行下一个循环,不破坏循环结构;

```java
continue应用
for (int i = 0; i < 1000; i++) {
    if (i % 2 == 0) {
        continue;
    }
    System.out.println(i);
}
```

使用场合：①break常用于switch结构和循环当中；

​				②continue一般用于循环结构中；

### 8.方法

​	1)方法的定义格式

```java
public static 返回类型 方法名(){
		方法体;
}
```

​	2)static方法的作用域

​	static方法定义在public class之内，main方法之外，main方法有且可以调用static方法.

​	定义在类当中的方法称为类方法,类方法与方法有许多异同。

​	3)带返回值的方法定义格式

```java
public static 返回值类型 变量名(形参){//相比于类方法多了一个static
	方法体;
	return 返回值;
}
```

​	4)方法注意事项

- 方法必须定义在class 内，不可以单独定义在类外面 
- 含void返回值类型的方法内return可以省略，且return后面不接任何数值 (return;表示终止程序继续往下运行)
- 同一个类当中方法不可以相互嵌套，但可以相互调用（如：递归方法）
- 方法可以重载

例：

```java
public class MethodDemo01 {
    public static void main(String[] args) {
        System.out.println(compare(10,20));
    }
    public static boolean compare(int a,int b){
        return a==b;
    }
    public static boolean compare(double a,double b){
        return  a==b;
    }
    public static boolean compare(byte a,byte b){
        return a==b;
    }
    public static boolean compare(long a,long b){
        return a==b;
    }
}

```

5)方法的参数传递机制

①基本类型参数传递

对于基本类型的参数，形式参数的改变，不影响实际参数的值。

```java
public class MethodDemo02 {
    public static void main(String[] args) {
        int number = 10;
        change(number);//number是static栈空间开辟的变量,当传递给change()时,并不会传递number本身的引用或指针,而是传递一个number值的拷贝
        System.out.println(number);
    }

    public static void change(int number) {//由于change方法接收到的参数是main方法number的拷贝,而该方法只是修改了number的拷贝的值,并不影响main方法当中的number值
        number = 20;
    }
}
运行结果：
    10
```

②引用类型参数传递

对于引用类型的参数，形式参数的改变，影响实际参数的值(数组也是引用参数类型)

```java
public class Demo01 {
    public static void main(String[] args) {
        int[] arr = {10, 20, 30};
        System.out.println("调用change方法前："+arr[1]);
        change(arr);//main函数传递给change()方法一个数组arr,由于数组是一个引用数据类型,会在堆空间内开辟一个内存用于存放数据,当arr传递给change()方法时,是传递了一份引用类型的拷贝,但是拷贝指向的堆空间的数组是同一个,当修改传入的拷贝时,会修改到数组所在的堆空间,因此数组内容也会永久改变.
        System.out.println("调用change方法后："+arr[1]);
    }
public static void change(int[] arr){
        arr[1] = 200;
    }
}
运行结果：
    调用change方法前：20
	调用change方法后：200
```

两种方法参数类型的传递机制

​		方法当中的基本参数类型的定义都直接在栈内存当中产生，因为不存在指针由栈内存指向堆内存，所以与堆内存不同的是，栈中如果定义两个变量相等（int a=10; int b = a;）是直接由栈内存开辟两个栈空间,它们的值相等，内存地址不相同，除此之外没有任何联系。而方法当中对于引用类型的定义（如int[] a = new int[]{10};int[]b = a;），则是在栈内存开辟两个不同的变量名空间，由其指向相同的堆内存空间，a和b两者的内存地址相同。

​	无论是方法还是类方法，当方法被调用时都是在栈内产生，一旦方法结束及被释放。

### 9.形参和实参

- 形参:	`方法定义中的参数`
  				等同于变量定义格式。例如： public static void isEvenNumber(int number)
- 实参： `方法调用中的参数`
  				等同于使用变量或常量。例如：isEvenNumber(10)

```java
public class FangfaDemo01 {
    public static void main(String[] args) {
        int number = 10;
        isEvenNumber(10);
        isEvenNumber(number);
        //实参，方法调用时的参数，可以为具体的数字，也可以为有实际意义的变量。
    }

    public static void isEvenNumber(int number)//形参，即方法定义当中的参数 一般由数据类型+变量名组成
    {
        if (number % 2 == 0) {
            System.out.println(true);
        } else {
            System.out.println(false);
        }
    }
}
```

### 10.Debug概述

Debug:是供程序员使用的调试工具，它可以用于==查看程序的执行流程==，也可以用于==追踪程序执行过程来调试程序==。

- 步骤1：加断点
- 步骤2：运行Debug
- 步骤3：下一步 F7
- 步骤4：结束Debug 点击stop红点

### 11.变量赋值问题

默认值如下：

Boolean      false

Char           '\u0000'(null)

byte            (byte)0

short           (short)0

int               0

long            0L

float            0.0f

double        0.0d

局部变量声明之后，Java虚拟机不会自动给它初始化为默认值，因此局部变量的使用必须先经过显式的初始化。

但是需要声明的是：对于接收一个确认值的局部变量可以不初始化，参与运算和直接输出等其它情况的局部变量需要初始化。

通过下面这个测试可以看到JVM对哪些数据初始化，哪写数据不初始化：

```java
public class TestStatic {
static int x; //类的成员变量，JVM负责初始化

static int method()

{
int y=0;  //此处必须自己初始化，它不属于类成员变量，是个method的局部变量，JVM不负责初始化

return y;

}

public static void main(String[] args) {
TestStatic as=new TestStatic();

int z=0;  //此处必须自己初始化，它不属于类成员变量，是个主函数里的局部变量，JVM不负责初始化

int aa=3; //此处aa参与了运算，所以必须初始化

aa=aa+2;

int a=1,b=2,max; //max被定义

max=a>b?2:1; // 由于a和b都是一个被赋予值的变量,因此max一定能接收到一个确认的值,因此max可以不用初始化

System.out.println(max); //1

System.out.println(aa); //5

System.out.println("z="+z); //z=0

System.out.println("x="+as.x); //x=0

System.out.println("y="+as.method()); //y=0

}

}
```

总结为一句话便是：

类里定义的数据成员称为属性，属性可不赋初值，若不赋初值则JAVA会按上表为其添加默认值；

静态方法里定义的数据成员称为变量，变量在参与运算之前必须赋初值。（main方法也是静态方法方法）

### 12.可变参数

语法:

```java
访问修饰符 返回值类型 方法名称(数据类型...数组名){
	方法体;
}
```

```java
public class test {
  public static void main(String[] args) {
    System.out.println(sum1(1, 2)); 
    System.out.println(sum2(1, 2, 3)); 
    System.out.println(sum3(1, 2, 3, 4)); 
  }

  // 正常方法求和
  public static int sum1(int a, int b, int c) {
    return a + b + c;
  }

  // 可变参数求和
  public static int sum2(int... a) {
    int sum = 0;
    for (int i : a) { //int[] a 
      sum += i;
    }
    return sum;
  }

  // 可变参数求和，如果存在可变参数与非可变参数，可变参数需要放到后面，不能放在前面。
  // public static int sum3(int... a, int b) // 错误写法
  public static int sum3(int b, int... a) {//本质上是创建了一个数组a,将所以参数存储在a当中.
    int sum = 0;
    for (int i : a) {//遍历数组
      sum += i;
    }
    return sum + b;
  }
}
```

​	1.当使用可变参数的时候，实际上是先创建了一个数组，该数组的大小就是可变参数的个数，然后将参数放入数组当中，再将数组传递给被调用的方法.

​	2.可变参数重载条件下必须放在f最后一个方法的位置,不能放在前面,使用可变参数要小心;

## 二,数组

### 1.数组的声明

--什么是数组：存储相同数据类型的小组容器，本质上就是一块连续的内存空间。根据我们之前java的数据类型可知，数组属于引用数据类型。

--怎么使用？

步骤：①声明一个数组--②开辟内存空间--③存储数据<数组元素赋值>--④引用数组元素

```java
数据类型[] 数组名称;//推荐更直观显示这是一个数组变量 int[] arr;
数据类型  数组名称[];  // int arr[];
```

开辟内存空间：数组名 = new 数据类型[元素个数];   数组元素下标从0开始

赋值：数组名[下标] = 值；

引用：System.out.pritnln(数组名[下标]);

```java
int[][] arr = new int[4][3];
for (int i = 0; i < arr.length; i++) {
    for (int j = 0; j < arr[0].length; j++) {
        System.out.print(arr[i][j] + "\s");
    }
    System.out.println();
}
int row = arr.length;
int col = arr[0].length;

System.out.println("row=" + row + ",col=" + col);

0 0 0 
0 0 0 
0 0 0 
0 0 0 
row=4,col=3
```

在 java 中，其实只有一维数组，而二维数组是在一维数组中嵌套实现的。比如 int[][] a = {{},{},{},{}} 

要取行数和某一行的列数可以 ：

```java
int rowLen = a.length;//数组行数

int colLen = a[0].length;/数组列数

```



### 2.动态数组

==（声明开辟空间与赋值分开进行）==

即想分配多少空间就分配多少空间，动态分配。int[] arr = new int[5]

语法：

```java
数据类型[] 数组名;
数组名 = new 数据类型[元素个数];
数组名[0] = 92;
数组名[1] = 84;
```

```java
public class Text07 {
	//动态使用数组：声明开辟空间与赋值分开完成
	/*语法：	数据类型[] 数组名;
	 		数组名 = new 数据类型[元素个数];
	 		数组名[0] = 92;
	 		数组名[1] = 84;*/
	public static void main(String[] args) {
		//数组使用四个步骤；
		//1--声明数组
		//2--开辟空间-->此处开发了四个元素：score[0...3]
		int scores = new int[4];//动态分配四个内存空间。此处的int[4]表示的是开辟四个数组元素，但实质上开辟数组为int[0]~int[3]
		//3--赋值
		scores[0] = 92;
		scores[1] = 84;
		//4--引用
		System.out.println("第一个数组"+scores[0]);
		System.out.println("第二个数组"+scores[1]);
	}

}
```

### 3.静态数组

==声明开辟空间与赋值同时进行==（更方便）有点像构造方法

语法：数据类型[]  数组名 = new 数据类型[元素个数]；int[] arr = new int[]{1,2,3,4};

```java
	 数组名[下标] = 值；
```

```java
public class Text08 {
	//静态使用数组：声明开辟空间与赋值一起完成
	//语法：数据类型[] 数组名 = new 数据类型[]{值1,值2,值3...};
	public static void main(String[] args) {
		int[] scores = new int[]{92,84,63,68};//此处new int[]{...} []内不能加元素个数，后面有多少个值长度便是多少.且数组元素之间用“,”隔开。
		for(int i = 0;i <= scores.length; i++) {
			System.out.println(scores[i]+"\t");
		}
	}
}
```



动态数组,静态数组的定义

```java
int[][] arr = new int[4][3]; // 动态数组的定义
for (int i = 0; i < arr.length; i++) {
    for (int j = 0; j < arr[0].length; j++) {
        System.out.print(arr[i][j] + "\s");
    }
    System.out.println();
}
int row = arr.length;
int col = arr[0].length;

System.out.println("row=" + row + ",col=" + col);

int[][] arr2 = new int[][]{{1, 2, 3}, {4, 5, 6}, {7, 8, 9}}; // 静态数组的定义
for (int i = 0; i < arr2.length; i++) {
    for (int j = 0; j < arr2[0].length; j++) {
        System.out.print(arr2[i][j] + "\s");
    }
    System.out.println();
}

0 0 0 
0 0 0 
0 0 0 
row=4,col=3
1 2 3 
4 5 6 
7 8 9 

```



#### 4.数组内存解析

`使用new命令会在堆内存当中new出一个堆内存空间`,如果引用数据类型没有使用new而是直接使用=,表示两个引用数据类型直接指向同一个堆内存;

```java
public class ArrayDemo01 {
    public static void main(String[] args) {
        int[] arr = new int[3];
        arr[0] = 100;
        arr[1] = 200;
        arr[2] = 300;
        System.out.println(arr);
        System.out.println(arr[0]);
        System.out.println(arr[1]);
        System.out.println(arr[2]);
        int[] arr2 = arr;//在栈内存当中定义两个不同数组变量arr和arr2，但arr2并没有自己new出一个独立的堆内存空间，而是与arr共用一个内存空间。此时arr2[]内的索引也是arr[]的索引,通过对arr2的索引可以直接访问到arr的堆空间内容。
        arr2[0] = 111;
        arr2[1] = 222;
        arr2[2] = 333;
        System.out.println(arr2);
        System.out.println(arr2[0]);
        System.out.println(arr2[1]);
        System.out.println(arr2[2])；//更改arr2相当于直接访问arr1的堆内存进行永久更改
    }
}
运行结果：
[I@776ec8df//数组内存arr
100
200
300
[I@776ec8df//两数组共用一个内存空间
111
222
333
```

### 4.数组的遍历

​	1)for循环遍历

```java
import java.util.Scanner;

public class Text03 {

	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("请输入5个学生成绩：");
		int[] score = new int[5];
		for(int i = 0; i<=score.length; i++) {
			//注意：此处开辟了五个空间的数组score[5];使用for循环需从score[0]~score[4]依次赋值
			score[i] = scan.nextInt();
		}
		System.out.println("他们的成绩为："+score[0]+" "+score[1]+" "+score[2]+" "+score[3]+" "+score[4]);
		
	}

}
/*

请输入5个学生成绩：
11 22 33 44 55
他们的成绩为：11 22 33 44 55


```

​	2)foreach增强循环遍历+数组扩容

```java
package njpji;

import java.util.Arrays;
import java.util.Scanner;

public class ArrayDemo01 {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		String[] names = new String[] {"jack","rose","mike","kero"};//①直接遍历循环输出
		for(int i = 0; i<names.length; i++) {
			System.out.println(names[i]);
		}
		System.out.println("*******************************");
		for(String name : names) {//②foreach增强循环遍历输出
								//语法：for(数据变量 变量名称：数据集){变量名称}	
			System.out.println(name);
		}
		System.out.println("*******************************");
		int[] scores = new int[4];
		scores[0] = 99;
		scores[1] = 65;
		scores[2] = 77;
		scores[3] = 100;
		scores = Arrays.copyOf(scores, scores.length*2);//③实现数组内存扩容：scores.length变成scores.length*2 及长度变为原来的两倍。
		scores[4] = 66;
		for(int cj : scores) {
		if(cj!=0) {
			System.out.println(cj);
		}
		}
		System.out.println("你还可以输入"+(scores.length-5)+"个数据");
	}

}

```

### 5.二维数组两种遍历

```java
package njpji;

public class ArrayDemo02 {

	public static void main(String[] args) {
		int[][] nums = new int[][] {{1,2,3,4},{5,6,7,8},{9,10,11,12}};//声明一个三行四列的二维数组，静态赋值
		for(int i = 0;i<=2; i++) {//循环打印-1 for循环直接遍历
			for(int j = 0; j<=3; j++) {
				System.out.print(nums[i][j]+" ");
			}
			System.out.println();
		}
		System.out.println("*****************************");
		for(int num[] : nums) {//循环打印-2 二维数组的foreach增强循环遍历
            //可以理解为降维手段，先通过foreach将二维nums[][]降维为一维num[],再经过一次foreach将int num[]降维成int n,再输出n，即将二维数组的每一行看成一维数组的一个值。
			for(int n : num) {
				System.out.print(n+" ");
			}
			System.out.println();//注意输出换行
		}
		
	}

}

/*
1 2 3 4 
5 6 7 8 
9 10 11 12 
*****************************
1 2 3 4 
5 6 7 8 
9 10 11 12 
```

## 三,字符串

### 1.API

API概述:(Applicat Programmin Interfa) 应用程序编程接口

java API:指的是JDK当中提供的各种功能的Java类

这些类将底层的实现封装了起来,我们不需要关心这些类是如何实现的,

只需要学习这些类如何使用即可,我们可以通过帮助文档来学习这些API如何使用.

### 2.String构造方法

String两种方式得到对象的特点:

方法一:直接赋值的方法得到字符串对象

```java
String s1 = "abc";
String s2 = "abc";
System.out.println(s1==s2);
运行结果:
	true;
```

​		

方法二:创建对象的方法来得到字符串对象

```java
char[] chs = {'a','b','c'};
String s1 = new String(chs);
String s2 = new String(chs);
System.out.println(s1==s2);
运行结果:
	false;
```

因为通过new两个不同对象的方法来定义两个字符串对象,此时堆内存开辟了两个数值一样但内存不同的空间,即s1的地址与s2不相同.

```java
package Day01;

public class Demo01 {
//String构造方法:
    public static void main(String[] args) {
        String s1 = new String();
//        public String();创建一个空白字符串,不含有任何内容
        System.out.println("s1:"+s1);
        char[] chs = {'a','b','c'};
        String s2 = new String(chs);
//        public String(char[] chs);根据字符数组的内容,来创建字符串对象
        System.out.println("s2:"+s2);
        byte[] bys = {97,98,99};
        String s3 = new String(bys);
//        public String(byte[] bys);根据字节数组的内容来创建字符串对象
        System.out.println("s3:"+s3);
        String s4 = "abc";
//        String s4 = "abc";直接赋值的方法创建字符串对象.推荐使用对直接赋值的方法
        System.out.println("s4:"+s4);
    }

}
运行结果:
    s1:
    s2:abc
    s3:abc
    s4:abc
```

### 字符串的存储位置

1. `堆内存`:通过关键字new创建的字符串对象时,该字符串对象会被存放在堆内存当中.每次调用new关键字都会在堆内存中创建一个新的字符串对象,即使存在相同的字符串值,也会在堆内存中创建一个新的对象

   ```java
   String str = new String("Hello");
   ```

2. `字符串常量池`:在 Java 6 及之前的版本中，字符串常量池是存放在方法区（Method Area）中的一部分,方法区主要用于存储类信息、常量、静态变量、即时编译器编译后的代码等.在 Java 7 中，随着对永久代的调整，字符串常量池被移到了堆内存中的一部分，即存放在 Java 堆中的一个区域，而不再是方法区的一部分。

   ```java
   String str = "Hello"; // 因此即使使用字符串字面量创建的字符串对象，也可能存放在堆内存中。
   ```



### 3.字符串的遍历

遍历字符串首先得获取字符串当中每一个索引下的字符

​	public char charAt(int index);//返回指定索引的字符

```java
import java.util.Scanner;

public class Demo03 {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        System.out.println("请输入一个字符串:");
        String sc = scan.next();
        for (int i = 0; i <sc.length() ; i++) {
            System.out.println(sc.charAt(i));
            //sc.charAt(int index);返回字符串对应索引的字符值
            //获取字符串长度 sc.length(); 获取数组长度 arr.length;
        }
    }
}
运行结果:
    abcdefg
    a
    b
    c
    d
    e
    f
    g
```

### 4.字符串的拼接

字符串类型+"任意符号" 即可完成字符串的拼接

```java
package Day01;

import java.util.Scanner;

public class Demo04 {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        System.out.println("请输入数组的长度:");
        int i = scan.nextInt();
        int[] arr = new int[i];
        System.out.println("请输入数组的值:");
        for (int j = 0; j <arr.length ; j++) {
            arr[j] = scan.nextInt();
        }
        System.out.println(arrayToString(arr));
    }
    public static String arrayToString(int[] arr){
        String s = "";
        s+="[";
        for (int i = 0; i <arr.length ; i++) {
            if (i == arr.length-1) {
                s+=arr[i];
            }else{
                s+=arr[i]+",";
            }
        }
        s+="]";
        return  s;
    }
}
运行结果:
    请输入数组的长度:
    4
    请输入数组的值:
    11
    22
    33
    44
    [11,22,33,44]


```

## 四,ArrayList(动态数组)

### 1.ArrayList特点

`ArrayList` 是一种实现了动态数组的集合类。它实际上是一个基于数组的动态容器，可以根据需要动态调整其大小。

`ArrayList` 底层实现了 `List` 的数组接口，因此它具有列表的所有特性，如允许重复元素、按索引访问元素等，同时也支持动态增长和缩减容量。

`ArrayList<E>`	类似于可调整大小的数组

`<E>`是一种特殊的数据结构: 泛类

```java
举例:在出现E的地方我们使用引用数据类型替换即可.
	ArrayList<String>,ArrayList<Student>
```

### 2.ArrayList的创建即添加元素

```java
import java.util.ArrayList;

public class Demo01 {
    public static void main(String[] args) {
        ArrayList<String> array = new ArrayList<String>();
        //public ArrayList<>();创建一个空的集合对象
        array.add("Hello");
        array.add("world");
        array.add("java");
        //public boolean add(String a)将指定元素增加到集合的末尾
        array.add("javase");
        //public void add(int index, String a );在指定位置插入指定元素
        //在插入的位置之后的所有元素皆往后移动一位,不可以越界插入.
        array.add(2,"javaee");
        System.out.println(array);
    }
}
运行结果:
	[Hello, world, javaee, java, javase]
//集合输出自动加一个中括号;
```

### 3.集合的其他功能

```java
add -- 增加
    add(E element) //添加元素
    add(int index, E element) //在指定索引添加元素
    addAll(Collection<E> c) //将指定集合当中的所有元素添加到动态数组的末尾
    addAll(int index, Collection<E> c) //将指定集合中的所有元素添加到指定索引位置之后

remove -- 删除
    remove(int index) //移除指定索引的位置的元素
    remove(Object o) //移除指定元素
    removeAll(Collection<E> c) //移除动态数组中所有在指定集合中出现的元素
    clear() //移除动态数组中所有的元素
    
set -- 修改
    set(int index,E element) //修改指定索引的元素为新元素
    
get -- 获取元素
    get(int index) //获取指定索引位置的元素
    
查找元素:
	contains(Object o) //判断动态数组中是否包含指定的元素
    indexOf(Object o) //返回指定元素在动态数组中第一次出现的索引位置
    lastIndexOf(Object o) //返回指定元素在动态数组中最后一次出现的索引位置
        
其他方法:
	isEmpty() //判断动态数组是否为空
    size() //返回动态数组中元素个数
    trimToSize() //将动态数组的容量调整为实际元素的个数,节省空间
    toArray(T[] a) //将动态数组转为指定类型的数组,默认转为普通数组
```



### 4.集合的遍历

```java
import java.util.ArrayList;

public class Demo03 {
    public static void main(String[] args) {
        ArrayList<String> array = new ArrayList<>();
        array.add("刘正风");
        array.add("风清扬");
        array.add("左冷禅");
        for (int i = 0; i < array.size() ; i++) {
            String s = array.get(i);//get方法获取集合当中对应索引的元素
            System.out.println(s);
        }
    }
}

```

### 5.next()与nextline()区别

- next()一定要读取到有效字符后才可以结束输入，对输入有效字符之前遇到的空格键、Tab键或Enter键等结束符，next()方法会自动将其去掉，只有在输入有效字符之后，next()方法才将其后输入的空格键、Tab键或Enter键等视为分隔符或结束符。
- nextLine()方法的结束符只是Enter键，即nextLine()方法返回的是Enter键之前的所有字符。如果一上来就输入Enter会自动跳过输入。（难以把握）

# -----java进阶-----

## 耦合性和内聚性

耦合性是编程中的一个判断代码模块构成质量的属性，不影响已有功能，但影响未来拓展，与之对应的是内聚性。

耦合性：也称块间联系。指软件系统结构中各模块间相互联系紧密程度的一种度量。模块之间联系越紧密，其耦合性就越强，模块的独立性则越差。模块间耦合高低取决于模块间接口的复杂性、调用的方式及传递的信息。

内聚性：又称块内联系。指模块的功能强度的度量，即一个模块内部各个元素彼此结合的紧密程度的度量。若一个模块内各元素（语名之间、程序段之间）联系的越紧密，则它的内聚性就越高。

因此，现代程序讲究==高内聚低耦合==，即将功能内聚在同一模块，模块与模块间尽可能独立，互相依赖低。没有绝对没有耦合的模块组，只有尽量降低互相之间的影响。使模块越独立越好。

## 五,类与对象

​	==类是对象的抽象，对象是类的实体==

### 1.类和对象的创建

1)类的创建

​	语法：

```java
class 类名称{
    				//属性
    				...
    				//方法
}
```



2)对象的创建

​	语法：

```java
类名称 对象名称 = new 类名称();
```

​	只要看见new关键词，系统则会自动为该对象分配堆空间

### 2.对象属性和方法的调用

​	语法：

```java
对象.属性 = 值；
对象.方法();
```

### 3.类方法的编写

​	语法格式：

```java
访问修饰符 返回值数据类型 方法名称([参数列表]){
	方法具体代码;
}
```

--方法当中有无返回值和参数分为如下几类:

​	(1)无返回值，无参数---->用于打印现实数据

​	(2)无返回值，有参数---->用于接收数据设置信息

​	(3)有返回值，无参数---->用于采集信息、处理信息并返回信息

​	(4)有返回值，有参数---->用于采集信息、处理信息并返回信息

代码示例：

```java
import java.util.Scanner;
public class HelloWorld {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		Person per = new Person();
		per.ID = "2020370227";
		per.name = "邓云希";
		per.age = 18;
		per.showInfo();
		System.out.println("请更改"+per.name+"的ID为：");
		String xH = scan.next();
		per.setId(xH);
		
		System.out.println("更改后"+per.name+"ID为："+per.getID());
		per.chat("love");
	}
}

class Person{
	//--属性
	public String ID;
	public String name;
	public int age;
	//-方法
	//方法-1：无返回值，无参数---->用于打印信息
	public void showInfo() {
		System.out.println("ID:"+ID+";name:"+name+";age:"+age);
	}
	//方法-2：无返回值，有参数---->用于接收数据
	public void setId(String xueHao) {
		ID = xueHao;
	}
	//方法-3：有返回值，无参数---->用于返回数据
	public String getID() {
		return ID;
	} 
	//方法-4:有返回值，有参数---->用于接收信息，处理信息并返回信息
	public String chat(String msg) {
		if(msg.contains("love")) {
			return"i love you too!";
		}else if(msg.contains("hate")) {
			return"i hate you too";
		}else {
			return "who are you?";
		}
	}
}
/*
ID:2020370227;name:邓云希;age:18
请更改邓云希的ID为：
2020370222
更改后邓云希ID为：2020370222

```



### 4.成员变量与局部变量

1)定义位置不同

​	成员变量：定义在类中，方法外

​	局部变量：定义在方法内，语句内

2)作用域不同

​	成员变量：作用范围整个类

​	局部变量：方法内，语句内

3)默认值不同

​	成员变量：成员变量在类中声明时可以不用赋初值，JVM会自动给成员变量赋默认值。

​	局部变量：局部变量在使用前必须先经过显式的初始化。

默认值如下：

Boolean      false

Char           '\u0000'(null)

byte            (byte)0

short           (short)0

int               0

long            0L

float            0.0f

double        0.0d

```java
public class TestStatic {
static int x; //类的成员变量，JVM负责初始化

static int method()

{
int y=0;  //此处必须自己初始化，它不属于类成员变量，是个method的局部变量，JVM不负责初始化

return y;

}

public static void main(String[] args) {
TestStatic as=new TestStatic();

int z=0;  //此处必须自己初始化，它不属于类成员变量，是个主函数里的局部变量，JVM不负责初始化

int aa=3; //此处aa参与了运算，所以必须初始化

aa=aa+2;

int a=1,b=2,max; //max只是负责接收表达式的值，不需要初始化

max=a>b?2:1;

System.out.println(max); //1

System.out.println(aa); //5

System.out.println("z="+z); //z=0

System.out.println("x="+as.x); //x=0

System.out.println("y="+as.method()); //y=0

}

}
```

总结：类里方法外定义的数据成员称为属性，属性可不赋初值，若不赋初值则JAVA会按上表为其添加默认值；方法或者类方法里定义的数据成员称为变量，变量在参与运算之前必须赋初值。

4)内存位置不同

​	成员变量：堆内存

​	局部变量：栈内存

栈内存：存储局部变量，即定义在方法中的变量，使用完毕立即消失；

堆内存：存储new出来的内容（实体、对象） 数组在初始化时，会为存储空间添加默认值。
	每一个new出来的内容都有一个地址值，使用完毕不会立即消失，而是在垃圾回收器空闲时被回收。

5)生命周期不同

​	成员变量：成员变量随着对象的创建而产生，待该对象不再使用而一起被JVM清理。生存周期较长。

​	局部变量：随方法的使用而创建产生，待方法使用完则释放变量内存空间。生命周期较短。

### 4.构造方法和重载

什么是构造方法？

​	构造方法是一种特殊的方法，名称与类名相同，但无返回值，无需用void修饰。

构造方法的功能·：

​	开辟堆内存空间，实例化对象
​	实现对象初始化，通过重载为类的属性赋值

```java
	类名 对象名 = new 类名();
	例: Student s1 = new Student();
```

注意事项:

1) 当一个类中没有给构造方法时.系统将给出一个默认的无参构造方法.
2) 一旦你自己创建出一个构造方法,系统将不在给出默认构造方法.
3) 构造方法可以重载,也可以沿用上一个构造方法继承.

```java
public class Demo02 {
    public static void main(String[] args) {
        Student student1 = new Student();
        student1.show();
        Student student2 = new Student("邱文");
        student2.show();
        Student student3 = new Student(18);
        student3.show();
        Student student4 = new Student("邱文", 18);
        student4.show();
    }
}

class Student {
    String name;
    int age;
    public Student() {
    }

    public Student(String name) {
        this.name = name;
    }

    public Student(int age) {
        this.age = age;
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void show() {
        System.out.println(name + "," + age + ",");
    }
}
运行结果:
    null,0,
    邱文,0,
    null,18,
    邱文,18,

```

### 5.标准类

①含有private修饰的属性
②每个属性都含有它的set和get方法,show方法等
③含有无参和有参构造方法
④创建一个main方法测试类,并通过构造方法new出至少一个实体对象
⑤通过对象实现类当中各个类方法

```java
   public class StudentDemo {
    public static void main(String[] args) {
       // ④创建一个main方法测试类,并通过构造方法new出至少一个实体对象
        Student s1 = new Student();
        //⑤通过对象实现类当中各个类方法
        s1.setName("林青霞");
        s1.setAge(30);
        s1.show();
        Student s2 = new Student("林青霞",30);
        s2.show();
    }
}

     public class Student {
    //属性
    // ①含有priva修饰的属性
    private String name;
    private int age;
        
    //成员方法
    // ③含有无参和有参构造函数
    public Student(){

    }
    public Student(String name,int age){
        this.name = name;
        this.age = age;
    }
    public String getName() {
        return name;
    }
	//②每个属性都含有它的set和get方法,show方法等
    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
    public void show(){
        System.out.println(name+","+age);
    }
}

运行结果:
    林青霞,30
    林青霞,30
```

### 6.面向对象的三大特征为封装，继承和多态。

①封装：==封装也就是把客观事物封装成抽象的类==，并且类可以把自己的数据和方法只让可信的类或者对象操作，对不可信的进行信息隐藏。见类与对象-->封装

②继承extends：==一个类继承另一个类的属性和方法==

继承的优点：可以实现代码的重用

通过继承创建的新类称为"子类"或"派生类"

被继承的类称为"基类"、"父类"或"超类"

且一个类只能继承一个直接父类，但可以实现多个接口。

③多态：==一个类实例（对象）的相同方法在不同情形有不同表现形式==。多态机制使具有不同内部结构的对象可以共享相同的外部接口。

实现多态，有二种方式，覆盖，重载。

覆盖(override)：是指子类重新定义父类的虚函数的做法。

重载(overload)：是指允许存在多个同名函数，而这些函数的参数表不同（或许参数个数不同，或许参数类型不同，或许两者都不同）。重载与返回值无关。

## 六,封装

1)封装的作用：

① 对象的数据封装特性彻底消除了传统结构方法中数据与操作分离所带来的种种问题，提高了程序的可复用性和可维护性，降低了程序员保持数据与操作内容的负担。

②对象的数据封装特性还可以把对象的私有数据和公共数据分离开，保护了私有数据，减少了可能的模块间干扰，达到降低程序复杂性、提高可控性的目的。

一个对象中实列中将复杂的内部细节全部进行封装，只给我们留下简单的接口，通过接口进行调用和操作。

2)封装的优点:

1. 提高代码的安全性。

2. 提高代码的复用性。

3. “高内聚”：封装细节，便于修改内部代码，提高可维护性。

4. “低耦合”：简化外部调用，便于调用者使用，便于扩展和协作

## 七,继承

### 1.继承的格式

```java
public class 子类名 extends 父类名{};
```

​	父类也被称为基类,超类.

​	子类也被称为派生类.

### 2.继承的利弊即注意事项

- 好处:


​	①提高了代码的复用性
​	②提高了代码的维护性

- 弊端:


​	继承让类与类之间产生了联系,类的耦合性增强了,当父类发生变化时子类也不得不发生变化,削弱了类的独立性.

- 注意事项:

1. java当中类支持单继承,不支持多继承
2. java类支持多层继承(即: 儿子 -> 父亲 -> 爷爷)

### 3.继承当中访问成员变量及方法的特点

​		通过子类对象访问一个成员变量和成员方法时,先在子类成员范围内寻找,无果后再在父类成员范围内寻找,如果都没有即报错

### 4.this与super关键字

- this关键字

1. 表示调用本类中的属性,语法: this.属性
2. 表示调用本类当中的方法,语法: this.方法()
3. 表示本类当中的构造方法,语法: this([参数])  //用于子类继承父类的构造方法
4. 表示当前对象,语法: this

- super关键字

1. super用于指向父类的属性和方法.与this指向当前类的属性和方法不同.

二者区别

- 当this指向子类对象的成员时,java程序先访问子类成员,若没有找到则继续访问父类成员.

- ==This和super都不能在main()及static方法中使用,只能在类方法中使用==　　
  　　因为，main()方法是静态的，this是本类对象的引用，静态先于对象，所以是不能使用的。
    　　==this通常指当前对象，super则指父类的==

若使用super则直接访问父类成员,不访问子类.

代码演示

```java

public class Fu {//定义父类
    public int age = 40;
}

public class Zi extends Fu {//定义子类及show方法
    public int age = 30;
    public void show(){
        int age = 20;
        System.out.println(age);//访问的是方法当中的局部变量age
        System.out.println(this.age);//访问本类当中的成员变量age
        System.out.println(super.age);//访问的是父类当中的成员变量age
    }
}

public class Demo01 {//测试类.new一个Zi类对象,调用show方法
    public static void main(String[] args) {
        Zi zi = new Zi();
        zi.show();
    }
}

运行结果:
	20
	30	
  	40
```

### 5.继承中构造方法的访问特点

在测试类当中调用子类构造方法时,会在调用子类构造方法前默认调用父类==无参==构造方法.(相当于在子类的构造方法里面第一句默认为super();)

当子类用super()调用父类的构造方法时,==一定要写在子类构造方法的第一行==.否则报错

```java
public class Fu {//定义两个父类构造方法
    public int age;
    public Fu(){
        System.out.println("访问父类无参构造方法");
    }
    public Fu(int age){
        System.out.println("访问父类带参构造方法");
    }

}

public class Zi extends Fu {//定义两个子类构造方法
    public int age;
    public Zi(){
        //super();默认调用父类无参构造方法
        System.out.println("访问子类无参构造方法");
    }
    public Zi(int age){
        //super();默认调用父类无参构造方法
        System.out.println("访问子类带参构造方法");
    }
}

public class Demo01 {//测试类调用子类构造方法
    public static void main(String[] args) {
        Zi zi = new Zi();
    }
}
运行结果:
    访问父类无参构造方法
    访问子类无参构造方法
```

如果父类没有无参构造方法,则可以指定去调用父类当中的有参构造方法.

```java
public class Fu {//定义两个父类构造方法
    public int age;
    /*public Fu(){
        System.out.println("访问父类无参构造方法");
    }*/
    public Fu(int age){
        System.out.println("访问父类带参构造方法");
    }

}

public class Zi extends Fu {
    public int age;
    public Zi(){
    	super(20);//指定子类调用父类带参构造方法
        System.out.println("访问子类无参构造方法");
    }
    public Zi(int age){
        super(20);//指定子类调用父类带参构造方法，SUper（内添加使用实参
        System.out.println("访问子类带参构造方法");
    }
}

```

### 6.super内存图

当子类调用父类的成员时,会在子类对象new出来的堆内存当中再开辟一个super空间用来存放子类调用的父类的成员.

| ![image-20211116095812529](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211116095812529.png) |
| ------------------------------------------------------------ |

### 7.方法重写(override)

​	子类出现了和父类一样的方法声明

1)方法重写的应用

当子类需要父类的功能,而功能主体子类又持有内容时,可以重写父类当中的方法,这样即沿袭了父类的方法,又定义了子类特有的内容

2)方法重写的注意事项

- 私有方法不能被重写(父类私有成员子类不允许继承)
- 子类方法访问权限不能更低(public > 默认 > 私有)
- 重写是子类对父类的允许访问的方法的实现过程进行重新编写, ==返回值和形参都不能改变==

```java
public class Phone {
    public void call(String name){
        System.out.println("给"+name+"打电话");
    }
}

public class NewPhone extends Phone{
    public void call(String name){
        System.out.println("开启视频");
//        System.out.println("给"+name+"打电话");
        super.call(name);//利用super关键字继承父类当中的call方法,并添加额外的功能语句.
    }
}

public class Demo02 {
    public static void main(String[] args) {
        Phone p = new Phone();
        p.call("邱文宣");
        System.out.println("--------------");
        NewPhone np = new NewPhone();
        np.call("何镇洋");
    }
}
```



### 8.overload与override的区别

1、综述
　　重写(Override)也称覆盖，它是父类与子类之间多态性的一种表现，而重载(Overload)是一个类中多态性的一种表现。 override从字面就可以知道，它是覆盖了一个方法并且对其重写，以求达到不同的作用。overload它是指我们可以定义一些名称相同的方法，通过定义不同的输入参数来区分这些方法，然后再调用时，JVM就会根据不同的参数样式，来选择合适的方法执行。

2、override（重写，覆盖）
（1）方法名、参数、返回值相同。
（2）子类方法不能缩小父类方法的访问权限。
（3）子类方法不能抛出比父类方法更多的异常(但子类方法可以不抛出异常)。
（4）存在于父类和子类之间。
（5）方法被定义为final不能被重写。
（6）被覆盖的方法不能为private，否则在其子类中只是新定义了一个方法，并没有对其进行覆盖。

3、overload（重载，过载）
（1）参数类型、个数、顺序至少有一个不相同。
（2）不能重载只有返回值不同的方法名。
（3）针对于一个类而言。
（4）不能通过访问权限、返回类型、抛出的异常进行重载；
（5）方法的异常类型和数目不会对重载造成影响；

4、override应用：
（1）最熟悉的覆盖就是对接口方法的实现，在接口中一般只是对方法进行了声明，而我们在实现时，就需要实现接口声明的所有方法。
（2）除了这个典型的用法以外，我们在继承中也可能会在子类覆盖父类中的方法。

5、总结
　　override是在不同类之间的行为，overload是在同一个类中的行为。

### 9.object

-  Object类是所有类的父类	
- 所有类都可以继承Object中允许被继承的方法
-  一个类没有使用extends关键字明确标识继承关系，则默认继承Object类

### 10.抽象函数（abstract）

```java
package shapes;
public abstract class Shapes {
	//构造抽象类Shape，抽象类不能直接拿来创建对象。而是通过以父类的作用继承给子类继而创建对象
	public abstract void draw();
    //该抽象函数drow的作用为提示以shapes为父类的子类当中需含有一个drow()函数，且必须要有一个drow()函数，但抽象函数本身不能定义函数体
    //一旦该类被定义为抽象类，则类内的所有成员函数都为抽象函数，需加（abstract）
}

```

```java
public abstract class Shapes {
	public abstract void draw() {};//（错误）抽象函数不能有大括号
	public static void main(String[] args) {
		shape s = new Shape();//（错误）抽象类不能产生对象
	}
}
```

总结：

- 抽象函数——表达概念而无法实现具体代码的函数

- 抽象类——表达概念而无法构造出实体的类，带有abstract修饰符的函数的类一定为抽象类

### 11.权限修饰符

四种权限修饰符:private, default,protected,public

| 修饰符    | 类内部 | 同一个包 | 不同的子类 | 同一个工程 |
| :-------- | :----: | :------: | :--------: | :--------: |
| private   |   √    |          |            |            |
| (缺省)    |   √    |    √     |            |            |
| protected |   √    |    √     |     √      |            |
| public    |   √    |    √     |     √      |     √      |

java当中修饰类只能用缺省/public修饰符。相比于缺省修饰符，public能被不同工程调用。（及类内的话可以用四种权限修饰符，而在类外修饰类的话只能用缺省或public)

### 12.状态修饰符

1)阻止继承: final

- final修饰符用来修饰方法,表明该方法是最终方法,不可以被重写

- final也可以用来修饰类,表明该类被称为最终类,不可被继承

- final也可以用来修饰变量,该变量即变成常量,不能被再次赋值,且被final修饰的变量名最好大写,表常量

  全局常量：关键字public + static + final联合修饰
   *	public final static 数据类型 常量名称 = 常量值;
  
  ```java
  //final修饰基本类型变量
  final int age = 20;
  age = 200;
  System.out.println(age);//修饰的age变量为常量,不可被赋值
  
  //final修饰引用类型变量
  final Student s = new Student(100);//即fianl修饰对象s ,仅仅只是将s的地址置为不变,地址里面的内容仍可变.
  s.age = 200;
  System.out.println(s.age);
  
  输出结果:
   200
   100
  
  ```
  
  

2)static关键字 

static可以用来修饰成员方法和成员变量

类不可以被修饰成静态类，即只能修饰静态成员方法和成员变量 例：

```java
public static void main{} //静态main方法

public static String nums;//静态成员变量
```

如果一个属性或者方法被关键字static修饰，那么这个属性或者方法就有了类的性质，可以不用通过对象来调用,可以直接使用类名称调用，语法：类名称.属性  或者  类名称.方法()

静态成员变量是属于类的，也就是说，该成员变量并不属于某个对象，即使有多个该类的对象实例，静态成员变量也只有一个。只要静态成员变量所在的类被加载，这个静态成员变量就会被分配内存空间。因此在引用该静态成员变量时，通常不需要生成该类的对象，而是通过类名直接引用。引用的方法是“类名 . 静态变量名”。

与静态成员变量类似，静态成员方法是类方法，它属于类本身而不属于某个对象。因此静态成员方法不需要创建对象就可以被调用，而非静态成员方法则需要通过对象来调用。

特别需要注意的是，在静态成员方法中不能使用 this、super 关键字，也不能调用非静态成员方法，同时不能引用非静态成员变量。这个道理是显而易见的，因为静态成员方法属于类而不属于某个对象，而 this、super 都是对象的引用，非静态成员方法和成员变量也都属于对象。

例:定义一个静态ScannerTool类,用于直接通过类名调用Scanner方法,而不需要通过new一个对象来调用其方法;

```java
public class ScannerTool {
	static Scanner scan = new Scanner(System.in);;
	//提供一个可以通过类直接取得的Scanner对象
	public static Scanner getScanner(){
		return scan;
	}
    
    public static void main(String[] args) {
		//Scanner scan = new Scanner(System.in);
		System.out.println("欢迎您来到计嵌宠物店^-^");
		System.out.println("请输入要领养的宠物的名字:");
		String name = ScannerTool.getScanner().next();//直接用类名调用方法,省去了创建对象
        //String name = scan.next();
    }
```

static修饰的特点

- 可被类的所有对象共享

- 可以直接通过类名调用,也可以通过对象名调用,但==推荐使用类名调用==.

  ```java
  public class Student {
      private String name;
      private int age;
      public  static String university;//被静态修饰时权限最好改为public
  }
  
  public class StaticDemo01 {
      public static void main(String[] args) {
          Student.university = "传智大学";//直接用类名调用被attic修饰的成员变量和方法,并给他们静态赋值
          Student s1 = new Student();
          s1.setName("林青霞");
          s1.setAge(30);
  //        s1.setUniversity("传智大学");静态赋值之后无需再为其赋值.
          System.out.println(s1.toString());
  
          Student s2 = new Student();
          s2.setName("风清扬");
          s2.setAge(33);
  //        s2.setUniversity("传智大学");
          System.out.println(s2.toString());
  
      }
  }
  
  StaticDemo{name='林青霞', age=30, university='传智大学'}
  StaticDemo{name='风清扬', age=33, university='传智大学'}
  ```


- 静态成员方法只能访问静态成员

  ​	因为main方法是静态方法,所以定义方法时都必须加上static修饰符,以确保可以被main方法调用

  ```java
  public class Student {
      private String name;
      private int age;
      public static String university;
      
      public void show1(){//非静态成员方法可以访问所有静态和非静态成员
          System.out.println(name);
          System.out.println(age);
          System.out.println(university);
          show1();
          show2();
      }
      public static void show2(){//静态成员方法只能访问静态成员
          System.out.println(name);//报错
          System.out.println(age);//报错
          System.out.println(university);
          show1();//报错
          show2();
      }
  ```

  知道了static和private之后，我们可以实现第二个设计模式：==单例设计模式==

### 13.单例设计模式

单例设计模式：整个程序中只有一个类的实例，也就是说无法再实例化多余的实例

关键: 私有化private构造方法，这样类的外部就不能直接访问构造，不能new构造，也就无法实例化对象此时类的内部提供一个唯一的可以通过类名称访问的静态static实例对象

```java
public class SIngleDemo {//单例模式
	public static void main(String[] args) {
//		虽然创建了两个对象Wife1和Wife2,但两个对象完全相同
//		在单例模式下,永远只存在一个对象
		Wife wife1 = Wife.getWife();
		Wife wife2 = Wife.getWife();
		System.out.println(wife1);
		System.out.println(wife2);
	}
}

class Wife{
	//1--私有化构造方法
	private Wife() {}
//	类内部唯一一次new构造,实例化一个唯一的对象,并将其私有化
	private static Wife onlyYou = new Wife();
	//2--内部对外提供一个静态的实例对象
	public static Wife getWife() {
		return onlyYou;
	}
}
```



## 八,多态

### 1.多态概述

​	==同一个对象在不同时刻表现出来的不同形态==

例:猫

​	我们可以说猫是猫: 猫 cat= new 猫();
​	我们也可以说猫是动物: 动物 cat= new cat();
​	这种猫在不同时刻表现出的不同形态成为多态.

### 2.多态的前提

- 有继承/实现关系

- 有方法重写

- 有父类引用指向子类对象



### 3.多态中成员访问热点

如: Animal cat = new cat(); 

由于成员方法有重写,所以当cat对象引用成类员时:

- 成员变量: 编译看左边,执行看左边
- 成员方法: 编译看左边,执行看右边

### 4.多态的利弊

1)优点:提高了代码的拓展性

具体体现: 定义类方法时参数使用父类类型,当子类对象调用该方法时使用具体的子类类型参数,无需再而外增加不同参数类型的类方法.提高了代码的可拓展性

定义6个类:Animal,  AnimalOperator, AnimalDemo, Cat, Dog, Pig

其中Cat, Dog, Pig都继承自Animal 

```java
public class Animal {
    public void eat(){
        System.out.println("动物吃东西");
    }
}

public class AnimalOperator {
    public void useAnimal(Cat c){
        c.eat();
    }
    public void useAnimal(Dog d){
        d.eat();
    }
    public void useAnimal(Pig p){
        p.eat();
    }
}

public class Cat extends Animal{
    @Override
    public void eat() {
        System.out.println("猫吃鱼");
    }
}

public class Dog extends Animal{
    @Override
    public void eat() {
        System.out.println("狗吃骨头");
    }
}

public class Pig extends Animal{
    @Override
    public void eat() {
        System.out.println("猪吃白菜");
    }
}

public class AnimalDemo {
    public static void main(String[] args) {
        AnimalOperator ao = new AnimalOperator();

        Cat c= new Cat();
        ao.useAnimal(c);

        Dog d = new Dog();
        ao.useAnimal(d);

        Pig p = new Pig();
        ao.useAnimal(p);//此方法用来调用AnimalOpreator方法,需要每创建一个类如Pig,就需要重新在AnimalOpreator类当中添加一个重载方法,比较繁琐,我们可以利用Pig,Cat,Dog类皆继承自Animal这一个父类的特点,利用子类的多态性将Pig类理解为Animal类的一种,即Animal p = new Pig();

    }
}

```

优化后:

```java
public class AnimalOperator {
   /* public void useAnimal(Cat c){
        c.eat();
    }
    public void useAnimal(Dog d){
        d.eat();
    }
    public void useAnimal(Pig p){
        p.eat();
    }*/
    public void useAnimal(Animal a){//优化后的方法,无需因为类的创建而重新定义类方法,即利用了Animal子类的多态性
        a.eat();
    }
}

public class AnimalDemo {
    public static void main(String[] args) {
        AnimalOperator ao = new AnimalOperator();

     /*   Cat c= new Cat();
        ao.useAnimal(c);

        Dog d = new Dog();
        ao.useAnimal(d);

        Pig p = new Pig();
        ao.useAnimal(p);*/
        Animal c = new Cat();//利用子类的多态性,提高了代码的拓展性
        Animal d = new Dog();
        Animal p = new Pig();

    }
}

```

2)多态的弊端:

​	当以父类的形式创建子类对象时,该子类会失去原本子类特有的成员方法和变量,即该子类不能访问子类特有的功能.

```JAVA
public class Dog extends Animal{//在原本Dog类当中定义一个独有的lookDoor方法
    @Override
    public void eat() {
        System.out.println("狗吃骨头");
    }
    public void lookDoor(){
        System.out.println("狗看们")
    }
}

public class AnimalDemo {
    public static void main(String[] args) {          
        Animal d = new Dog();  
        d.LookDoor()//报错;即当以父类类型创建一个子类对象时,该子类不能访问子类独有的功能,只能访问父类当中存在的功能
    }
}

```

### 5.多态的转型

1)向上转型(小变大/子转父)

​	从子到父

​	父类引用指向子类对象

```java
 Animal a = new Cat();
```

2)向下转型(大变小/父转子)

​	从父到子

​	父类引用转为子类对象

```java
 Cat c = (Cat)a; 
```

我们可以把继承看做是一个由父类向下发展的树状图,由子类转为父类方向向上,父类转子类方向向下

```
									父类
									 |
									子类  									 					
```



```java
public class Cat extends Animal{
    @Override
    public void eat() {
        System.out.println("猫吃鱼");
    }
    public void playGame(){
        System.out.println("猫猫爱玩耍");
    }
}

public class AnimalDemo {
    public static void main(String[] args) {
        Animal a = new Cat();//把子类对象赋值给父类的引用称为向上转型
        a.eat();            /*多态中访问成员方法编译看左边,运行看右边.
       						即左边编译的时候发现Animal内含有eat()方法,编译通过,
       						运行的时候看Cat内存在被重写的eat()方法,运行成功.*/
        a.playGame();       /*多态中访问成员方法:编译看左边,运行看右边.
                            Animal a = new Cat();左边Animal当中不存在playGame()方法,编译失败.系统报错!*/ 
        Cat c = (Cat)a; 	//把父类引用转为子类对象称为向下转型
        c.eat();
        c.playGame();		//通过向下转型,将父类对象转为子类引用,此时对象可以使用子类特有的功能
    }
}

```

多态示例;

```java
public class Animal {
    public void eat(){
        System.out.println("动物吃东西");
    }
}

public class Cat extends Animal{
    @Override
    public void eat() {
        System.out.println("猫吃鱼");
    }
    public void playGame(){
        System.out.println("猫猫爱玩耍");
    }
}

public class Dog extends Animal{
    @Override
    public void eat() {
        System.out.println("狗吃骨头");
    }
    public void lookDoor(){
        System.out.println("狗看门");
    }
}

public class AnimalDemo {
    public static void main(String[] args) {
        //向上转型 Cat a转为Animal
        Animal a = new Cat();
        a.eat();//转型后Cat a不能使用Cat的特有功能:如a.playGame();

        //向下转型 Animal a转为 Cat
        Cat c = (Cat) a;//向下转型需要创建新对象名,此时Cat c为转型后的对象,Animal a仍然存在
        c.eat();
        c.playGame();//原本父类Animal a通过向下转型转为子类Cat c,就可以使用子类Cat的特有playGame功能
		
        ((car) a).playGame();//通过强制类型转换让Animal a访问到子类Cat当中的变量
        
        //向上转型Animal a转为 Dog
        a = new Dog();
        a.eat();//向上转型虽然限制了子类特有功能的使用,但还是会调用子类当中的重写方法.

        //向下转型Animal a转为 Dog
        Dog d = (Dog)a;
        d.eat();
        d.lookDoor();//向下转型释放了一定的子类功能可以被使用,但须注意原本的父类并没有消失,
                    // 而是根据需求额外创建了一个子类对象用于调用子类当中的功能

        //d = (Dog)c;//同级类名之间不可以发生转型,否则系统报错:ClassCaseException 类型转换异常
    }
}

```

总结:

- 向上转型(限制转型):由子类转型成父类,转型后的子类无法访问子类独有的功能,但是还是可以访问子类当中的重写方法.由于子类本身就属于父类,可以直接发生向上转型而无需强制类型转换
- 向下转型(强制类型转换):父类为了使用子类当中的特有方法,将原本的父类强制类型转换成子类对象,(注意:向下转型不是将父类对象永久转换成子类对象,而是额外创建出一个由父类对象所转型出的新子类对象,原父类对象仍然存在且为父类,所创建出的子类对象与父类对象名须不同) 
- 直接添加强制类型转换符进行向下转型:((car) a).playGame();//Animal直接访问子类Cat当中playGame方法;

## 九,抽象abstract

abstract只能修饰类和类方法，不能修饰成员变量

### 1.抽象类的特点

1. 抽象类或抽象方法需要加abstract关键字;抽象方法格式例: public abstract void eat();//不能有方法体
2. 含有抽象方法的类一定是抽象类,抽象类里面可以不含抽象方法
3. 抽象类不能直接实例化对象,但可以参照多态的方式通过子类实例化抽象类对象  例:抽象类Animal  Animal a = new Cat();
4. 抽象类的子类要么重写抽象类中所有的抽象方法,要么就是抽象类//即如果抽象类的子类也是抽象类,则子类可以不用重写父类抽象类的方法.

### 2.抽象类的成员特点

- 成员变量

  ​	既可以是变量也可以是常量	

- 构造方法

  ​	有构造方法,但不能实例化,用于给子类访问父类数据的初始化

- 成员方法

  ​	可含抽象方法,一旦定义了抽象方法则限定子类必须对该方法进行重写.也可以有非抽象方法.  含有抽象方法的类一定是抽象类,抽象类不一定含有抽象方法.

```java

public abstract class Animal {//abstract声明抽象类
    private String name;
    private int age;

    public Animal() {
    }

    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public abstract void eat();//定义抽象方法

    public void sleep(){//定义非抽象方法
        System.out.println("睡觉");
    }
}

public class Cat extends Animal{
    public Cat() {
    }

    public Cat(String name, int age) {
        super(name, age);
    }
    
	@Override
    public void eat(){//子类继承抽象类必须要重写父类当中的抽象方法
        System.out.println("猫吃鱼");
    }
}

public class Dog extends Animal{
    public Dog() {
    }

    public Dog(String name, int age) {
        super(name, age);
    }

    @Override
    public void eat() {
        System.out.println("狗吃骨头");//子类继承抽象类必须要重写父类当中的抽象方法
    }
}

public class AnimalDemo {
    public static void main(String[] args) {
        Animal a = new Cat();
        a.setName("加菲");
        a.setAge(5);
        System.out.println(a.getName()+","+ a.getAge());
        a.eat();
        System.out.println("------------------");

        a = new Cat("加菲",5);
        System.out.println(a.getName()+","+a.getAge());
        a.eat();
    }
}

```

​	abstract抽象类与抽象方法---->应用场景：模板设计模式

### 3.抽象类名作为形参和返回值

1)抽象类作形参和返回值与普通类作形参和返回值对比:

```java
public abstract class Animal {
    public abstract void eat();
}

public class Cat extends Animal{
    @Override
    public void eat() {
        System.out.println("猫吃鱼");
    }
}

public class AnimalOperator {
    public void useAnimal(Animal a){
        a.eat();
    }
     public Animal getAnimal(){
        Animal a = new Cat();
        return a;
    }
}

public class CatOperator {
    public void useCat(Cat c){
        c.eat();
    }
    public Cat getCat(){
        Cat c = new Cat();
        return c;
    }
}

public class Demo {
    public static void main(String[] args) {
        CatOperator co = new CatOperator();
        Cat c1 = new Cat();
        co.useCat(c1);//普通类作形参
	    Cat c2 = co.getCat();//类名称作返回值,等同于Cat c2 = new Cat();
        c2.eat();
        
        AnimalOperator ao = new AnimalOperator();
        Animal a1 = new Cat();
        ao.useAnimal(a1);//抽象类作形参
        //由于抽象类不能直接创建对象,但又必须要使用抽象类怎么办呢,可以利用多态将Animal继承给子类Cat,让子类通过多态来创建一个Animal对象 Animal a = new Cat();
        Animal a2 = ao.getAnimal();//抽象类作为返回值,等同于Animal a2 = new Cat();
        a2.eat();
    }
}

```

总结:

- 方法的形参是(抽象)类名,其实需要的是该(抽象)类的子类对象
- 方法的返回值是(抽象)类名,其实返回的是该(抽象)类的子类对象



## 十,接口

----

### 1.接口特点

- 1.接口是一个特殊的类,此类中只有==公共全局常量==和==公共抽象方法==

  由关键字interface声明

  ```java
  interface 接口名{
  	public static final 数据类型 常量名 = 值;//公共全局常量
  	public abstract 方法返回值数据类型 方法名称([参数]);//公共抽象方法
  }
  ```
  
  2.接口无法直接呗实例化,必须由子类继承实现,且java当中类与接口之间可以多实现implements,接口与接口之间可以多继承.
  
  3.接口比抽象类抽象的更加彻底,表现的是一种能力,描述的是"has-a"的关系
  
  4、需求：利用抽象类和接口特性表述防盗门实现
  
  ​		防盗门是一个门，描述的是is-a的关系，继承父类<抽象类>
  
  ​		防盗门有锁，具有上锁和开锁功能，描述的是has-a的关系，实现接口	
  
  5.接口的应用: 工厂设计模式+[单例设计模式 + 模板设计模式]

### 2.接口的成员特点

1. 成员变量

   只能是常量 ,默认修饰符为 public static final 

2. 构造方法

   接口没有构造方法,因为接口主要是对行为进行抽象的,没有具体存在,接口默认继承object类.

   由于接口没有构造方法,子类继承接口的构造方法时使用的是object内的无参构造方法

   例:public class 类名 implements 接口名{} 

   等价于 public class 类名 ==extends object== implement 接口名{}

3. 成员方法

   只能是抽象方法

   默认修饰符: public abstract

   ```java
   
   public interface Jumping {
   	public abstract void jump();
   }
   
   public abstract class Animal {
   	
   	private String name;
   	private int age;
   	public Animal() {
   		super();
   	}
   	public Animal(String name, int age) {
   		super();
   		this.name = name;
   		this.age = age;
   	}
   	public String getName() {
   		return name;
   	}
   	public void setName(String name) {
   		this.name = name;
   	}
   	public int getAge() {
   		return age;
   	}
   	public void setAge(int age) {
   		this.age = age;
   	}
   	public abstract void eat();
   }
   
   public class Cat extends Animal implements Jumping{//猫类即继承了父类Animal也实现了接口Jumping,拥有最多的功能.具有运行意义;
   	@Override
   	public void eat() {
   		// TODO Auto-generated method stub
   		System.out.println("猫吃鱼");
   	}
   	@Override
   	public void jump() {
   		// TODO Auto-generated method stub
   		System.out.println("猫可以跳高啦");
   	}
   }
   
   public class Demo01 {
   
   	public static void main(String[] args) {
   		Cat c = new Cat();
   		c.setName("加菲");
   		c.setAge(5);
   		System.out.println(c.getName()+","+c.getAge());
   		c.eat();
   		c.jump();
   	}
   
   }
   
   运行结果:
       加菲,5
       猫吃鱼
       猫可以跳高啦
   ```
   

### 3.类和接口的关系

- 类和类的关系

  ==继承关系== 只能单继承,但是可以实现多层继承

- 类和接口的关系

  ==实现关系== 可以单实现,也可以多实现,还可以在继承一个类的同时实现多个接口

- 接口和接口的关系

  ==继承关系== 可以单继承,也可以多继承

```java
public class InterImpl extends Object implements Inter1,Inter2,Inter3 {
    //类和接口的关系:类可以实现多个接口,也可以在继承了父类的情况下实现多个接口
}

public interface Inter3 extends Inter1,Inter2{
    //接口与接口之间是继承关系,即可以单继承也可以多继承
}
```

### 4.抽象类和接口的区别

- 成员区别

  抽象类--> 即有变量,也有常量; 有抽象方法,也有非抽象方法

  接口--> 只有常量; 抽象方法

- 关系区别

  类与类--> 继承,单继承

  类与接口--> 实现,可以单实现,也可以多实现

  接口与接口--> 继承, 单继承,多继承

- 设计理念区别

  抽象类--> ==对类抽象==,包括属性,行为

  接口--> ==对行为抽象==,主要是行为方法

例如一个抽象类门Door(),接口Alarm();

​		抽象类Door定义了门的基本功能open(),close(),进而被子类防盗门继承,防盗门相比于Door额外多了一个报警功能Alarm(),但如果直接在抽象类Door()中额外添加这个功能,则其他继承Door的子类也都具有了Alarm()功能,不符合常理,由于接口interface只是对行为方法的抽象,因此可以定义一个功能接口对子类进行选择性的功能实现,而不会干扰到其他子类及父类功能.好比把接口比作一个插座,即插即用,提高了代码的维护性.

### 5.instanceof关键字

在 Java 中可以使用 instanceof 关键字判断一个对象是否为一个类（或接口、抽象类、父类) 的实例 .

语法格式如下:返回的是一个boolean类型

```java
boolean result = obj instanceof Class
```

也可以判断该对象的类是否为另一个类的子类

```java
Person n = new Person();
Teacher t = new Teacher();
System.out.println(t instanceof Person)//判断对象t是否属于Person类的子类对象,输出结果为true
```



## 十一,内部类

1.内部类 需间接调用

内部类就是在一个类当中定义一个类.举例:在一个类A当中定义一个类B,类B就被当成内部类

```java
内部类的定义格式:
public class 类名{
	修饰符 class 类名{
	}
}
范例:
public class Outer{
    public class Inner{
    }
}
```

特点:

​	1.内部类可以直接访问外部类的成员,包括私有

​	2.外部类要访问内部类的成员,必须要创建对象

```java
public class Outer {
	private int num = 20;
	public class Inter{
		public void show() {
			System.out.println(num);//内部类可以直接访问外部类的成员,包括私有
		}
	}
	public void method() {
//		show();
		Inter i = new Inter();
		i.show();//外部类要访问内部类传言,需要创建对象再调用
	}
}

```

外界访问内部类对象的格式:

​		外部类名.内部类名 对象名 = 外部类对象.内部类对象;

```java

public class OuterDemo {
    public static void main(String[] args) {
//        Inner i = new Inner();
        Outer.inner oi = new Outer().new inner();//测试类内创建内部类对象的方法
        oi.show();
    }
}

```

​	一般来说设计内部类目的是将其隐藏在外部类当中,使外界不可见,因此内部类用private

修饰符修饰,所以外部类通过创建内部类对象 的方法来访问内部类成员不可靠.

改进方法:

```java

public class Outer {
    private int num = 0;

    private class Inner {//内部类被private隐藏
        public void show() {
            System.out.println(num);
        }
    }

    public void method() {//调用该方法即可访问内部类的方法
        Inner i = new Inner();
        i.show();
    }
}

public class OuterDemo {
    public static void main(String[] args) {
//        Inner i = new Inner();
//        Outer.inner oi = new Outer().new inner();//测试类内创建内部类对象的方法
//        oi.show();
        Outer o = new Outer();
        o.method();//等同于Outer.inner oi = new Outer().new inner(); 
        				//oi.show();
    }
}


```

2.局部内部类 (间接调用)

局部内部类是在方法中定义的类所以外界是无法直接访问的,需要在方法内部创建对象并调用.

局部内部类可以直接访问外部类的成员,也可以访问方法内的局部变量

```java

public class Outer {
    private int num1 = 0;

    public void method(){
        int num2 = 20;
        class Inner{
            public void show(){
                System.out.println(num1);//既可以访问内部属性;也可以访问外部类的属性
                System.out.println(num2);
            }
        }
        Inner i = new Inner();
        i.show();
    }
}

```

3.匿名内部类(局部内部类的特殊形式)

本质:是一个继承了该类或者实现了该类接口的子类匿名对象

```java
格式:
	new 类名或者接口名(){
		重写方法;
	};//本质上是一个对象
调用该对象方法格式:
	new 类名或者接口名(){
		重写方法;
	}.成员方法();
```

```java
//举例:
public interface Inter {
    void show();
}//定义一个接口

public class Outer{
    public void method(){
        new Inter(){
            @Override
            public void show() {
                System.out.println("匿名内部类");
            }
        }.show();//由于本质上是一个对象,因此可以通过 对象.方法名();来调用方法
    }
}

```

也可以通过多态的方式调用:

```java
package demo01;
public class Outer{
    public void method(){
       Inter i = new Inter(){//由于匿名内部类继承了接口Inter,采用多态的方法将匿名类向上转型成Inter接口类型的对象,通过对象名调用内部类方法
            @Override
            public void show() {
                System.out.println("匿名内部类");
            }
        };
       i.show();//通过对象名调用内部类方法
    }
}
```

匿名内部类的作用:

一般形式:

```java

public interface Jumpping {
    void jump();
}

public class JumppingOperator {
    public void method(Jumpping j){
        j.jump();
    }
}

public class Cat implements Jumpping{
    @Override
    public void jump() {
        System.out.println("猫可以调高了");
    }
}

public class Dog implements Jumpping{
    @Override
    public void jump() {
        System.out.println("狗可以调高了");
    }
}

public class JumppingDemo {
    public static void main(String[] args) {
        JumppingOperator jo = new JumppingOperator();
        Jumpping j = new Cat();//多态实例化接口,编译看左边,执行看右边
        jo.method(j);

        Jumpping j2 = new Dog();
        jo.method(j2);//通过直接创建两个类Cat,Dog,通过调用该类对象实现相应的功能
    }   
    
    运行结果:
    猫可以调高了
	狗可以调高了
```

此方法相对比较麻烦,而且还需额外定义两个类;

采用内部类的方法效果:减少无需的类对象的创建,代码更简洁

```java
public class JumppingDemo {
    public static void main(String[] args) {
        JumppingOperator jo = new JumppingOperator();
        Jumpping j = new Cat();//多态实例化接口,编译看左边,执行看右边
        jo.method(j);

        Jumpping j2 = new Dog();
        jo.method(j2);
        System.out.println("-------------------------------");

        jo.method(new Jumpping() {//即jo.method(...);在...内创建一个匿名内部类,因为匿名内部类本身就是一个对象,所以无需而外创建类去new一个对象了
            @Override
            public void jump() {
                System.out.println("猫可以跳高了");
            }
        });
        jo.method(new Jumpping() {
            @Override
            public void jump() {
                System.out.println("狗可以条高了");
            }
        });
    }
}
运行结果:
    猫可以调高了
    狗可以调高了
    --------
    猫可以跳高了
    狗可以条高了

```



## 十一,OBJECT类

java当中所有类都直接或间接继承object类,称为单根结构

object类当中含有的方法t

1.toString:返回对象的地址; 子类覆写toString默认是显示该子类对象的值的字符格式

2.equals:返回一个boolean类型;

## 十二,异常

### 1.异常

​	异常的含义:程序中不正常的指示流,会造成程序的执行中断

​	常见异常:数组越界,算数异常,类型转换异常...

### 2.java当中常见异常体系

​	==Throwable==  类是java语言中所有错误或异常的超类.但Throwable也继承Object类,其子类有:

- ==Exception== 类用于用户程序可能出现的异常情况，它也是用来创建自定义异常类型类的类。Exception又分为==RuntimeException==和==非RuntimeException==异常
  - RuntimeException:编译可以通过,即在运行时才能发现的错误
  - 非RuntimeException: 编译不可以通过,需要处理后才能正常运行
- ==Error== 定义了在通常环境下不希望被程序捕获的异常。一般指的是 JVM 错误，如堆栈溢出

| ![img](http://c.biancheng.net/uploads/allimg/181023/3-1Q0231H424V1.jpg) |
| ------------------------------------------------------------ |

### 3.JVM默认异常处理机制

​	如果程序出现了问题,我们不做任何处理的话,JVM会默认做出处理(默认调用Throwable当中的PrintStractTrace方法)

​	1.把程序的异常的名称,异常原因和异常出现的位置输出在控制台

​	2.程序在异常位置之后停止运行

| ![image-20211211160753818](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211211160753818.png) |
| ------------------------------------------------------------ |

### 4.java当中异常处理方式

Java的异常处理是通过5个关键字来实现的：try、catch、 finally、throw、throws

#### 1.try-catch-finally (抓抛型)

具体语法:

```java
try{
	可能存在异常的代码块
}catch(XXXException e){
	捕获try模块中出现的异常
}...finally{//这类异常处理中,可以捕获多个异常对象,但是粗异常放在细异常之后
	不管有没有异常,都会执行的代码
}
```

执行流程:

程序从try里面的代码开始执行

出现异常,会自动生成一个异常类对象,该异常类对象将被提交给java运行时系统

当java运行时系统接收到异常对象时,会到catch当中去寻找匹配的异常类,然后进行异常的处理,执行完毕之后程序还可以往下执行.(自定义设置异常处理机制本质上是为了弥补JVM遇到异常程序不会往下执行的缺点)

| ![image-20211211161523230](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211211161523230.png) |
| ------------------------------------------------------------ |

#### 2.throws处理异常

对于异常处理机制,有些情况我们是不能或不需要立马处理该异常的,因此我们可以使用throws关键字将异常抛出,等到需要处理异常的情况下进行处理.

```java
例:
public static void method() throws XxxException {
	方法体;
}
```

| ![image-20211211170345378](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211211170345378.png) |
| ------------------------------------------------------------ |

![image-20211211171220347](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211211171220347.png)

#### 3.自定义异常

自定义异常若继承Exception类则可看作一个非RuntimeException异常类

继承RunException类则作为一个RuntimeException异常类去使用

```java
public class 异常类名 extends Exception{//自定义异常继承自Exception或RunException
	无参构造;
	带参构造;
}
```

| ![image-20211211192513045](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211211192513045.png) |
| ------------------------------------------------------------ |

测试方法:

| ![image-20211211192835532](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211211192835532.png) |
| ------------------------------------------------------------ |



### 5.Throwable的成员方法

| ![image-20211211162102006](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211211162102006.png) |
| ------------------------------------------------------------ |

| ![image-20211211163918694](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211211163918694.png) |
| ------------------------------------------------------------ |

### 6.编译时异常和运行时异常的区别

1.编译时异常==非RuntimeException==: 必须通过异常处理机制(如try-catch)进行处理,否则程序就会发生错误,无法通过编译

2.运行时异常==RuntimeException==: 无需通过异常处理机制进行处理,JVM会默认输出处理结果但程序无法继续执行,但也可以和编译时异常一样处理,程序可继续执行.



## 十三,基础类型包装类

### 1.基本类型包装类概述

将基本数据类型封装成对象的好处是可以在对象中定义更多的功能方法操作该数据

常见的操作之一: 用于基本数据类型与字符串之间的转换

基本数据类型与Java号称一切接对象矛盾，因此Java为8中基本数据类型提供了引用数据类型<包装类>
byte  short  int          long  float  double  boolean  char
Byte  Short  Integer  Long Float  Double  Boolean  Character

| ![image-20211207195210291](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211207195210291.png) |
| ------------------------------------------------------------ |

### 2.Integer包装类的概述和使用

integer:包装一个对象中原始类型int的值

| ![image-20211207195821377](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211207195821377.png) |
| ------------------------------------------------------------ |

```java

public class IntegerDemo {
    public static void main(String[] args) {
//        Integer i1 = new Integer(100);这种方法构造Integer类型过时了
//        Integer i2 = new Integer("abc");
//        System.out.println(i1);
//        System.out.println(i2);
        Integer i3 = Integer.valueOf(100);
//        Integer i4 = Integer.valueOf("abc");
        System.out.println(i3);
//        System.out.println(i4);
    }
}
输出:
	100
```

| ![image-20211207201850814](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211207201850814.png) |
| ------------------------------------------------------------ |

### 3.int 和String类型的相互转换

```java
1.将String转换成int类型
	1）int i = Integer.parseInt([String]); 或 i = Integer.parseInt([String],[int radix]);
	2） int i = Integer.valueOf(my_str).intValue();
2.将int转换成String类型
	1）String s = String.valueof(i);
	2)  String s = Integer.toString(i);
    3)  String s = ""+i; 
```



```java

import org.w3c.dom.ls.LSOutput;

public class IntegerDemo {
    public static void main(String[] args) {
        //int转换成String
        int number = 100;
        //方式1
        String s1 = "" + number;
        System.out.println(s1);
        //方式2
//        public static String valueOf(number);
        String s2 = String.valueOf(number);
        System.out.println(s2);
        System.out.println("====================");
//        String ----int
        String s = "100";
//        方式1
//        String---Integer---int
        Integer i = Integer.valueOf(s);
        int x = i.intValue();
        System.out.println(x);
        //方式2
        //public static int parseInt(String s)
        int y = Integer.parseInt(s);
        System.out.println(y);
    }
}
输出:
    100
    100
    ====================
    100
    100
```

总结:

基本类型包装类常见的操作就是:用于基本类型和字符串类型之间的转换

​	1.int转String类型

```java
public static String valueOf(int i)//返回int参数的字符串表达形式,该方法是String类中的方法
```

​	2.String转换为int(String-Integer-int)

```java
public static int parseLnt(String s)//将字符串解析为int类型,该方法是Integer类中的方法
```

## 十四,装箱和拆箱

1.装箱:把基本数据类型转换为对应的包装类类型

2.拆箱:把包装类类型转换成对应的基本数据类型

```java
package day1208;

public class Demo01 {
    public static void main(String[] args) {
        //装箱:把基本数据类型转换成对应的包装类类型

        Integer i2 = 100;//自动装箱
        Integer i1 = Integer.valueOf(100);//手动装箱
        //拆箱:把包装类类型转换成对应的基本类型
        i1 += 200;//自动拆箱
        i1 = i1.intValue() + 200;//手动拆箱

        System.out.println(i1);
        System.out.println(i2);
    }
}

```



## 十五,集合体系结构

| ![image-20211211194622623](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211211194622623.png) |
| ------------------------------------------------------------ |

### 1.排序

​	集合排序方法 Java ArrayList sort() 方法

​	sort() 方法根据指定的顺序对动态数组中的元素进行排序。

​	sort() 方法的语法为：

```java
ArrayList<Integer> list = new ArrayList<Integer>();
arraylist.sort(Comparator c)；
list.sort(null);//默认自然序列排序
list.sort(Comparator.naturalOrder());//Java Comparator 接口的 naturalOrder() 方法指定元素以自然顺序（升序）排序
list.sort(Comparator.reverseOrder());//Comparator 接口的 reverseOrder() 方法指定以相反的顺序（降序）对元素进行排序

```

```java
//返回值
sort() 方法不返回任何值，它只是更改动态数组列表中元素的顺序。
```

 实例

以自然顺序排序，以下是字母的顺序：

 实例

```java
import java.util.ArrayList;
import java.util.Comparator;

class Main {
  public static void main(String[] args){

    // 创建一个动态数组
    ArrayList<String> sites = new ArrayList<>();
    
    sites.add("Runoob");
    sites.add("Google");
    sites.add("Wiki");
    sites.add("Taobao");
    System.out.println("网站列表: " + sites);

    System.out.println("不排序: " + sites);

    // 元素进行升序排列
    sites.sort(Comparator.naturalOrder());
    System.out.println("排序后: " + sites);
  }
}

```

执行以上程序输出结果为：

```
网站列表: [Runoob, Google, Wiki, Taobao]
不排序: [Runoob, Google, Wiki, Taobao]
排序后: [Google, Runoob, Taobao, Wiki]
```

在上面的实例中，我们使用了该 sort() 方法对名为 sites 的动态数组进行排序。

注意这一行：

```
sites.sort(Comparator.naturalOrder());
```

在此，Java Comparator 接口的 naturalOrder() 方法指定元素以自然顺序（升序）排序。

Comparator 接口还提供了对元素进行降序排列的方法：

## 十六，输入输出流

> System类中有两个静态的成员变量：
>
> - public static final InputStream in :标准输入流。通常该流对应于键盘输入或由主机环境或用户指定的另一个输出源
> - public static final PrintStream out :标准输出流。通常该流对应于显示输出或由主机环境或用户指定的另一个输出目标

1.标准输入流

自己实现键盘输入数据

- `BufferReader br = new BufferReader(new InputStreamReader(System.in))`

  原理：

  ```java
  public class StreamIn {
      public static void main(String[] args) {
  //        public static final InputStream in;标准输入流
          InputStream is = System.in;//创建一个输入流in，将接收的数据传入is
  
          InputStreamReader isr = new InputStreamReader(is);//用转换流InputStreamReader 把is字节流转换为字符流，但是该对象读取字符只能一次读取一个
  
          BufferedInputStream br = new BufferedInputStream(isr);//再将字符流由于创建BufferedInputStream对象，通过该对象可以利用其方法一次性读取一行数据
  
          BufferedInputStream br = new BufferedInputStream(new InputStreamReader(System.in));//综合起来就是这个
      }
  }
  
  ```

写起来太麻烦，java提供了一个类实现键盘录入

- `Scanner sc = new Scanner(System.in);` Scanner类也需要通过System.in来构造对象。

  此类提供了很多方便的方法用于录入不同数据类型

```java
 Scanner sc = new Scanner(System.in);
        int a = sc.nextInt();
        String b = sc.next();
```

2.标准输出流

输出语句的本质是一个标准输出流：

- ```java
   public class StreamIn {
      public static void main(String[] args) {
    
          PrintStream ps = System.out; //PrintStream对象ps可以被System.out取代
          ps.print("hello");
          ps.println("world");
    
          System.out.print("java");
          System.out.println();
      }
  }
  ```

  

3.Stream流

- 3.1体验Stream流

  > 需求:按照下面的要求完成集合的创建和遍历
  >
  > 创建一个集合，存储多个字符串元素
  > 把集合中所有以"张"开头的元素存储到一个新的集合
  >
  > 把"张"开头的集合中的长度为3的元素存储到一个新的集合
  >
  > 遍历上一步得到的集合

  使用Stream流的方式完成过滤操作

  `list.stream().filter(s-> s.startsWith("张")).filter(s-> s.length() == 3).forEach(System.outprintin);`直接阅读代码的字面意思即可完美展示无关逻辑方式的语义:生成流、过滤姓张、过滤长度为3、逐一打印Stream流把真正的函数式编程风格引入到java中

- 3.2 Stream流的生成方法

  ![image-20220926210240327](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20220926210240327.png)

  Stream流的使用

  > - 生成流
  >
  >   ​	通过数据源(集合,数组等)生成流list.stream0
  >
  > - 中间操作
  >
  >   ​	一个流后面可以跟随零个或多个中间操作，其目的主要是打开流，做出某种程度的数据过滤/映射，然后返回一个新的流,交给下一个操作使用
  >
  >   filter()
  >
  > - 终结操作
  >
  >   ​	一个流只能有一个终结操作，当这个操作执行后，流就被使用“光”了，无法再被操作。所以这必定是流的最后一个操作forEach（）,

```java
public class StreamDemo {

    public static void main(String[] args) {

        // ColLection体系的集合可以使用默认方法stream ()生成流List<String> list = new ArrayList<String>();
        Stream<String> liststream = list.stream();
        Set<String> set = new HashSet<String>();
        Stream<String> setStream = set.stream();
        //Map体系的集合间接的生成流
        Map<String, Integer> map = new HashMap<String，Integer > ();
        Stream<String> keyStream = map.keySet().stream();
        Stream<Integer> valueStream = map.values().stream();
        Stream<Map.Entry<String, Integer>> entryStream = map.entrySet().stream();
        //数组可以通过Stream接口的静态方法of (T... values)生成流
        String[] strArray = {"he1lo", "world", "java"};
        Stream<String> strArrayStream = Stream.of(strArray);
        stream<String> strArrayStream2 = Stream.of("hello", "wor1d", "java");
        Stream<Integer> intStream = Stream.of(10, 20, 30);
    }

```





























































## 实例

```java
import java.util.ArrayList;
import java.util.Comparator;

class Main {
  public static void main(String[] args){

    // 创建一个动态数组
    ArrayList<String> sites = new ArrayList<>();
    
    sites.add("Runoob");
    sites.add("Google");
    sites.add("Wiki");
    sites.add("Taobao");
    System.out.println("网站列表: " + sites);

    System.out.println("不排序: " + sites);

    // 降序
    sites.sort(Comparator.reverseOrder());
    System.out.println("降序排序: " + sites);
  }
}

```

执行以上程序输出结果为：

```
网站列表: [Runoob, Google, Wiki, Taobao]
不排序: [Runoob, Google, Wiki, Taobao]
降序排序: [Wiki, Taobao, Runoob, Google]
```

在上面的实例中，Comparator 接口的 reverseOrder() 方法指定以相反的顺序（降序）对元素进行排序。

### 1.Collection

#### 1.Collection集合概述和使用

Collection<E>(interface)

集合层次结构的根界面,集合表示一组称为元素的对象

JDK不提供此接口的任何实现,但它提供了更具体的子接口的实现,如Set和List

####  2.创建Collection集合的对象

​	1.通过多态的方法

​	2.具体实现类ArrayList

| ![image-20211211202816253](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211211202816253.png) |
| ------------------------------------------------------------ |

#### 3.Collection集合常用方法

| ![image-20211211203606798](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211211203606798.png) |
| ------------------------------------------------------------ |

| ![image-20211212081937913](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211212081937913.png) |
| ------------------------------------------------------------ |

#### 4.Collection集合的遍历

iterator: 迭代器,集合专门的遍历方式

​	1.Iterator<E> iterator(): 返回此集合中元素的迭代器,通过集合的iterator()方法得到

​	2.迭代器是通过集合的iterator()方法得到的,所以我们说它是依赖于集合而存在

iteartor中的常用方法

​	1.E.next(); 返回迭代器中的==下一个元素==

​	2.boolean hasNext(); 如果迭代器所指下一位仍有元素,则返回true

| ![image-20211212083644513](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211212083644513.png) |
| ------------------------------------------------------------ |

方法改进:

| ![image-20211212084110568](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211212084110568.png) |
| ------------------------------------------------------------ |

综上所述,集合的使用步骤为:

| ![image-20211212085054883](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211212085054883.png) |
| ------------------------------------------------------------ |

#### 5.创建学生类对象并存入ArrayList集合

```java
package arraylist;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;

public class CollectionDemo {
    public static void main(String[] args) {
        Collection<Student> c = new ArrayList<Student>();
            Student s1 = new Student("林青霞",30);
            Student s2 = new Student("张曼玉",35);
            Student s3 = new Student("王祖贤",33);

            c.add(s1);
            c.add(s2);
            c.add(s3);
            Iterator<Student> it = c.iterator();
            while(it.hasNext()){
                Student s = it.next();
                System.out.println(s.getName()+","+s.getAge());
            }
    }
}

```

| ![image-20211212100014843](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211212100014843.png) |
| ------------------------------------------------------------ |

#### 6.增强for循环与迭代器Iterator

增强for是用来遍历数组跟集合的,如下:

```java
public class ForEachTest {

	public static void main(String[] args) {
		doArray();
		doIterable();
	}
	
	// 使用foreach遍历数组
	public static void doArray() {
		String[] arr = new String[] {"A","B","C","D"};
		for (String str : arr) {
			System.out.println(str);
		}
        for(int i = 0; i < arr.length ; i++){
            String s = arr[i];
            sout(i);
        }
	}
	
	// 使用foreach遍历集合（Iterable对象）
	public static void doIterable() {
		Set set = new HashSet();
		set.add("A");
		set.add("B");
		set.add("C");
		set.add("D");
		for (Object obj : set) {
			System.out.println(obj);
		}
	}
}
```

看着好像没问题,但我们一起来揭晓其中的原理:

| ![preview](https://pic1.zhimg.com/v2-09848e7b3063eb9d37bc8711b128f080_r.jpg) |
| :----------------------------------------------------------- |

从上图我们可以知道:

==增强for操作数组，底层实现是普通for循环，通过下标取值。==

==增强for操作集合对象时，底层是使用了迭代器，而不涉及下标。==

但使用增强循环遍历来删除集合元素时会产生异常:(并发修改异常)

| ![preview](https://pic4.zhimg.com/v2-0b51fcb5ae5780f81f12c46575272fbf_r.jpg) |
| ------------------------------------------------------------ |

原因如下:             

| ![preview](https://pic3.zhimg.com/v2-aad362bb6678cb5c50c5dd99607c96ca_r.jpg) |
| ------------------------------------------------------------ |

### 3.List

List集合是一个子接口继承于Collection接口

```java
public interface List<E> extends Collection<E>
```

#### 1.List集合概述

​	1.List是一种有序集合(也被称为序列),用户可精确控制列表当中的每一个元素插入的位置,用户可通过整数序列访问元素(即含有索引)

​	2.与Set集合不同,List集合允许出现重复的元素

#### 2.List集合特有方法

| ![image-20211212101906694](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211212101906694.png) |
| ------------------------------------------------------------ |

==由于List集合存在索引,所以使用带索引的方法千万不要越界,否则会出 IndexOutOfBoundsException(索引越界异常)==

#### 3.List集合存储学生对象并遍历

```java
package list;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class ListDemo {
    public static void main(String[] args) {
        //创建List集合对象
        List<Student> list = new ArrayList<Student>();
        //创建学生类对象
        Student s1 = new Student("林青霞",30);
        Student s2 = new Student("张曼玉",35);
        Student s3 = new Student("王祖贤",33);
        //把学生添加到集合
        list.add(s1);
        list.add(s2);
        list.add(s3);
        //迭代器方式
        Iterator<Student> it = list.iterator();
        while(it.hasNext()){
            Student s = it.next();
            System.out.println(s.getName()+","+s.getAge());
        }
        System.out.println("-----------------------");
        //for循环方式遍历
        for (int i = 0; i < list.size(); i++) {
            Student s = list.get(i);
            System.out.println(s.getName()+","+s.getAge());
        }
    }
}

```

| ![image-20211212104440525](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211212104440525.png) |
| ------------------------------------------------------------ |

### 4.Set

​	Set与List一样继承于Collection接口,具有如HashSet和TreeSet等实现类

#### 1.HashSet

1.HashSet是Set的实现类之一,具有两个特征:

​	1.元素不允许重复

​	2.元素无序(不存在索引)

2.HashSet的常见方法:

```java
add(E e)//添加元素
remove(Object o)//删除元素
Contains(Object o)//查找元素
size()//取元素个数
iterator()//迭代器
```

3.根据HashSet元素不重复的特性,我们可以将ArrayList当中的元素放在HashSet当中去重再转为ArrayList,这样可以达到一个去重的目的,集合类型不变

| ![image-20211214203504401](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211214203504401.png) |
| ------------------------------------------------------------ |

| ![image-20211214203808133](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211214203808133.png) |
| ------------------------------------------------------------ |

4.产生20以内的随机数，由于要不重复，装进hashset对象中

```java
package demo01;

import java.util.HashSet;
import java.util.Random;
public class Text01 {
    public static void main(String[] args) {
        HashSet<Integer> set = new HashSet<Integer>();
        //产生20以内的随机数，由于要不重复，装进hashset对象中
        while(true){
            int num = new Random().nextInt(19)+1;
            set.add(num);
            if(set.size()==10){
                break;
            }
        }
        for(int x:set){
            System.out.print(x+"\t");
        }
    }
}

```

5.HashSet存储学生对象时,由于两个学生对象属性相同,但地址不同,HashSet集合并不会将它们认定为同一个元素,因此集合中会存在两个属性一样的学生对象,不符合HashSet不含重复元素的理念

| ![image-20211214204613575](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211214204613575.png) |
| ------------------------------------------------------------ |

为了避免这种情况,我们希望程序能够仅仅通过学号ID这一个属性来判断对象是否为同一个元素

因此我们可以在Student类当中右键源码覆写hashCode和equals方法，指定任一属性为判断依据字段,更符合实际

| ![image-20211214205002669](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211214205002669.png) |
| ------------------------------------------------------------ |

### 5.Map

Map:最大的键值对父接口

Map的实现类:

- HashMap:无序键值对,且键key不能重复,当key键重复时,会对值value进行覆盖
- TreeMap:有序键值对,且根据键的自然顺序进行排列

#### 1.HashMap:无序键值对

常见方法:

```java
put(K key,V value) //添加/覆盖元素
size() //集合大小
remove(Object key) //删除对应的键值
containsKey(Object key) //
containsValue(Object value)
get(Object key) //获取对应键的值
keySet() //
entrySet() //
```

基本用法:

例1:

| ![image-20211221201223641](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211221201223641.png) |
| ------------------------------------------------------------ |

```java
package demo02;

import java.util.HashMap;
import java.util.Map.Entry;
import java.util.Set;

public class Text01 {
    public static void main(String[] args) {
        HashMap<String,Integer> hm = new HashMap<String,Integer>();
        hm.put("zhangsan", 84);
        hm.put("lisi", 92);
        hm.put("wangwu", 80);
        System.out.println("键值对个数:"+hm.size());
        hm.put("wangwu", 78);//覆盖hm中原key="wangwu"的value值为78
        System.out.println(hm);
        System.out.println("========================");
        Set<String> keySet = hm.keySet();//利用hm当中的keySet方法创建一个Set集合(keySet)用于保存hm所有value值
        for (String key:keySet) {//遍历keySet集合当中的所有元素
            System.out.println(key+"-->"+hm.get(key));
        }
        System.out.println("========================");
        Set<Entry<String, Integer>> entrySet = hm.entrySet();//创建Set集合entrySet保存hm的value和key值
        for (Entry<String, Integer> entry:entrySet) {
            System.out.println(entry.getKey()+"-->"+entry.getValue());
        }
    }
}

```

| ![image-20211221200123148](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211221200123148.png) |
| ------------------------------------------------------------ |

#### 2.TreeMap:有序键值对

映射根据其键的自然顺序进行排序,或者根据自己实现Comparable 创建排序规则

常见方法类似于HashMap

例1:(String类型自然顺序排序)

```java
package demo02;

import java.util.TreeMap;

//TreeMap
public class Text03 {
    public static void main(String[] args) {
        TreeMap<String,Integer> tm = new TreeMap<String,Integer>();//由于String类型继承了Comparable<E>接口,TreeMap默认按String类型首字母排序
        tm.put("zhangsan",21);
        tm.put("lisi",18);
        tm.put("wangwu",19);
        tm.put("alex",17);
        System.out.println(tm);
        System.out.println(tm.firstKey());//输出排序后的第一个元素的key值
        System.out.println(tm.firstEntry());//输出排序后的第一个元素的key值和value值
    }
}

```

例2:创建Student类实现排序方法

```java
package demo02;

import java.util.TreeMap;

public class Text04 {
    public static void main(String[] args) {
        TreeMap<Student, Integer> tm = new TreeMap<Student, Integer>();
        Student stu1 = new Student("2001", "jack", 19);
        Student stu2 = new Student("2002", "rose", 21);
        Student stu3 = new Student("2003", "mike", 18);
        tm.put(stu1,63);
        tm.put(stu2,14);
        tm.put(stu3,35);//将Student类添加到TreeSet集合当中排序,但由于Student没有实现Comparable<E>接口,将出现ClassCastException(类型型转)异常
        System.out.println(tm);//因此要想要正确输出TreeMap排序后的Student元素,需要在Student类当中实现Comparable<E>接口并重写compareTo()方法来定义排序所需要依据的属性值,
    }
}

```

| ![image-20211221203207023](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211221203207023.png) |
| ------------------------------------------------------------ |
| ![image-20211221203213326](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211221203213326.png) |

例3:将HashMap通过TreeMap完成排序

​	1.显示words当中不同字母出现的次数

```java
mport java.util.HashMap;
import java.util.Map;
import java.util.Set;
import java.util.TreeMap;

public class Text02 {
    public static void main(String[] args) {
        String words = "dahuudalfjkmashflllkl";
        //显示words当中不同字母出现的次数
        HashMap<Character, Integer> hm = new HashMap<Character, Integer>();
        char[] chars = words.toCharArray();
        for (char c:chars) {
            if (hm.containsKey(c)) {
                hm.put(c,hm.get(c)+1);
            }else{
               hm.put(c,1);
            }
        }
        System.out.println(hm);
    }
```

| ![image-20211221200242919](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211221200242919.png) |
| ------------------------------------------------------------ |

进一步升级代码:要求将HashMap中统计的字母按照次数降序排序,次数相同按照字母升序

| ![image-20211221202051637](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211221202051637.png) |
| ------------------------------------------------------------ |
| ![image-20211221202237481](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211221202237481.png) |
| ![image-20211221202455154](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211221202455154.png) |

| ![image-20211221203637120](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211221203637120.png) |
| ------------------------------------------------------------ |

总代码:

```java
package demo02;

import java.util.HashMap;
import java.util.Map;
import java.util.Set;
import java.util.TreeMap;

public class Text02 {
    public static void main(String[] args) {
        String words = "dahuudalfjkmashflllkl";
        //显示words当中不同字母出现的次数
        HashMap<Character, Integer> hm = new HashMap<Character, Integer>();
        char[] chars = words.toCharArray();
        for (char c:chars) {
            if (hm.containsKey(c)) {
                hm.put(c,hm.get(c)+1);
            }else{
               hm.put(c,1);
            }
        }
        System.out.println(hm);
        //在HashMap代码优化:要求统计的字母按照次数降序,次数相同时按照字母升序
        TreeMap<CSortObj, Integer> tm = new TreeMap<>();//优化
        Set<Map.Entry<Character, Integer>> entrySet = hm.entrySet();
        for (Map.Entry<Character,Integer> entry:entrySet) {
            Character character = entry.getKey();
            Integer value = entry.getValue();
            CSortObj cso = new CSortObj(character.toString(),value);
            tm.put(cso,1);
        }
        System.out.println(tm);
    }
}

public class CSortObj implements Comparable<CSortObj> {
    private String zimu;
    private int cishu;

    public CSortObj() {
    }

    public CSortObj(String zimu, int cishu) {
        this.zimu = zimu;
        this.cishu = cishu;
    }

    public String getZimu() {
        return zimu;
    }

    public void setZimu(String zimu) {
        this.zimu = zimu;
    }

    public int getCishu() {
        return cishu;
    }

    public void setCishu(int cishu) {
        this.cishu = cishu;
    }

    @Override
    public String toString() {
        return "CSortObj{" +
                "zimu='" + zimu + '\'' +
                ", cishu=" + cishu +
                '}';
    }

    @Override
    public int compareTo(CSortObj o) {
        if (this.cishu == o.cishu) {
            return this.zimu.compareTo(o.zimu);
        } else {
            return this.cishu - o.cishu;
        }
    }
}

```

###  6.sort()排序方法

1.数组当中的sort方法：Arrays.sort()

​	按照数组内值的大小进行排序

```java
public class Test8 {
	public static void main(String[] args) {
		Car[] carList = new Car[3];
		carList[0] = new Car(30);
		carList[1] = new Car(10);
		carList[2] = new Car(55);
		Arrays.sort(carList);
		for(Car c:carList)
		{
			System.out.println(c.getSpeed());
		}
	}	
}
```

![img](https://img-blog.csdnimg.cn/20181210173749372.png)

按字符大小进行排序

```java
package lanqiaocup;

import java.lang.reflect.Array;
import java.util.Arrays;

public class Main {
	public static void main(String[] args) {
		String s = "adwfaawfg";
		char[] a = new char[s.length()];
		for (int i = 0; i < s.length(); i++) {
			a[i] = s.charAt(i);
		}
		for (char c : a) {
			System.out.print(c);
		}
		System.out.println();
		Arrays.sort(a);
		for (char c : a) {
			System.out.print(c);
		}
	}
}
```

2.集合当中的sort()排序

1. sort (List list)：根据元素的自然顺序对指定列表按升序进行排序。

```java
例如：直接调用Collections工具类中的sort方法，传入上面创建的list集合作为参数，然后输出，代码如下：

Collections.sort(list);

    System.out.println("sort自然排序：" + list);

```

结果如图所示：

![img](https://img-blog.csdnimg.cn/20190417185805628.png)

```java
1. sort(List list, Comparator c)：根据指定比较器产生的顺序对指定列表进行排序，也就是自定义排序，第一个参数是传入的要排序的列表，第二个参数是指定的比较器。

例如：将list列表按降序进行排序，代码如下：

首先创建一个比较器，在这个比较器中定义好排序规则（升序或降序），此处为降序排序：

Comparator<Integer> c = new Comparator<Integer>() {

      @Override

      *public* *int* compare(Integer o1, Integer o2) {

        // 升序排序 o1 - o2

        // 降序排序 o2 - o1

        *return* o2 - o1;

      }

};

然后将这个比较器作为一个参数传递进sort方法：

Collections.sort(list, c);

System.out.println("sort降序排序：" + list);

```



输出结果如下：

![img](https://img-blog.csdnimg.cn/20190417185805649.png)

​                

```java
也可以直接在调用sort方法时创建比较器，代码如下：

Collections.sort(list, *new* Comparator<Integer>() {

      @Override

      *public* *int* compare(Integer o1, Integer o2) {

        // 升序排序 o1 - o2

        // 降序排序 o2 - o1

        *return* o2 - o1;

      }

});

只是写法不同，输出结果一致，如图3所示！

```



## 十七,stream流

![image-20221110205926863](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221110205926863.png)

```c
  单列集合获取stream流：
        ArrayList<String> list = new ArrayList<>();
        Collections.addAll(list, "a", "b", "c", "d", "e");
//        Stream<String> stream1 = list.stream();
//        stream1.forEach(new Consumer<String>() {
//            @Override
//            public void accept(String s) {
//                System.out.println(s);
//            }
//        });
        
        list.stream().forEach(s -> System.out.println(s));//与上面代码效果一样，先获取list的stream()流，然后通过foreach输出每个字符s
    
```

```java
双列集合不能直接获取stream流
        HashMap<String, Integer> hm = new HashMap<>();
        hm.put("aaa", 111);
        hm.put("bbb", 222);
        hm.put("ccc", 333);
        hm.put("ddd", 444);

        //第一种获取stream流，keyset获取hashmap的key值并输出
		hm.keySet().stream().forEach(s -> System.out.println(s));

        //第二种获取stream流，entrySet获取hashmap的key和value的对象
        hm.entrySet().stream().forEach(s -> System.out.println(s));
输出：
    aaa
    ccc
    bbb
    ddd
    
    aaa=111
    ccc=333
    bbb=222
    ddd=444

```

