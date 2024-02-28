### 1，两种不同的输入输出

> 1.cin cout
>
> 头文件为`#include <iostream>`
>
> ```c++
> int a,b;
> cin >> a >> b;
> cout << a+b <<endl;//endl为换行的意思
> ```
>
> 2.printf 和 scanf
>
> 头文件`#include <cstdio>`
>
> ```c++
> int a;
> float b;
> scanf("%d%lf",&a,&b);//%d表示接收一个整数，%f为一个浮点数。 &表示取地址符
> printf("a*b=%f/n a+b=%f/n",a*b,a+b);//也可写为a*b=%.2f表示浮点数保留两位小数
> ```
>



### 2.不同类型输出格式

> int --> %d
>
> float --> %f
>
> char --> %c
>
> double --> %lf
>
> long long --> %lld



### 3.数据类型转换

默认转换顺序,两种类型进行运算默认转为精度更大的类型

> int 和 `float double`  //转为后面标红的类型
>
> char 和 `int`
>
> float 和 `double`
>
> int 和 `long`



### 4.printf几种输出格式（对整数，浮点数同样适用）

```java

int main() {
    int a = 1;
    int b = 23;
    int c = 456;
1.往前面补空格
    printf("%5d\n",a);//不足往前面补五个空格
    printf("%5d\n",b);
    printf("%5d\n",c);
    return 0;
}
输出：
    1
   23
  456
    
2.往后面补空格（加一个负号）
    printf("%-5d\n",a);//不足往后面补五个空格
    printf("%-5d\n",b);
    printf("%-5d\n",c);

输出：
1    
23   
456  
    
3.用0填满空格
    printf("%05d\n",a);
    printf("%05d\n",b);
    printf("%05d\n",c);

输出：
00001
00023
00456
    
4.浮点数
    printf("%05.1lf\n",f)//前面补0 保留一位小数的浮点数
```

5.转义字符：

`\+"对应符号"` 该对应字符就会表示成普通字符串不具备相应的语法功能。

###### 注意事项

> printf（）输出格式比cout输出格式稳定，如果对输出格式有要求最好选择printf()(cout输出之后会在前面加一个空格)

### 6.循环语句

###### 注意事项

> 解决c++整数除法计算精度损失问题：
>
> 首先先找到整数相除精度损失原因-->因为两个整数相除，得到的数必定是整数：`int a = 5; a/3 = 1`,因此我们有解决的办法就是将`除数`变为浮点数，即`a/3.0=1.6`
>
> 在进行变量相除时，需将被除数先转型成浮点数（一般建议转型成精度较高的double类型）即`int a = 5,b = 3; c = a/(double)b`

###### 注意事项

==含误差的大小判断==，因为浮点数保存的有效位数有限，在遇到一些长度大于浮点数有效位数的数字时会有误差，因此我们判断只要两个数相差在1e-6之内就令两数相等，即`|a-b|<=1e-6,a和b相等`,

> ```java
> using namespace std;
> const double eps = 1e-6;//定义差值10的负六次方
> int main() {
>     int a, b;
>     cin >> a, b;
>     if (abs(a - b) <= eps) puts("相等");
>     else if (a < b - eps) puts("小于");
>     else if (a  - eps > b) puts("大于");
>     return 0;
> }
> ```

> ```java
> using namespace std;
> const double eps = 1e-6;//定义差值10的负六次方
> int main() {
>     int a = 3;
>     if (fabs(sqrt(3) * sqrt(3)) < 3) puts("不相等");
>     if (fabs(sqrt(3) * sqrt(3) - 3) < eps) puts("相等");
> //fabs指的是浮点数绝对值
>     return 0;
> }
> 
> 如果不加入常量eps差值判断，我们甚至算出根号3的平方小于3，这就是精度损失。
> ```

### 7.数组

7.1一维数组初始化

![image-20221012191139545](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221012191139545.png)

###### 注意事项

> 当我们需要在函数内部定义一个非常大的数组时，因为数组内开辟的空间是在栈内存，而栈内存空间默认只有1M，因此我们选择在函数外定义一个全局数组，将数组存取在堆空间内。
>
> ```java
> using namespace std;
> int main() {
>  int a[1000000] = {0};
>  for (int i = 0; i < 1000000; ++i) {
>      a[i] = i;
>  }
>  return 0;
> }//系统报错
> ```
>
> ```java
> using namespace std;
> int a[1000000];//全局数组，可以不用加const
> 
> int main() {
> 
>  for (int i = 0; i < 1000000; ++i) {
>      a[i] = i;
>  }
>  return 0;
> }
> 全局数组内默认全部都是0；
> ```
>

7.2.数组倒序

```java
using namespace std;
int a[1000000];

int main() {
    int n, a[100];
    cin >> n;
    for (int i = 0; i < n; ++i) cin >> a[i];
    for (int i = n - 1; i >= 0; i--) cout << a[i] <<" ";

    return 0;
}
倒序输出数组中的元素
```

7.3数组旋转

旋转一次就是把末尾的数组移到第一位，多次重复旋转

```java
#include <iostream>
#include "cmath"

using namespace std;

int main() {
    int a[100];
    int n, k;
    cin >> n >> k;//数组大小为n，旋转K次
    for (int i = 0; i < n; ++i) {
        cin >> a[i];
    }
    while (k--) {
        int t = a[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            a[i + 1] = a[i];
        }
        a[0] = t;
    }
    for (int i = 0; i < n; ++i) {
        cout << a[i] << " ";
    }
    return 0;
}

5 3
1 2 3 4 5
3 4 5 1 2 
```

第二种翻转数组的写法（利用翻转函数reverse）

![image-20221012195151933](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221012195151933.png)

```c++
#include <iostream>
#include "cmath"
#include "algorithm"//翻转数组

using namespace std;

int main() {
    int a[100];
    int n, k;
    cin >> n >> k;//数组大小为n，旋转K次
    for (int i = 0; i < n; ++i) {
        cin >> a[i];
    }
    reverse(a, a + n);//这里的a指的是a[0],即a的首地址
    //先翻转整个数组a[0]~a[n]
    reverse(a, a + k);
    //再翻转前面需要的数组a[0]~a[k]
    reverse(a + k, a + n);
    //最后翻转a[k+1]~a[n]即剩余的数组
    for (int i = 0; i < n; ++i) {
        cout << a[i] << " ";
    }
    return 0;
}

```

7.4 二维数组初始化

```c++
#include <iostream>
#include <algorithm>

using namespace std;

int main()
{
    int b[3][4] = {         // 三个元素，每个元素都是大小为4的数组
        {0, 1, 2, 3},       // 第1行的初始值
        {4, 5, 6, 7},       // 第2行的初始值
        {8, 9, 10, 11}      // 第3行的初始值
    };

    return 0;
}
```

7.5 利用函数初始化数组

```c++
#include <iostream>
#include "cmath"
#include "cstring"//引入cstring库函数

using namespace std;

int main() {
    int a[10], b[10];

    memset(a, 0, sizeof a);
    //a指的是数组名称。
    // 0表示为每个字节赋值0，这样所有的数组单位都是0；要么为-1，这样所有的数组单位都初始化为-1，如果输入其他的数字则会赋奇怪的数字。
    // 这里sizeof=40 表示的是赋值的长度（单位byte）,sizeof 运算符返回一个数组的大小（单位byte），即一共需要10个数组空间，每个空间存放的是int类型占用4个字节，即为4*10=40
    
    for (int i = 0; i < 10; ++i) {
        cout << a[i] << " ";
    }
    return 0;
}

```

7.6 两种数组初始化时间对比

- 循环初始化数组(30ms)

  ```c++
  using namespace std;
  int a[1000000];
  
  int main() {
      for (int i = 0; i < 1000000; ++i) {
          a[i] = i;
      }
      return 0;
  }
  ```

- 函数初始化数组（16ms）

  ```c++
  #include <iostream>
  #include "cmath"
  #include "cstring"
  
  using namespace std;
  int a[1000000];
  
  int main() {
      memset(a, -1, sizeof a);//数组初始化为-1；
      return 0;
  }
  ```

> ==即用函数初始化数组更快==

> ###### 注意事项
>
> 数组两种常用的函数：
>
> `memset(a,0,sizeof)`;//初始化数组
>
> `memcpy(b,a,sizeof)`;//复制函数，将a数组值复制给b

7.7 c++常见math函数

```c
#include <math.h>

//平方 pow()
int a = pow(4,2);// 4的平方=16
//开方
int b = pow(4,0.5);// 4的平方根=2
int c = sqrt(4);// 4的平方根=2
//整数绝对值
int c = abs(b-c);
//浮点数绝对值
double d = fabs(b-c);
```

### 8.字符串

8.1 char字符

![image-20221015153246528](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221015153246528.png)

> 字符char在存储上本质上就是一个数字如'a' = 97;
>
> 只是计算机会在最终显示将存储的数字转化为char类型的字符。
>
> 我们可以将char类型之间相加减就很好地证明了这一点
>
> 如char a + 6= 'g'

> 字符串本质上就是存储一连串的字符最后加上"\n"，所以字符串的长度等于字符总长度+1（加一个字符的长度）

```c
  	char a1[] = {'C','+','+'};
    char a2[] = {'C','+','+','\n'};
    char a3[] = {"C++"};
a3与a2等值
    char a4[6] = "Daniel";//报错，数组越界
```

> 字符的输出：
>
> ​	
>
> ```c
> char a2[] = {'C','+','+','\n'};
> char a3[] = "ABCDE";
> 
> scanf("%c",a);//两种字符串输入都行，但需要注意的是用scanf（"%c",a）这个a不能用取地址符&,因为所有数组名字本身就是一个指针，因此不能将数组的地址改为输入的值；
> cout << a2+1 <<endl;//输出BC
> 
> printf("%s\n",a3+2);//输出CDEF 输入 在数组名称后+数字，表示从数组第几个位置开始存取，前面没赋值的位置默认为0
> puts(a2);//与如上printf等价，且包括换行
> 
> ```
>
> 因为数组本身就是一个指针，直接输出a3表示的是数组指针从第一个值'A'输出到最后一个值"\n"，a2+1表示指针向后移动一位输出到最后一个值；

> 字符串的输入：
>
> 输入字符时，遇到空格或者回车就会停止
>
> 输出字符时，遇到空格或者回车不会停止，遇到'\0'时停止
>
> ```c
>	char a[30];
> 	scanf("%c", a);//输入字符串时，遇到空格或者回车就会停止
>	printf("%c",a)//输出字符串时，遇到空格或者回车不会停止，遇到'\0'时停止
> ```



> ```c
>	char str[100];
> 
> 	scanf字符数组读法： fgets(str, 100, stdin);  // gets函数在新版C++中被移除了，因为不安全。
>                           // 可以用fgets代替，但注意fgets不会删除行末的回车字符
>    	cin字符数组读法：cin.getline(str,100);//读入字符到数组
>    ```

> 2.2 字符数组的常用操作
> 下面几个函数需要引入头文件:
>
> #include <string.h>
> (1) strlen(str)，求字符串的长度，不包含最后的'\n'的长度，求的就是字符串的实际长度 	
>
> ```c
> 可以利用strlen方法遍历数组
> 	char s1[100], s2[100];
>  scanf("%s", s1);
>  for (int i = 0; i < strlen(s1); ++i) {
>      cout << s1[i] << endl;
>  }
> ```
>
> 但因为strlen(str)本质上是一个循环，放在for循环内相当于两个循环嵌套，复杂度过高，因此我们可以将strlen定义在循环初始化部分
>
> ```c
> for (int i = 0, len = strlen(s1); i < len; ++i) {
>      cout << s1[i] << endl;
>  }
> 或者是：
> for (int i = 0; s1[i]; ++i) {//这里直接用s1[i]，当数组到底了即s1[i]就指向"\n"，在c++当中“\n”等于0，循环结束
>      cout << s1[i] << endl;
>  }
> ```
>
> (2) `strcmp(a, b)`，比较两个字符串的大小，a < b返回-1，a == b返回0，a > b返回1。这里的比较方式是字典序！
>
> - 字典序就是比较Ascll码的大小，从第一个开始一一比较，直到比较两个不同字符，Ascll码越大的字符串越大
>
> (3) `strcpy(a, b)`，将字符串b复制给从a开始的字符数组。后面复制给前面
>
> （4）`str.substr(i,len):` 取str字符串从i开始，到len长度的字符串，也可以省略len表示从i开始取到最后

8.2 标准库类型string

可变长的字符序列，比字符数组更加好用。需要引入头文件：

`#include <string>`

> 字符串的定义初始化：
>
> ​		
>
> ```c
> #include <iostream>
> #include <string>
> 
> using namespace std;
> 
> int main()
> {
>     string s1;              // 默认初始化，s1是一个空字符串
>     string s2 = s1;         // s2是s1的副本，注意s2只是与s1的值相同，并不指向同一段地址
>     string s3 = "hiya";     // s3是该字符串字面值的副本
>     string s4(10, 'c');     // s4的内容是 "cccccccccc"
> 
>     return 0;
> }
> ```
>
> 

string上的操作：

​		(1) string的读写

```c
string s1, s2;

 cin >> s1 >> s2;
 cout << s1 << s2 << endl;
	printf(“%s”, s.c_str());
	put(s.c_str());
```

​		注意：不能用printf直接输出string，需要写成：`printf(“%s”, s.c_str());`,且不能用scanf()来输入字符串

`s.c_str()`方法可以将字符串转化为char*const类型才能将字符串输出

​		(2)getline()，getline相比于cin读入字符串可以读取空格，而cin遇到空格停止输入；

​		`string的输入一行`为getline(cin,s1);

> getline(cin, s, ',');//遇到','停止读入，不写默认为'\n'换行
>
> getline()的原型是istream& getline ( istream &is , string &str , char delim );
> getline函数的第三个参数char delim表示遇到这个字符停止读入，通常系统默认该字符为’\n’，也可以自定义字符，这里就是自定义字符为逗号

​		`数组的输入一行`为cin.getline(str,len);



​		(3) string的empty和size操作（注意size是无符号整数，因此 s.size() <= -1一定成立）：

​		string当中的字符串长度函数str.size()时间复杂度为O(1),相比于字符数组strlen(str)时间复杂度是一个循环，str.size()几乎不占时间复杂度

​		

​		（4）string对象相加

​		当把string对象和字符字面值及字符串字面值混在一条语句中使用时，必须确保每个加法运算符的两侧的运算对象至少有一个是string，即两个相加至少要有一个string对象：

```c
string s4 = s1 + ", ";  // 正确：把一个string对象和有一个字面值相加
string s5 = "hello" + ", "; // 错误：两个运算对象都不是string

string s6 = s1 + ", " + "world";  // 正确，每个加法运算都有一个运算符是string
string s7 = "hello" + ", " + s2;  // 错误：不能把字面值直接相加，运算是从左到右进行的，先算左边两个非字符串对象相加会报错
```

​		（5）foreach循环

​		用于循环输出string对象

​		`		auto类型`，唯一确定的数字类型可用auto代替，若有多种类型计算机可能会报错 

```c
string s = "Hello World!";
 for (auto c: s) {//这里c显而易见是一个char类型，计算机确认它为char类型，auto可用
     cout << c << endl;
 }
```

​	如果利用foreach改变原字符串s的值

```c
string s = "Hello World!";
 for (auto c: s) {
 	c = 'A';
 }
  cout << s << endl;//输出 Hello World!
这样做是不可以改变s当中的值，因为c相当于是s每个字符的复制品，改变c当中的值不能改变s当中的值
此时我们需要用到指针取地址
 string s = "Hello World!";
 for (auto &c: s) {//这里相当于把S的地址存入c当中，改变c的值相当于也改变了s的值
 	c = 'A';
 }
  cout << s << endl;
输出：AAAAAAAAAAA
```

> string字符串比较
>
> ​		字符串大小比较比的是从左到右依次比较字符的字典顺序，直到字典顺序不相同则比较出大小关系。如果两个字符串长度不相同，其余字母全相同，那么长度长的字符串更大，相当于字符与空字符'\n'比较,`'\n'表示一个字符都没有，而空格字典顺序为32`；
>
> ```c
> 
>     if(a == b) cout << '=';
>     if(a > b) cout << '>';
>     if(a < b) cout << '<';
> 
> ```
>
> 

> \1. 字符数组
>
> 输入：<iostream>:
>
>  cin字符数组读入：
>
> cin.getline(str,100);//读入字符到数组
>
> <cstdio>
>
> scanf("%c", a);//输入字符串时，遇到空格或者回车就会停止
>
> scanf字符数组读法： fgets(str, 100, stdin);  // gets函数在新版C++中被移除了，因为不安全。
>
>  
>
> 输出：<iostream>:cout << a2+1 <<endl;//输出BC
>
> <cstdio>:printf("%s\n",a3+2);//输出CDEF 输入 在数组名称后+数字，表示从数组第几个位置开始存取，前面没赋值的位置默认为0
>
> 函数：puts(a2);//与printf等价，且包括换行
>
> \2. 字符串
>
> (1) 输入（字符串输入不能用scanf）
>
> ① cin >> s1 >> s2;
>
> getline()，getline相比于cin读入字符串可以读取空格，而cin遇到空格停止输入；
>
>  
>
> (2) 输出
>
> ① cout << s1 << s2 << endl;
>
> ​	printf(“%s”, s.c_str());
>
> ② put(s.c_str());
>
>  

### 9.函数

1.函数内`static`静态变量

> ​		在函数内定义一个静态变量static,无论该函数被调用多少次，静态变量只初始化一次。即在函数内部开辟一个只有该函数能使用的全局变量，且静态变量初始化为0。静态变量开在`堆内存`

2.参数传递

2.1 传值参数

> 当初始化一个非引用类型的变量时，初始值被拷贝给变量。此时，对变量的改动不会影响初始值。

```c
#include <iostream>

using namespace std;

int f(int x)
{
    x = 5;
}

int main()
{
    int x = 10;

    f(x);
    cout << x << endl;
    
    return 0;

}
```

2.2 传引用参数

> 当函数的形参为引用类型时，对形参的修改会影响实参的值。使用引用的作用：避免拷贝、让函数返回额外信息。

```c

#include <iostream>

using namespace std;

int f(int &x)
{
    x = 5;
}

int main()
{
    int x = 10;


f(x);
cout << x << endl;

return 0;


}
```

2.3 参数默认初始化

```c
如定义一个函数：函数参数传入都是从左到右，如果main方法传入参数不足，函数使用默认初始化的值b = 10；
void fool(int a, int b = 10){
	cout << a << ' ' << b << endl;
}
int main(){
    fool(5);//输出 5 10；如果传入了一个参数，系统赋值给函数变量a = 5,b为默认值10
    fool(5,6);//输出： 5 6；如果传入两个参数，系统会传入到a,b当中
}
```

> 注意事项：
>
> ​		函数内的数组指针差别：
>
> ​		如main函数内有一个数组a输出长度为10*4字节
>
> ​		而调用函数foo传入相同数组a，其长度却变成了8，这是为什么呢？
>
> ​		因为传入函数的数组其实并不是main函数中数组a本身，而是a的指针，在64位电脑中占八字节。
>
> ```c
> void foo(int b[]) {
>     cout << sizeof b << endl;
> }
> 
> int main() {
>     int a[10];
>     cout << sizeof a << endl;//输出40
>     foo(a);//输出8
>     return 0;
> }
> 
> ```
>
> 

2.4 内联函数inline

> ​		内联函数就是在函数定义前面加上一个inline关键字，表示这个函数为内联函数，内联函数不同于在main函数内跳转到其它函数，而是直接相当于将函数体copy到main函数内展开，运行速度会快一点，内联函数适用于短小且调用次数多的函数；
>
> ```c
> inline void foo(int b[]) {
>     cout << sizeof b << endl;
> }
> 
> int main() {
>     int a[10];
>     cout << sizeof a << endl;//输出40
>     foo(a);//输出8
>     return 0;
> }
> 等同于：
>     int main() {
>     int a[10];
>     cout << sizeof a << endl;//输出40
>     //foo(a);//输出8
>     cout << sizeof b << endl;相当于直接把foo函数复制进来；
>     return 0;
> }
> ```
>
> 

### 10.类，结构体，指针与引用

1.默认修饰符

> 类（class）的默认修饰符是`private`
>
> 结构体（struct）默认修饰符是`public`

2.构造函数初始化

```c
struct Person {
    int age, height;
    double money;

    Person() {}

    Person(int _age, int _height, double _money) : age(_age), height(_height), money(_money) {}
//快捷初始化
};
```

3.指针

![image-20221026105912027](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221026105912027.png)

> ​		当一个变量a在内存中开辟空间，我们可以让指针变量p内存入a变量的地址，当访问p时会自动指向a地址从而间接访问到a空间的内容。

```c
 	int a = 10; --定义一个int类型变量a赋值为10
    int *p = &a; --将a地址赋值给变量p，定义变量中int* 指的是为了区分该变量p为指针类型
    cout << *p << endl; 10 --*p表示p指针指向对应的地址变量a的值 类似于*p = a;
    *p = 12; --通过修改指针p对应的地址空间a的值，相应的a的值也会得到修改
    cout << p << endl; 0x8728fffd6c --p内存放的是a的地址
    cout << *p << endl; 12 --*p会通过p内存放的a地址找到并输出a的值
    cout << a << endl; 12
	cout << (void *) &p << endl;0xe9927ffd60 --输出内存地址
    cout << (void *) &a << endl;0xe9927ffd6c --c++当中两个连续定义的变量其内存地址一般也相邻 
```

> 数组名的含义：
>
> ​		输出数组名a，发现a与a[0]的地址是相同的，说明a既表示一个数组名称也表示数组的头数组a[0]的地址。
>
> ​		数组在内存当中是一个连续开辟的地址空间，当定义一个数组a，a数组后面连续的数组值a[i]都被定义了。其中a或a[0]当中包含了数组的长度size。

```c
int main() {
    int a[] = {1, 2, 3, 4, 5};//由于数组时int类型，其在内存当中相邻两个数组值地址差4个字节

    cout << a << endl;//a与a[0]地址相同
    for (int i = 0; i < 5; ++i) {
        cout << (void *) &a[i] << endl;
    }


    return 0;
}
0xeff7fffd90
0xeff7fffd90
0xeff7fffd94
0xeff7fffd98
0xeff7fffd9c
0xeff7fffda0
```



> C++引用别名
>
> ​		定义一个`int a = 10;int* p = &a;`
>
> ​		表示将p指针指向a的地址，c++提供了一个简单的写法 `int* p = &a等价于int& p = a;` 相当于给a取了一个别名p，使得p与a共用一个地址空间，这就是c++别名的用法，其底层是指针的应用。



> 指针的应用：`链表`
>

```c


#include <iostream>

using namespace std;

struct Node {
    int val;
    Node *next; //定义next的类型为Node*指针类型而不是Node类型，因为如果是Node类型就变成了递归会导致死循环，转化成指针类型就可以避免此类问题

    Node(int _val) : val(_val), next(NULL) {}
};

int main() {
    Node *p = new Node(1);
    Node *q = new Node(2);
    Node *o = new Node(3);

    p->next = q;//普通方法调用就是比如Person.getName();但是指针类型引用就是->,p指针类型方法调用p->next;
    q->next = o;

    Node *head = p;//定义头链表head=p    
    
遍历链表：
    for (Node *i = head; i != NULL; i = i->next) {//即当链表next值为空停止循环
        cout << i->val << endl;输出链表的val值
    }
    return 0;
}

1
2
3
	
```

添加链表：

![image-20221029103117210](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221029103117210.png)

```c
添加链表：
	因为链表的尾端下标不能够直接获取，因此我们一般都在头节点处添加链表；
    很简单：
    1.首先创建一个节点
    Node *r = new Node(4);
	2.然后将其next指向头结点
    r->next = head;
	3.将r变为头结点
    head = u;
```

删除链表：

![image-20221029103619311](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221029103619311.png)

> 链表的删除就是指在遍历时遍历不到这个点
>
> ​			`head->next = head->next->next;`

### 11.STL，位运算，常用库函数

1.vector

> vector是变长数组，支持随机访问，不支持在任意位置 O(1)O(1) 插入。为了保证效率，元素的增删一般应该在`末尾`进行。

```c
#include <vector>   // 头文件
vector<int> a;      // 相当于一个长度动态变化的int数组
vector<int> b[233]; // 相当于第一维长233，第二位长度动态变化的int数组，相当于一个二维数组。
struct rec{
    int x,y;
};
vector<rec> c;      // 自定义的结构体类型也可以保存在vector中
```

2.vector常用方法

> 1.size/empty
>
> ​		size函数返回vector的实际长度（包含的元素个数），empty函数返回一个bool类型，表明vector是否为空。二者的时间复杂度都是 O(1)O(1)。
> 所有的STL容器都支持这两个方法，含义也相同，之后我们就不再重复给出。
>
> 2.clear
>
> ​		clear()函数把vector清空。

3.迭代器iterator

> ​		迭代器it类似于STL容器里的指针，可以把它理解成数组里的数组下标，但是迭代器是指针，可以通过迭代器`*it`找到容器对应的值。
>
> 一个保存int的vector的迭代器声明方法为：`vector<int>::iterator it;`
>
> ​		可以把vector的迭代器与一个整数相加减，其行为和指针的移动类似。可以把vector的两个迭代器相减，其结果也和指针相减类似，得到两个迭代器对应下标之间的距离。
>
> `begin/end`
>
> ​		begin函数返回指向vector中`第一个元素的迭代器`。例如a是一个非空的vector，则*a.begin()与a[0]的作用相同。
>
> ​		所有的容器都可以视作一个`“前闭后开”`的结构，end函数返回`vector的尾部迭代器`，即第n 个元素再往后的“边界”。`*a.end()`与`a[n]`都是越界访问，其中n = a.size()。即`[a.begin,a.end)`
>
> `front/back`
> 		front函数返回vector的第一个`元素`，等价于a.begin()和a[0]。
> 		back函数返回vector的最后一个`元素`，等价于--a.end()和a[a.size() – 1]

```makefile
a.front()等价于a[0]等价于*a.begin()//因为a.begin()是一个迭代器，相当于容器的指针，需要在前面加*取其值
a.back()等价于a[a.size()-1]等价于*a.end()-1
```

> `push_back()和pop_back()`
>
> ​		a.push_back(x)把元素x插入到vector a的尾部。
> ​		b.pop_back()删除vector a的最后一个元素。

```c
迭代器的使用案例：
#include "iostream"
#include "vector"

using namespace std;

int main() {
    vector<int> a{{1, 2, 3}};
三种vector遍历方法
    1.循环遍历
    for (int i = 0; i < a.size(); ++i) {
        cout << a[i] << ' ';
    }
    cout << endl;
	2.迭代器遍历
    for (vector<int>::iterator i = a.begin(); i != a.end(); ++i) {//这里也可以写成for (auto i = a.begin(); i != a.end(); ++i)
        cout << *i << ' ';
    }
    cout << endl;
	3.增强循环遍历
    for (int x: a) {
        cout << x << ' ';
    }
    cout << endl;
    return 0;
}
```

2.queue队列

![image-20221029161005853](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221029161005853.png)

> 1.声明引用`#include<queue>`
>
> 2.queue头文件包含： 循环队列queue和优先队列priority_queue两个容器。

`循环队列queue`与`优先队列priority_queue`

> 循环队列queue遵循先入先出原则，类似于一个管道内存；
>
> 优先队列是一个闭环，优先输出优先值大的数；

```c
#include "iostream"
#include "vector"
#include"queue"

using namespace std;

int main() {
    queue<int> q; //定义一个循环队列
    priority_queue<int> a; //默认大根堆优先队列
    priority_queue<int, vector<int>, greater<int>> b;//小根堆优先队列

    struct Rec {
        int a, b;

        bool operator<(const Rec &t) const {//结构体内重载小于号
            return a < t.a;
        }
    };
    priority_queue<Rec> d;//定义结构体类型的大根堆队列需要在结构体内重载小于号
    d.push({1, 2});
    return 0;
}
```

> 定义结构体类型小根堆的话需要重载大于号：
>
> ```c
> struct Rec {
>         int a, b;
> 
>         bool operator>(const Rec &t) const {//结构体内重载小于号
>             return a > t.a;
>         }
>     };
>   
>     priority_queue<Rec,vector<Rec>,greater<Rec>> e;//定义结构体类型的小根堆队列需要在结构体内重载大于号
> ```

循环队列常用方法：`只能在对头插入，队尾弹出`

```c
queue<int> q; //创建队列

q.push(1);//队列头插入一个元素

q.pop();//弹出队尾元素

q.front();//返回队列头元素

q.back();//返回队尾元素

```

优先队列方法：

```c
priority_queue<int> a;//大根堆
a.push(1);//插入数
a.top();//取最大值
a.pop;//删除最大值

```

> 注意事项：
>
> ​		除了`循环队列，优先队列和栈没有clear()`函数，其他所有STL容器都有clear（）函数；

3.stack栈

```c
stack<int> stk; //定义栈
stk.push(1); //栈内插入一个元素
stk.top(); //返回栈顶数值
stk.pop(); //删除栈顶
```

4.双端队列deque:`相比于循环队列，其两边都可以插入和弹出`

```c
	deque<int> a;//定义双端队列
    a.begin(), a.end();//返回头尾deque迭代器
    a.front(), a.back();//返回头尾deque元素
    a.push_back(1), a.push_front(2);//从队尾、队头入队；

    a[0];//返回deque的一个随机值，0表示随机
    a.pop_back(), a.pop_front();//删除队尾、队头的值
```

5.set、multiset容器

```c
#include "set"

int main(){
	set<int> a; //set不允许重复
    multiset<int> b; //允许重复
    struct Rec {
        int x, y;

        bool operator<(const Rec &t) const {
            return x < t.x;
        }
    };
    set<Rec> c; //结构体rec中必须定义小于号
}
```



> ​		set容器`是一个有序容器`，里面的值自动按照顺序进行排列，因此才有upper_bound(x)，lower_bound(x)等利用二分法查找的方法。
>
> ​		set和multiset的迭代器称为“双向访问迭代器”，不支持“随机访问”，支持星号*解除引用，仅支持++和--两个与算术相关的操作。
>
> `设it是一个迭代器，例如set<int>::iterator it;`
>
> ​		若把it ++，则it会指向“下一个”元素。这里的“下一个”元素是指在元素从小到大排序的结果中，排在it下一名的元素。同理，若把it --，则it将会指向排在“上一个”的元素。

set,multiset函数

> 1.`s.begin()/s.end()` 返回收尾元素迭代器it
>
> 2.`s.insert(x)` 插入元素，在set中，若元素已存在，则不会重复插入该元素，对集合的状态无影响。
>
> 3.`s.find(x)` 在集合s中查找等于x的元素，并返回指向该元素的迭代器。若不存在，则返回s.end(),find()方法底层是利用`二分法查找`。
>
> 4.`s.lower_bound(x)`查找大于等于x的元素中最小的一个，并返回指向该元素的迭代器。
>
> ​	s.`upper_bound(x)`查找大于x的元素中最小的一个，并返回指向该元素的迭代器。
>
> 5.`s.erase(it)`从s中删除迭代器it指向的元素
>
> 6.`s.count(x)`返回集合s中等于x的元素个数，set不存在重复元素返回值只能为0或1；

```c
#include "set"

using namespace std;

int main() {

    set<int> a;
    multiset<int> b;

    set<int>::iterator it = a.begin();//创建迭代器
    it++;it--;
    ++it;--it;

    a.end();
    a.insert(x);//插入数值x
    if (a.find(x) == a.end()) //find(x)函数会查找set容器a内所有与x值相等的值并返回该值的迭代器，若不存在，则返回a.end()因此可以判断容器是否含有该值

//        两个函数的用法与find类似，但查找的条件略有不同，
    a.lower_bound(x);//找到大于等于x的最小元素的迭代器
    a.upper_bound(x);//找到大于x的最小元素的迭代器

    a.count(x);//返回集合a中等于x的元素个数,set容器只返回0或1，multiset存在重复值返回可以大于1
    a.erase(it);//设it是一个迭代器，s.erase(it)从s中删除迭代器it指向的元素
}
```

6.map容器

![image-20221102184251390](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221102184251390.png)

> ​		map容器是一个键值对key-value的映射，Map的key和value可以是任意类型，其中key必须定义小于号运算符。

```c
#include <map>
int main(){
	map<key_type, value_type> name;

	//例如：
	map<long long, bool> vis;
	map<string, int> hash;
	map<pair<int, int>, vector<int>> test;
}
```

map函数方法

> 1.`size/empty/clear/begin/end`均与set类似。
>
> 2.`insert/erase`与set类似，但其参数均是pair<key_type, value_type>。
>
> 3.`find(x)`h.find(x)在变量名为h的map中查找key为x的二元组。传入一个key返回一个迭代器
>
> 4.`[]操作符`h[key]返回key映射的value的引用
>
> []操作符是map最吸引人的地方。我们可以很方便地通过h[key]来得到key对应的value，还可以对h[key]进行赋值操作，改变key对应的value。
>
> 即通过[]操作符可以像操作数组一样操作map容器
>
> ```c
>  map<string, vector<int>> a;
>     a.insert({"a", {}});
>     a["yxc"] = vector<int>({1, 2, 3, 4});
>     cout << a["yxc"][2] << endl;
> 
> ```
>

位运算

![image-20221102185346731](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221102185346731.png)

> AND 与 &
>
> OR 或 |
>
> NOT 取反 ~
>
> XOR 异或 ^

移位运算

> `a>>右移`
>
> `a<<左移`
>
> ![image-20221102190012627](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221102190012627.png)

作用：

> 1.可以取二进制任意位置的数
>
> ![image-20221102190354154](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221102190354154.png)
>
> 先给二进制编号，如图取a的第2位数字，先将a>>2,然后再和1相与&1，得到该位置上的数字；
>
> 即取a的第K位置的数，a>>K&1即可；
>
> 2.取最后一位1的二进制数
>
> ![image-20221102190855409](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221102190855409.png)
>
> 即a=‘10101100100000’
>
> 取最后一个1出现的二进制数就是’100000‘
>
> 将a取反+1，(~a+1)=-a；在计算机二进制当中-a的存储方法就是(~a+1)
>
> 然后再将a与(&)-a得到结果

常用库函数

> 1.算法库：<algorithm>
>
> ​	①翻转函数reverse(); //翻转函数可以接收任何类型的数值
>
> ​		`reverse(a.begin(),a.end());`
>
> ​	②去重函数unique(); 
>
> ​		`unique(a.begin(),a.end());`
>
> ​		`unique(a.begin(),a.end())-a.begin();`//返回int类型重复出现元素的个数 。
>
> ​		③`random_shuffle` 随机打乱
>
> ​		`random_shuffle(a.begin(),a.end());` //用法与reverse()函数类似
>
> ```c
> 
> #include <iostream>
> #include "algorithm"
> #include "vector"
> #include "ctime"//引入时间变量需要导入库函数
> 
> using namespace std;
> 
> int main() {
>     vector<int> a({1, 2, 3, 4, 5});
>     srand(time(0));//随机打乱函数默认执行srand(0)方法，表示只打乱一次，刷新之后a不再改变
>     //因此我们要引入一个时间变量time(0)表示传入此时距19xx年的秒速，时间形参发生改变，从而每次刷新都会返回不一样的随机值；
>     random_shuffle(a.begin(), a.end());//返回随机打乱容器
> 
>     for (int x: a)cout << x << ' ';
>     cout << endl;
>     return 0;
> }
> 输出：1 3 5 4 2 
> 刷新 ：2 5 4 1 3
> ```
>
> ​		④`sort` 排序函数
>
> ​	按照从小到大的顺序返回：sort(a.begin(),a.end());
>
> ​	按照从大到小的顺序返回：sort(a.begin(),a.end(),greater<int>());//后面添加一个参数，重载从大到小顺序返回；
>
> ​	我们也可以自定义返回顺序：
>
> 自定义一个方法cmp,执行`sort(a.begin(),a.end(),cmp);`
>
> ```c
> bool cmp(int a, int b) { //a满足如下条件则应该排在b的前面
>     return a > b;//条件：从大到小
> }//从大到小排序
> 
>     bool cmp(int a, int b) { //a满足如下条件则应该排在b的前面
>     return a < b;//条件：从小到大
> }//自定义从小到大排序
> 	理解起来就是：sort()里两个参数a,b满足函数体内的条件（a>b或a<b），则a排在b的前面.
> ```
>
> 结构体定义一个方法实现sort自定义排序：
>
> ```c
> struct Rec {
>     int x, y;
> } a[5];
> 
> bool cmp(Rec a, Rec b) { //a满足如下条件则应该排在b的前面
>     return a.x < b.y;//从小到大
> }
> 
> 
> int main() {
>     for (int i = 0; i < 5; ++i) {
>         a[i].x = -i;
>         a[i].y = i;
>     }
>     sort(a, a + 5, cmp);
>     for (int i = 0; i < 5; ++i) {
>         printf("(%d,%d)", a[i].x, a[i].y);
>     }
>     cout << endl;
>     return 0;
> }
> ```
>
> 输出：
>
> ![image-20221102201743714](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221102201743714.png)
>
> 也可以不用写cmp重载函数，直接在结构体当中重载
>
> ```c
> struct Rec {
>     int x, y;
> 
>     bool operator<(const Rec &t) const {
>         return x < t.x;
>     }
> } a[5];
> ```
>
> ​		⑤ `lower_bound/upper_bound`  二分
>
> ​	`lower_bound(a.begin(),a.end().b);`返回有序容器（数组也可）大于等于b的数值；
>
> ​	`upper_bound(a.begin(),a.end().b);`返回有序容器（数组也可）大于b的数值；
>
> ```c
> int i = lower_bound(a + 1, a + 1 + n, x) - a;//不返回数值，而是返回一个大于等于x的数组下标或迭代器
> ```
>
> 
