## 1.九九乘法表

代码图：

```java
package day0306;

public class Demo01 {
    public static void main(String[] args) {
        for (int i = 1; i <= 9; i++) {
            for (int j = 1; j <= i; j++) {
                System.out.print(i+"*"+j+"="+(i*j)+" ");
            }
            System.out.println();
        }
    }
}
```

效果图：

```

1*1=1 
2*1=2 2*2=4 
3*1=3 3*2=6 3*3=9 
4*1=4 4*2=8 4*3=12 4*4=16 
5*1=5 5*2=10 5*3=15 5*4=20 5*5=25 
6*1=6 6*2=12 6*3=18 6*4=24 6*5=30 6*6=36 
7*1=7 7*2=14 7*3=21 7*4=28 7*5=35 7*6=42 7*7=49 
8*1=8 8*2=16 8*3=24 8*4=32 8*5=40 8*6=48 8*7=56 8*8=64 
9*1=9 9*2=18 9*3=27 9*4=36 9*5=45 9*6=54 9*7=63 9*8=72 9*9=81 
```



## 2.实现交替式发言

（如何实现交替式发言）

![image-20211016165010260](C:/Users/HP/AppData/Roaming/Typora/typora-user-images/image-20211016165010260.png)

运行结果：

![image-20211016165156091](C:/Users/HP/AppData/Roaming/Typora/typora-user-images/image-20211016165156091.png)

![image-20211019163808681](C:/Users/HP/AppData/Roaming/Typora/typora-user-images/image-20211019163808681.png)

## 3.输出101~200之间的质数

​		判断101-200之间有多少个素数，并输出所有素数。 素数又叫质数，就是除了1和它本身之外，再也没有整数能被它整除的数。也就是素数只有两个因子。

```java
public class Text$01 {

	public static void main(String[] args) {
		int i,j,n,m,x;//n用来存取余数；m是用来统计具体一个数的因子；
		n = 0; m = 0; x = 0;//x是用来统计101~200之间素数的个数
		for(i = 101; i <= 200; i++) {
			for(j = 1; j<=i; j++) {//双重循环
				n = i%j;
				if(n == 0) {//判断余数为零，因子加一
					m = m + 1;
				}
			}
			if(m==2) {//当因子只有两个，那么该数为质数
				System.out.println(i+" ");//输出质数
				x = x+1;
			}
			m = 0;//使用完记得清零
		} 
		System.out.println();
		System.out.println("在101~200之间一共有素数"+x+"个");
	}

}
```

4.

## 4.Java中输入3个整数并且实现按从小到大排序输出

1.if循环死方法(一个大循环if-else内嵌一个if-else-if-else法)

```java
import java.util.Scanner;
public class demo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("a=");
        int a = sc.nextInt();
        System.out.print("b=");
        int b = sc.nextInt();
        System.out.print("c=");
        int c = sc.nextInt();
        if (a > b) {                      //a>b的情况 a较大
            if (c > a) {                  //c>a 已知a>b c最大
                System.out.println(b + "," + a + "," + c);
            } else if (c < b) {  //c<b 已知a>b 
                System.out.println(c + "," + b + "," + a);
            } else {
                System.out.println(b + "," + c + "," + a);
            }
            // a<b情况
        } else {
            if (c < a) {                  //a<b c<a
                System.out.println(c + "," + a + "," + b);
            } else if (c > b) {           //a<b c>b
                System.out.println(a + "," + b + "," + c);
            } else {
                System.out.println(a + "," + c + "," + b);
            }
        }
    }
}
```

2.大中小逐个求法（三个if-else-if-else）

```java
import java.util.Scanner;
public class test1 {
    public static void main(String[] args) {
        Scanner input=new Scanner(System.in);
        System.out.print("Enter a:");
        int a=input.nextInt();
        System.out.print("Enter b:");
        int b=input.nextInt();
        System.out.print("Enter c:"); //输入三个数
        int c=input.nextInt();
        int max =0;
        int mid=0;
        int min=0;
        if (a>b & a>c){max=a;}else if(b>a & b>c){  //暴力列举出最大数情况
            max=b;
        }else{
            max=c;
        }
        if(a<b & a<c ){min=a;}else if (b<a & b<c){  //列举出最小数所有情况
            min=b;
        }else{
            min=c;
        }
        if(a<b & a>c || a<c &a>c){mid=a;}else if(b<a & b>c || b>a & b<c){mid=b;}else{mid=c;} //列举出中间值情况
        System.out.println(min+" "+mid+" "+max);
    }
}
```

3.容器互换法（定义一个容器temp)（三个if并列法）

```java
import java.util.Scanner;
public class demo {
    public static void main(String[] args) {
        Scanner s = new Scanner(System.in);
        System.out.println("Enter");
        int a = s.nextInt();
        int b = s.nextInt();
        int c = s.nextInt();
        if (a < b) {
            int t = a;  /*如果a<b时候 a、b数值互换*/
            a = b;
            b = t;
        }
        if (a < c) {    /*如果a<c时候 a、c数值互换*/
            int t = a;
            a = c;
            c = t;
        }
        if (b < c) {    /*如果b<c时候 b、c数值互换*/
            int t = b;
            b = c;
            c = t;
        }
        System.out.println("");
        System.out.println(c + " " + b + " " + a);
    }
}
```

## 5.打印图形

### 1.打印等腰三角形(含解析)

```java
public class PrintDY {
    public static void main(String[] args) {
        for (int i = 1; i <= 5; i++) {
             //外层循环；执行五次，控制图形有多少行
            for(int j=1;j<=5-i;j++){
             //内层循环1；控制打印空格，且不能换行
                System.out.print(" ");
            }
            for (int j = 1;j<=2*i-1;j++){
             //内层循环2；控制打印*，且不能换行
             //两个内层循环当中的j为循环内的局部变量，一旦脱离了创造它的循环，内存及释放，因此两者不为同一变量。
             //易错 j<=2*i-1而不是+1
                System.out.print("*");
            }            
            //输出换行符；在外层循环与内层循环之间
            System.out.println();
        }
       
        
    }
}

```

代码图：

```java
	*
   ***
  *****
 *******
*********
```
### 2.打印菱形

```java
import java.util.Scanner;

class Main {
    public static void main(String[] args) {
        Scanner cin = new Scanner(System.in);

        int n = cin.nextInt();//输出等腰三角形边长为n
        for (int i = 1; i <= n; i++) {//菱形上半部分为等腰三角形
            for (int j = 1; j <= n - i; j++) {
                System.out.print(" ");
            }
            for (int j = 1; j <= (i - 1) * 2 + 1; j++) {
                System.out.print("*");
            }
            System.out.println();
        }
        for (int i = 1; i <= n - 1; i++) {//下半部分为道三角形
            for (int j = 1; j <= i; j++) {
                System.out.print(" ");
            }
            //(n*2)-1
            for (int j = 1; j <= 2 * n - 1 - i * 2; j++) {
                System.out.print("*");
            }
            System.out.println();
        }
    }

}
```

代码图：

```JAVA
  	*
   ***
  *****
 *******
*********
 *******
  *****
   ***
    *

```

### 3.打印直角三角形（特殊）

```java
public class Text02 {

	public static void main(String[] args) {
		for(int i = 1; i <= 4;i++) {
			for(int j = 1; j <= 2*i-1;j++) {
				System.out.print("*");
			}
			for(int j = 1; j<=9-(5-i)*2; j++) {
				System.out.print(" ");
			}
			System.out.println();
		}
		
	}

}

/*代码图：
    
* 
***   
*****     
*******
```

4.打印平行四边形

```JAVA
public class PrintParallelogram {
    public static void main(String[] args) {
       
        for (int i = 1; i <= 5; i++) {
            for (int j = 1; j <= 5 - i; j++) {
                System.out.print(" ");
            }
            
            for (int j = 1;j<=5;j++){
                System.out.print("*");
            }
            System.out.println();
        }
    }
}

/*代码图：

 	*****
   *****
  *****
 *****
*****
```



## 6.数组赋值循环练习

```java
import java.util.Scanner;
//5、黄黄去参加青年歌手大奖赛,有10个评委打分,(去掉一个最高一个最低)求平均分？利用数组实现评委打分录入存储，然后数组排序后去掉最高分最低分，计算显示平均分。
public class Text05 {

	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		int[] score = new int[10];
		int sum = 0;
		for(int i = 0; i<score.length; i++) {
			System.out.println("请输入第"+(i+1)+"位评委打分：");//因为数组i初始值必须赋0，目															//的是让score[0]赋到值
			int a = scan.nextInt();
			if(a<0 || a>100) {
				System.out.println("您输入的数据有误！请重新输入：");
				i--;
				continue;//continue后面不能接语句，否则系统会报错。
			}
			score[i] = a;
			sum = sum + a;
		}
		int max = score[0];
		int min = score[0];
		for(int i = 0; i<score.length; i++) {
            //找出数组的最大值与最小值
			if(max < score[i]) {
				max = score[i];
			}else if(min > score[i]) {
				min = score[i];
			}
		}
		sum = sum-min-max;
		int avgScore = sum/(score.length-2);
		System.out.println("该选手的平均成绩为："+avgScore);
	}

}


/*

请输入第1位评委打分：
1
请输入第2位评委打分：
2
请输入第3位评委打分：
3
请输入第4位评委打分：
4
请输入第5位评委打分：
5
请输入第6位评委打分：
6
请输入第7位评委打分：
8
请输入第8位评委打分：
9
请输入第9位评委打分：
7
请输入第10位评委打分：
10
该选手的平均成绩为：5

```

## 7.杨辉三角（二维数组练习）

```java
public class Textyanghui {
	public static void main(String[] args) {
		int[][] arr = new int[10][10];
        for (int i = 0; i < arr.length; i++) {
            arr[i][0] = 1;
        }
        for (int i = 1; i < arr.length; i++) {
            for (int j = 1; j < arr.length; j++) {
                arr[i][j]=arr[i-1][j-1]+arr[i-1][j];
            }
        }
        for (int i = 0; i < arr.length ; i++) {
            for (int k = arr.length-i;k>0;k--) {
            }

            for (int j = 0; j < arr.length; j++) {

            if (arr[i][j]!=0) {

                System.out.print(arr[i][j]+"   ");}
            }
            System.out.println();
        }

	}
}

/*
1   
1   1   
1   2   1   
1   3   3   1   
1   4   6   4   1   
1   5   10   10   5   1   
1   6   15   20   15   6   1   
1   7   21   35   35   21   7   1   
1   8   28   56   70   56   28   8   1   
1   9   36   84   126   126   84   36   9   1   

```

## 8.数组练习

1、从键盘读入学生成绩，找出最高分，并输出学生成绩等级。（①使用Arrays当中的copyOF方法保存数组 copyOF也可作为数组扩容）
成绩>=最高分-10       等级为‘A’ 
成绩>=最高分-20       等级为‘B’
成绩>=最高分-30       等级为‘C’
其余                           等级为'D'

```java
import java.util.Arrays;
import java.util.Scanner;

public class ArrayDemo01 {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("请输入学生人数:");
		int stuNum = scan.nextInt();
		//根据之前录入的学生人数，声明数组开辟对应大小空间
		int[] scores = new int[stuNum];
		System.out.println("请录入"+stuNum+"个成绩：");
		for(int i=0;i<stuNum;i++){
			scores[i] = scan.nextInt();
		}
		//for循环得到所有成绩，都存储在数组 scores中
		//通过 Arrays类中的sort()方法排序，排序后取最高分
		//查阅API：static void sort(int[] a) 对指定的 int 型数组按数字升序进行排序。 
		//在排序前将原本的顺序数组值保存下来，一遍后面要用
		int[] newScores = Arrays.copyOf(scores, scores.length);//复制原数组，将原数组顺序保存在新建数组newScores内。
		//int[] newScores = scores;//为什么这样不能完成复制数组
		Arrays.sort(scores);//将原数组进行升序排序，最高分为最后一个元素
		System.out.println("最高分为:"+scores[stuNum-1]);
		System.out.println("======================");
		for(int i=0;i<stuNum;i++){
			String grade = null;
			if(newScores[i]>scores[stuNum-1]-10){
				grade = "A";
			}else if(newScores[i]>scores[stuNum-1]-20){
				grade = "B";
			}else if(newScores[i]>scores[stuNum-1]-30){
				grade = "C";
			}else{
				grade = "D";
			}
			System.out.println("第"+(i+1)+"个学生成绩为:"+newScores[i]+" grade is"+grade);
		}
	}
}
/*
请输入学生人数:
5
请录入5个成绩：
99
86
42
75
38
最高分为:99
======================
第1个学生成绩为:99 grade isA
第2个学生成绩为:86 grade isB
第3个学生成绩为:42 grade isD
第4个学生成绩为:75 grade isC
第5个学生成绩为:38 grade isD

```

2.编写方法，利用数组实现学生的姓名排序 (按字母顺序)(使用Arrays当中的sort方法进行排列)

```java
package day1030;

import java.util.Arrays;
import java.util.Scanner;
public class ArrayDemo02 {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("请一次输入5个学生的姓名:");
		//声明准备好一个数组接受输入的数据
		String[] names = new String[5];
		//for循环录入姓名
		for(int i=0;i<5;i++){
			names[i] = scan.next();//输入姓名赋值存储到数组元素
		}
		System.out.println("======排序后=======");
		//利用Arrays类中的sort()方法进行排序
		Arrays.sort(names);
		//打印显示排序后的姓名，验证一下
		for(String name:names){
			System.out.print(name+"\t");
		}
	}
}
/*
请一次输入5个学生的姓名:
jack
rose
bob
tom
lucy
======排序后=======
bob	jack	lucy	rose	tom	
```

3.调用方法求数组的平均值

```java
public class AvgArray {
	public static void main(String[] args) {
		int[] arr = {1,2,3,4};
		System.out.println(avg(arr));
	}
	public static double avg(int[] arr) {
	//实现一个方法avg，以数组为参数，求数组中所有元素的平均值（注意方法的返回值类型）。
	double sum = 0;
	for(int value : arr) {
		sum += value;
	}
	return sum/arr.length;
}
}
```

4.利用数组求最值

```java
public class ArrayDemo04 {
    public static void main(String[] args) {
        int max = 0;
        int[] arr4 = new int[]{44, 29, 98, 75};
        for (int i = 1; i < arr4.length; i++) {
            max = arr4[0];
            if (arr4[i] > max) {
                max = arr4[i];
            }
        }
        System.out.println(max);
    }
}
运行结果：
    7
```

5.反转数组元素各值

```java
//定义一个方法使数组各个元素倒置
public class Demo03 {
    public static void main(String[] args) {
        int[] arr = new int[]{19, 28, 37, 46, 50};
      	reserve(arr);
        plantArray(arr);
    }
    //返回值类型：void 因为数组为一个引用形参类型，调用方法直接影响数组值，而不需要返回值
    //参数:int[] arr
    //反转数组方法
    public static void reserve(int[] arr){
        for (int start = 0, end = arr.length-1; start <= end; start++, end--) {//利用for循环遍历数组，定义一个头数组元素和尾数组元素，定义一个临时空间temp用来使头元素和尾元素交换，交换完毕后头元素-1，尾元素+1，且头元素不大于尾元素。
            int temp = arr[start];
            arr[start] = arr[end];
            arr[end] = temp;
        }
    }

    //返回值类型：void
    //参数:int[] arr
    //打印数组方法
    public static void plantArray(int[] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            if (i == arr.length - 1) {
                System.out.print(arr[i]);
            } else {
                System.out.print(arr[i] + ",");
            }
        }
        System.out.print("]");
    }
}
运行结果：
    [50,46,37,28,19]
```



## 9.类和对象

### 1.类和对象的创建和引用

```java
import java.util.Scanner;
public class Demoproject03 {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		int java,C,DB;
		ScoreCalc score = new ScoreCalc();
		System.out.println("请该同学的输入java的成绩：");
		java = scan.nextInt();
		System.out.println("请输入C的成绩：");
		C = scan.nextInt();
		System.out.println("请输入DB的成绩：");
		DB = scan.nextInt();
		score.avgScore();
		System.out.println("该同学总成绩是："+score.sumScore(java, DB, C));
		System.out.println("平均成绩是："+score.avgScore());
		}
	}

class ScoreCalc{
	//属性；
	public int sum;
	//方法；
	public int sumScore(int a,int b,int c) {//调用此方法自动给类方法属性sum赋值
        								//方法参数列表可以不用定义，但方法内的其他属性需先定义
		sum = a+b+c;
		return sum;//如sum属性,需在类当中定义 而a,b,c不需在方法内定义
	}
	public double avgScore() {
		return sum/3;
	}
}

/*
请该同学的输入java的成绩：
85
请输入C的成绩：
90
请输入DB的成绩：
100
该同学总成绩是：275
平均成绩是：91.0

```

## 10.ArrayList集合经典代码

1.存储学生信息并遍历

```java


import java.util.ArrayList;
import java.util.Scanner;

public class ArrayListTest01 {
    public static void main(String[] args) {
        ArrayList<Student> array = new ArrayList<Student>();
        addStudent(array);
    }

    /*  定义方法addStudent
      返回值类型:void
      参数:ArrayList<Student> array*/
    public static void addStudent(ArrayList<Student> array) {
        Scanner scan = new Scanner(System.in);
        System.out.println("请输入学生姓名:");
        String name = scan.nextLine();
        System.out.println("请输入学生年龄:");
        int age = scan.nextInt();
        Student s = new Student();
        s.setName(name);
        s.setAge(age);
        array.add(s);
        System.out.println("学生姓名:" + s.getName() + "学生年龄:" + s.getAge());
    }
}

//学生类
public class Student {
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.age = age;
        this.name = name;
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
}

```

## 11.自制学生管理系统

```java
public class Student {//定义一个学生类
    private String Sid;
    private String name;
    private int age;
    private String address;

    public Student() {
    }

    public Student(String Sid, String name, int age, String address) {
        this.Sid = Sid;
        this.name = name;
        this.age = age;
        this.address = address;
    }

    public String getSid() {
        return Sid;
    }

    public void setSid(String Sid) {
        this.Sid = Sid;
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

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}

import java.util.ArrayList;//主界面测试类
import java.util.Scanner;

public class StudentManager {
    public static void main(String[] args) {
        ArrayList<Student> array = new ArrayList<Student>();
        boolean flag = true;
        while (flag) {
            System.out.println("----------欢迎来到学生管理系统----------");
            System.out.println("1.添加学生");
            System.out.println("2.删除学生");
            System.out.println("3.修改学生");
            System.out.println("4.查看所有学生");
            System.out.println("5.退出");
            System.out.println("请输入你的选择:");
            Scanner scan = new Scanner(System.in);
            String line = scan.nextLine();
            switch (line) {
                case "1":
//                    System.out.println("添加学生");
                    addStudent(array);
                    break;
                case "2":
//                    System.out.println("删除学生");
                    deleteStudent(array);
                    break;
                case "3":
//                    System.out.println("修改学生");
                    updateStudent(array);
                    break;
                case "4":
//                    System.out.println("查看所有学生");
                    findAllStudent(array);
                    break;
                case "5":
                    System.out.println("谢谢使用");
                    flag = false;
//                    System.exit(0);//退出java虚拟机
                    break;
            }
        }
    }

    public static void addStudent(ArrayList<Student> array) {
        Scanner scan = new Scanner(System.in);
        String sid;
        while(true){
        System.out.println("请输入学生学号:");
        sid = scan.nextLine();
        if (isUsed(array,sid)== true) {
            System.out.println("您输入的学号已存在,请重新输入");
        }else{
            break;
            }
        }
        System.out.println("请输入学生姓名:");
        String name = scan.nextLine();
        System.out.println("请输入学生年龄:");
        int age = scan.nextInt();
        System.out.println("请输入学生居住地:");
//        String addres = scan.nextLine();不能用nextline(),打回车键会自动录入在nextline当中;
        String address = scan.next();
        Student s = new Student();
        s.setSid(sid);
        s.setName(name);
        s.setAge(age);
        s.setAddress(address);
        array.add(s);
        System.out.println("添加学生成功");


    }

    public static void findAllStudent(ArrayList<Student> array) {
        if (array.size() == 0) {
            System.out.println("无信息,请先添加信息再查询");
            return;//为了让程序不再执行return;
        }
        System.out.println("学号\t\t姓名\t\t年龄\t\t居住地");
        for (int i = 0; i < array.size(); i++) {
            Student s = array.get(i);
            System.out.println(s.getSid() + "\t" + s.getName() + "\t\t" + s.getAge() + "岁\t\t" + s.getAddress());
        }
    }

    public static void deleteStudent(ArrayList<Student> array) {

        // 创建键盘录入对象
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入你要删除的学生的学号：");
        String Sid = sc.nextLine();
        // 我们必须给出学号不存在的时候的提示
        // 定义一个索引
        // 遍历集合
        int index = -1;
        for (int i = 0; i < array.size(); i++) {
            // 获取到每一个学生对象
            Student s = array.get(i);
            // 拿这个学生对象的学号和键盘录入的学号进行比较
            if (s.getSid().equals(Sid)) {
                index = i;
                break;
            }
        }
        if (index == -1) {
            System.out.println("该信息不存在,请重新输入");
        }else{
            array.remove(index);
            System.out.println("删除学生成功");
        }
    }

    public static void updateStudent(ArrayList<Student> array){
        Scanner scan =  new Scanner(System.in);
        Student s = new Student();
        System.out.println("请输入你要修改的学生的学号:");
        String sid = scan.nextLine();
        int index = -1;
        for (int i = 0; i <array.size() ; i++) {
            Student student = array.get(i);
            if (student.getSid().equals(sid)) {
                array.set(i,s);
                index = i;
                break;
            }
        }
        if (index == -1) {
            System.out.println("该信息不存在,请重新输入");
            return;
        }
        System.out.println("请输入学生新姓名:");
        String name = scan.nextLine();
        System.out.println("请输入学生新年龄:");
        int age = scan.nextInt();
        System.out.println("请输入学生新居住地:");
        String address = scan.next();

        s.setSid(sid);
        s.setName(name);
        s.setAge(age);
        s.setAddress(address);
        System.out.println("修改学生成功");
    }

    public static boolean isUsed(ArrayList<Student> array, String sid){
        boolean flag = false;
        for (int i = 0; i <array.size() ; i++) {
            Student s = array.get(i);
            if (s.getSid().equals(sid)) {
                flag = true;
                break;
            }
        }
        return flag;
    }
}

运行结果:

----------欢迎来到学生管理系统----------
1.添加学生
2.删除学生
3.修改学生
4.查看所有学生
5.退出
请输入你的选择:
1
请输入学生学号:
2020370227
请输入学生姓名:
邱文宣
请输入学生年龄:
20
请输入学生居住地:
江西抚州
添加学生成功
----------欢迎来到学生管理系统----------
1.添加学生
2.删除学生
3.修改学生
4.查看所有学生
5.退出
请输入你的选择:
1
请输入学生学号:
2020370228
请输入学生姓名:
汪忠辉
请输入学生年龄:
20
请输入学生居住地:
江西上饶
添加学生成功
----------欢迎来到学生管理系统----------
1.添加学生
2.删除学生
3.修改学生
4.查看所有学生
5.退出
请输入你的选择:
3
请输入你要修改的学生的学号:
2020370226
该信息不存在,请重新输入
----------欢迎来到学生管理系统----------
1.添加学生
2.删除学生
3.修改学生
4.查看所有学生
5.退出
请输入你的选择:
1
请输入学生学号:
2020370226
请输入学生姓名:
崔龙成
请输入学生年龄:
20
请输入学生居住地:
贵州贵阳
添加学生成功
----------欢迎来到学生管理系统----------
1.添加学生
2.删除学生
3.修改学生
4.查看所有学生
5.退出
请输入你的选择:
4
学号		姓名		年龄		居住地
2020370227	邱文宣		20岁		江西抚州
2020370228	汪忠辉		20岁		江西上饶
2020370226	崔龙成		20岁		贵州贵阳
----------欢迎来到学生管理系统----------
1.添加学生
2.删除学生
3.修改学生
4.查看所有学生
5.退出
请输入你的选择:
2
请输入你要删除的学生的学号：
2020370226
删除学生成功
----------欢迎来到学生管理系统----------
1.添加学生
2.删除学生
3.修改学生
4.查看所有学生
5.退出
请输入你的选择:
5
谢谢使用

Process finished with exit code 0

```

## 11.如何取一个数字的各个位上的数字

```java
public static void main(String[] args){
        int s = 1831;
        int g = s%10;
        int s = s/10%10;
        int b = s/100%10;
        int q = s/1000%10;
        System.out.println("个位数是:"+g+";十位数是:"+sw+";百位数是："+b+";千位数是："+q);
    }
结果:个位数是:1;十位数是:3;百位数是：8;千位数是：1
```

## 12.百钱买百鸡

```java
public class Demo08 {
    public static void main(String[] args) {
        for (int a = 0; a <= 20; a++) {
            for (int b = 0; b < 33; b++) {
                for (int c = 0; c < 100; c++) {
                    if (a + b + c == 100 && 5 * a + 3 * b + c/3 == 100){
                        System.out.println("鸡翁有"+a+"只，鸡母有"+b+"只，鸡雏有"+c+"只");
                    }
                }
            }
        }
    }
}
```

## 13.评委打分

```java
import java.util.Scanner;
public class Demo05 {
    public static void main(String[] args) {
        int[] arr = new int[6];
        System.out.println("请输入六个评委打分：");
        Scanner scan = new Scanner(System.in);
        for (int i = 0; i < arr.length; i++) {
            arr[i] = scan.nextInt();
        }
        System.out.println("该选手成绩为："+(sumArray(arr)-minArray(arr)-maxArray(arr))/4);
    }
    //方法一：求数组最高分
    //    返回值：int
    //    参数：arr[]
    public static int maxArray(int[] arr) {
        int max = arr[0];
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        return max;
    }
    //方法二：求数组最低分
    //    返回值：int
    //    参数： arr[]
    public static int minArray(int[] arr) {
        int min = arr[0];
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] < min) {
                min = arr[i];
            }
        }
        return min;
    }
    //方法三：数组求和
//    返回值：int sum;
//    参数：int[] arr;
    public static int sumArray(int[] arr){
        int sum = 0;
        for (int i = 0; i <arr.length ; i++) {
            sum+=arr[i];
        }
        return sum;
    }
}
运行结果：
   请输入六个评委打分：
44
55
66
77
88
99
该选手成绩为：71
```

## 14.标准类领养宠物

```java
import java.util.Scanner;

public class ThethodDemo01 {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        System.out.println("欢迎来到宠物店！");
        System.out.println("请输入要领养的宠物名字：");
        String name = scan.next();
        System.out.println("请输入要领养的宠物类型：（1.狗狗 2.企鹅）");
        int choose = scan.nextInt();
        if (choose == 1) {
            Dog dog = new Dog();
            dog.setName(name);
            System.out.println("请输入狗狗的品种：（1.聪明的拉布拉多 2.酷酷的雪纳瑞）");
            int pz = scan.nextInt();
            if (pz == 1) {
                dog.setBrand("聪明的拉布拉多");
            } else {
                dog.setBrand("酷酷的雪纳瑞");
            }
            System.out.println("请输入狗狗的健康值（1~100之间）：");
            int jkz = scan.nextInt();
            dog.setHealthValue(jkz);
            dog.showInfo();
        } else {
            Penguin pen = new Penguin();
            pen.setName(name);
            pen.setHealthValue(100);
            pen.setLoveValue(20);
            System.out.println("请输入企鹅的性别：（1.Q仔 2.Q妹）");
            int xb = scan.nextInt();
            if (xb == 1) {
                pen.setSex("雄");
            } else {
                pen.setSex("雌");
            }
            pen.showInfo();
        }


    }
}


//编写类的四个步骤
class Dog {
    //1--封装所有属性
    private String name;
    private int healthValue;
    private int loveValue;
    private String brand;

    //2--为封装的所有属性提供对外公共的setter和getter方法
    //3--编写构造方法
    public Dog() {
    }

    public Dog(String name, int healthValue, int loveValue, String brand) {
        this.name = name;
        this.healthValue = healthValue;
        this.loveValue = loveValue;
        this.brand = brand;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getHealthValue() {
        return healthValue;
    }

    public void setHealthValue(int healthValue) {
        if (healthValue >= 100 || healthValue <= 0) {
            System.out.println("你输入的健康值有误！默认值为60.");
            healthValue = 60;
        }
        this.healthValue = healthValue;

    }

    public int getLoveValue() {
        return loveValue;
    }

    public void setLoveValue(int loveValue) {
        this.loveValue = loveValue;
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    //4--编写功能方法，显示宠物所有信息
    public void showInfo() {
        System.out.println("宠物自白：\n" + "我的名字叫" + getName() + ",健康值是" + getHealthValue() + ",和主人的亲密度是" + getLoveValue() + ",我是一只" + getBrand());
    }
}

class Penguin {
    //1--封装所有属性
    private String name;
    private int healthValue;
    private int loveValue;
    private String sex;

    //2--为封装的所有属性提供对外公共的setter和getter方法
    //3--编写构造方法
    public Penguin() {
    }

    public Penguin(String name, int healthValue, int loveValue, String sex) {
        this.name = name;
        this.healthValue = healthValue;
        this.loveValue = loveValue;
        this.sex = sex;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getHealthValue() {
        return healthValue;
    }

    public void setHealthValue(int healthValue) {

        this.healthValue = healthValue;

    }

    public int getLoveValue() {
        return loveValue;
    }

    public void setLoveValue(int loveValue) {
        this.loveValue = loveValue;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    //4--编写功能方法，显示宠物所有信息
    public void showInfo() {
        System.out.println("宠物自白：\n" + "我的名字叫" + getName() + ",健康值是" + getHealthValue() + ",和主人的亲密度是" + getLoveValue() + ",我的性别是" + getSex());
    }
}


运行结果:
欢迎来到宠物店！
请输入要领养的宠物名字：
爱爱
请输入要领养的宠物类型：（1.狗狗 2.企鹅）
1
请输入狗狗的品种：（1.聪明的拉布拉多 2.酷酷的雪纳瑞）
2
请输入狗狗的健康值（1~100之间）：
120
你输入的健康值有误！默认值为60.
宠物自白：
我的名字叫爱爱,健康值是60,和主人的亲密度是0,我是一只酷酷的雪纳瑞
```

## 15.用户登录

```java

import java.util.Scanner;

public class Demo02 {
    public static void main(String[] args) {
        String username = "itheima";
        String password = "czbk";
        Scanner scan = new Scanner(System.in);
        for (int i = 1; i <= 3; i++) {


            System.out.println("请输入用户名:");
            String name = scan.next();
            System.out.println("请输入密码:");
            String pwd = scan.next();

            if (name.equals("itheima") && pwd.equals("czbk")) {
                System.out.println("登入成功!");
                break;
            } else {
                System.out.println("登入失败!你还有" + (3 - i) + "次机会.");
            }
        }
    }
}

运行结果:
    请输入用户名:
    qwe
    请输入密码:
    qe
    登入失败!你还有2次机会.
    请输入用户名:
    itheima
    请输入密码:
    czbk
    登入成功!
```

## 16.继承

### 1.teacher与student类(学生与老师)

```java
public class Person {//建立一个person类作为基类
    private String name;
    private int age;

    public Person(){}
    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
    
    public class Teacher extends Person{//创建一个Teacher类继承Perso类
    public Teacher(){}
//    public Teacher(String name, int age){
//        this.name = name;
//        this.age = age;
//    } 系统报错:即子类不能直接访问父类的私有变量private
    public Teacher(String name, int age){//继承父类当中的构造方法
        super(name,age);//使用super关键字来使子类继承父类当中的构造进行方法初始化
    }
    public void teach(){
        System.out.println("用爱成就每一位学员");
    }
}
    
    public class Student extends Person{//创建一个Student类继承Person基类
    public Student(){}
    public Student(String name, int age){
        super(name,age);
    }
    public void study(){
        System.out.println("好好学习,天天向上.");
    }
}

    public class Demo03 {//测试类
    public static void main(String[] args) {
        Teacher t1 = new Teacher();//无参构造方法
        t1.setName("林青霞");
        t1.setAge(30);
        System.out.println(t1.getName()+";"+t1.getAge());
        t1.teach();

        Teacher t2 = new Teacher("风清扬", 33);//带参构造方法
        System.out.println(t2.getName()+";"+t2.getAge());
        t2.teach();

        Student s = new Student("花无缺",20);
        System.out.println(s.getName()+";"+s.getAge());
        s.study();
    }
}
    
运行结果:
    林青霞;30
    用爱成就每一位学员
    风清扬;33
    用爱成就每一位学员
    花无缺;20
    好好学习,天天向上.

```

### 2.龟兔赛跑

```java

public class Animal {//定义基类Animal
    private double speed;

    public Animal() {
        super();
    }

    public Animal(double speed) {
        super();
        this.speed = speed;
    }



    public void setSpeed(double speed) {
        this.speed = speed;
    }

    public double getSpeed() {
        return speed;
    }
   
    public void run(int length) {//定义run方法
        System.out.println(length + "米跑了" + (length / speed) + "秒");
    }
}


public class Rabbit extends Animal{//定义子类Rabbit
    public Rabbit() {
    }

    public Rabbit(double speed){
        super(speed);
    }

}


public class Tortoise extends Animal{//定义子类torto
    public Tortoise() {
    }

    public Tortoise(double speed) {
        super(speed);
    }
}


public class Match {//定义Match类
    private double length;

    public Match() {
    }

    public Match(double length) {
        this.length = length;
    }

    public double getLength() {
        return length;
    }

    public void setLength(double length) {
        this.length = length;
    }

    public void begin(Rabbit rr,Tortoise tt){
        System.out.println(length+"米兔子跑了"+(this.length/rr.getSpeed())+"秒");
        System.out.println(length+"米乌龟跑了"+(this.length/tt.getSpeed())+"秒");
    }
}


public class Demo01 {//定义测试类
    public static void main(String[] args) {
        Rabbit rr = new Rabbit(100);
        Tortoise tt = new Tortoise(15);
        Match match = new Match(800);
        match.begin(rr,tt);

    }
}

运行结果:
    800.0米兔子跑了8.0秒
    800.0米乌龟跑了53.333333333333336秒

```

## 17.最大公因数与最小公倍数

1)基本概念

最大公因数，也称最大公约数、最大公因子，指两个或多个整数共有约数中最大的一个。a，b的最大公约数记为（a，b），同样的，a，b，c的最大公约数记为（a，b，c），多个整数的最大公约数也有同样的记号。

### 2)最大公因数:==辗转相除法(欧几里得算法)==

如,求(319,377)的最大公因数

319/377 = 0...319
(319,377)->(377,319)

377/319 = 1...58
(377,319)->(319,58)

319/58 = 5...29
(319,58)->(58,29)

58/29 = 2...0
(58,29) = (29,0)

因此,(319,377)的最大公因数为29;

### 2.最小公倍数

1)基本概念

两个或多个整数公有的倍数叫做它们的公倍数，其中除0以外最小的一个公倍数就叫做这几个整数的最小公倍数。整数a，b的最小公倍数记为[a，b]，同样的，a，b，c的最小公倍数记为[a，b，c]，多个整数的最小公倍数也有同样的记号。

与最小公倍数相对应的概念是最大公约数，a，b的最大公约数记为（a，b）。关于最小公倍数与最大公约数，我们有这样的定理：==(a,b)[a,b]=ab(a,b均为整数)==

```java
package day1125;

import java.util.Scanner;

public class Demo04 {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("请输入两个整数:");
		int a = scan.nextInt();
		int b = scan.nextInt();
		System.out.println(gcd1(a,b));
		System.out.println(gcd2(a,b));
		System.out.println(lcm1(a,b));
		System.out.println(lcm2(a,b));
	}
	public static int gcd1(int a, int b) {//最大公因数 递归法
		if(b==0) { 
		return a;
	}
		int r = a%b;
		return gcd1(b,r);
	}
	public static int lcm1(int a, int b) {//最小公倍数 公式法
		return a*b/gcd1(a,b);
	}
	public static int gcd2(int a, int b) {//最大公因数 枚举法
		int min = Math.min(a, b);
		int result = 0;
		for(int i = 1; i <= min; i++) {
			if(a%i==0 && b%i==0) {
				result = i;
			}
		}
		return result;
	}
	public static int lcm2(int a,int b){//最小公倍数 枚举法
        int temp=0;
        if(a<=0||b==0){
            return -1;
        }
        temp=Math.max(a,b);
        while(temp%a!=0||temp%b!=0){
            temp++;
        }
        return temp;
    }
}

```

## 18.闰年判断

闰年判断:

```java
package lqbday1129;

import java.util.Scanner;

//给定一个年份，判断这一年是不是闰年。
//当以下情况之一满足时，这一年是闰年：
//年份是4的倍数而不是100的倍数；
//年份是400的倍数。
//其他的年份都不是闰年。
public class Lunnianpanduan {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		while(true) {
		System.out.println("请输入一个年份:");
		int year = scan.nextInt();
		if(year<1990 || year>2050) {
			System.out.println("你输入的日期有误,请重新输入!");
		}else {
		if((year%4==0 && year%100!=0) || year%400==0) {
			System.out.println("该年份是闰年!");
		}else {
			System.out.println("该年份为平年!");
		}
		}
	}
}
	}

```

## 19.进制转换

```java
package com.zth;
import java.util.Scanner;
public class JinZhi {
  // 十进制转换为 n 进制
  public String fun(int n,int num) {
    // n 表示目标进制, num 要转换的值
    String str= "";
    int yushu ;      // 保存余数
    int shang = num;      // 保存商
    while (shang>0) {
      yushu = shang %n;
      shang = shang/n;
      
      // 10-15 -> a-f
      if(yushu>9) {
        str =(char)('a'+(yushu-10)) + str;
      }else {
        str = yushu+str;
      }
    }
    
    return str;
  }
 
  public static void main(String[] args) {
    // TODO Auto-generated method stub
    JinZhi s1 = new JinZhi();
    
    Scanner scanner = new Scanner(System.in);
    System.out.print("请输入目标进制：");
    int jinzhi = scanner.nextInt();
    System.out.print("请输入要转换的数字：");
    int num = scanner.nextInt();
    scanner.close();
    
    System.out.println(s1.fun(jinzhi, num));
  }
 
}
```

### 例：含偏移量的进制转换

![image-20221123150616565](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221123150616565.png)

```java
import java.util.Scanner;

class Main {
    public static void main(String[] args) {
        Scanner cin = new Scanner(System.in);
        int x = cin.nextInt();
        String s = "";//空数组接收值
        while (x > 0) {//判断值是否为0 
            s += (char) ((x - 1) % 26 + 'A');
            x = (x - 1) / 26;
        }
        for (int i = s.length() - 1; i >= 0; i--) {
            System.out.print(s.charAt(i));
        }
    }

}

```



## 20.用ArrayList集合存储Student对象,并按学生成绩排序遍历

```java
package demo01;

//4、定义一个学生集合，有年龄，考试分数，姓名三个属性 请从控制台接收5个学生，
//        存储到ArrayList集合对象中，并根据考试分数属性进行排序。

import java.util.ArrayList;
import java.util.Comparator;

public class ArrayList01 {
    public static void main(String[] args) {
        Student s1 = new Student("张三", 23,99);
        Student s2 = new Student("李四", 24,50);
        Student s3 = new Student("王五", 25,85);
        Student s4 = new Student("赵六", 26,70);
        Student s5 = new Student("刘七", 26,90);
        ArrayList<Student> list = new ArrayList<>();
        list.add(s1);
        list.add(s2);
        list.add(s3);
        list.add(s4);
        list.add(s5);
        list.sort(new Comparator<Student>() {
            @Override
            public int compare(Student s1, Student s2) {
                int num = s1.getScore() - s2.getScore();
                int num1 = num == 0 ? s1.getName().compareTo(s2.getName()) : num;
                return num1;
            }
        });
        for (Student s : list) {
            System.out.println(s.getName() + "," + s.getAge()+","+s.getScore());
        }
    }
}
class Student {
    private String name;
    private int age;
    private int score;

    public Student() {
    }

    public Student(String name, int age, int score) {
        this.name = name;
        this.age = age;
        this.score = score;
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

    public int getScore() {
        return score;
    }

    public void setScore(int score) {
        this.score = score;
    }
}


```

## 21.生成随机数

两种方法:

### 1.Random类

​	可以生成任意数据类型的随机数,需先创建类对象

| ![image-20211213192225983](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20211213192225983.png) |
| ------------------------------------------------------------ |

```java
package demo04;

import java.util.Random;

public class Text01 {
    public static void main(String[] args) {
        Random r = new Random();

        int i = r.nextInt();//生成所有int范围内的整数
        int i1 = r.nextInt(18)-3;//生成[-3,15)之间的数
        int i2 = r.nextInt(10);//生成[0,10)之间的数
        double d = r.nextDouble();//生成[0,1.0)之间的数
        double d1 = r.nextDouble()*7;//生成[0,7.0)之间的数
        float f = r.nextFloat();//生成一个[0,1.0)之间的随机浮点型数
        boolean b = r.nextBoolean();//生成一个随机布尔型值
        long l = r.nextLong();//生成一个long范围之间的所有随机长整型数
        
    }
}

```

### 2.Math类当中的random()方法

​	只能默认生成[0~1.0)之间的浮点型数,可以通过强制类型转换转为int型,无需创建类对象

```java
int a = (int)(Math.random()*1000);//[0,1000)
int a1 = (int)(2+Math.random()*98);//[2,100)
```

我们可以这么定义:

```java
package demo04;

public class Text02 {
    public static void main(String[] args) {
        int Min = 2;//定义最大值
        int Max = 102;//定义最小值
        for (int i = 0; i < 10; i++) {
            int s = (int) Min + (int) (Math.random() * (Max - Min));//生成一个[2~102)的随机数       
            System.out.println(s);
        }
    }
}
```

