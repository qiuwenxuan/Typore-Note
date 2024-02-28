### 1.算法复杂度

![image-20221108145158565](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221108145158565.png)



java相对于c++没有指针，那么我们java是怎么不通过指针传入方法参数改变主函数变量的值的呢

> 当在主函数外定义一个函数时，想在一个函数里改变一个变量的值，不能直接当形参传入，否则不会改变变量的值。
>
> 可以定义在外面，然后在函数里直接改变，
>
> 也可以放在[数组](https://so.csdn.net/so/search?q=数组&spm=1001.2101.3001.7020)中，然后将数组当形参传入，可以改变数组中的元素值，
>
> 请看代码示例：

```java
public class Public_static {

	static int n=0;
	//n不当参数传入时。可以改变它的值
	static int[] a;
	static void fun2(int n){
		n=3;
	}

	static void fun(){
		n=1;
	}

	static void fun1(int[] a){
		a[0]=2;
	}

	public static void main(String[] args) {
		// TODO 自动生成的方法存根
		fun2(n);//不会改变n的值
		System.out.println(n);
       
		fun();
		System.out.println(n);

		a=new int[1];
		fun1(a);
		System.out.println(a[0]);
	}
}
```

输出如下：

```java
0



1



2
```

 

### 2.递归

#### 案例一 递归实现`指数型`枚举

![image-20221108164158521](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221108164158521.png)

> 题解：
>
> ![image-20221108163931192](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221108163931192.png)
>
> ![截屏2022-01-22 下午2.56.55.png](https://cdn.acwing.com/media/article/image/2022/01/22/71096_9e663d7c7b-%E6%88%AA%E5%B1%8F2022-01-22-%E4%B8%8B%E5%8D%882.56.55.png)
>
> 

```c
#include <iostream>
#include <algorithm>
#include "cstring"
#include "cstdio"

using namespace std;
const int N = 16;

int n;
int st[N];//记录状态数组0表示未定义，1表示选择，2表示不被选择

void dfs(int u) {//dfs(u)代表第几层也代表一个指针指到第几个数
    if (u > n) {
        for (int i = 1; i <= n; i++)
            if (st[i] == 1)
                printf("%d ", i);
        printf("\n");
        return;
    }
    //数组没有遍历完成时分两种情况实现不同分支:

    st[u] = 1;//如果定义该分支为1，则实现下一分支
    dfs(u + 1);//递归调用实现下一分支的操作
    st[u] = 0;//回溯该分支为0未定义状态，因为我们最后只需要得到最后一层数组的输出即可，该位置回溯与否都不影响结果，可以省略，回溯是为了方便理解

    st[u] = 2;//如果定义该分支为2,又是一种情况实现下面的分支
    dfs(u + 1);//递归实现下一分支
    st[u] = 0;//回溯
}

int main() {
    scanf("%d", &n);
    dfs(1);

    return 0;
}


```

```java
Java代码：
import java.util.*;


public class Main {
    static int N = 20;
    static final int[] state = new int[N];//0表示默认，1表示取到，2表示不取到
    static int n;

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        n = scan.nextInt();

        f(1);

    }

    static void f(int u) {
        if (u > n) {
            for (int i = 1; i <= n; i++) {
                if (state[i] == 1) {
                    System.out.print(i + " ");
                }
            }
            System.out.println();
            return;
        }
        state[u] = 1;
        f(u + 1);
        state[u] = 0;

        state[u] = 2;
        f(u + 1);
        state[u] = 0;

    }

}

```



#### 案例二 递归实现`排列型`枚举

![image-20221109204821071](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221109204821071.png)

> ![image-20221109204921633](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221109204921633.png)
>
> 算法：
> 用state[]数组保存排列，当排列的长度为 n 时，是一种方案，输出。
> 用used[]数组表示数字是否用过。false未被使用，true被使用
> dfs(u) 表示的含义是：在 state[u] 处填写数字，然后递归在下一个位置填写数字。
> 恢复现场（回溯）：第 i 个位置填写某个数字的所有情况都遍历后，回溯到上一步，恢复现场；

```c
#include <iostream>
#include <algorithm>
#include "cstring"
#include "cstdio"

using namespace std;
const int N = 10;
int n;
int state[N];//0未定义，1选择，2不选择
bool used[N];//false未被使用，true被使用

void dfs(int u) {
    if (u > n) {
        for (int i = 1; i <= n; ++i) {//由于u从1开始代表层数也代表递归到第几位，state[u]应该也从1开始数据才有效
            printf("%d ", state[i]);//输出数组当中的数
        }
        printf("\n");
        return;
    }
    for (int i = 1; i <= n; ++i) {//由于我们是先从小到大对数组当中的元素赋值，所以输出的数组已经是字典序排序好的序列
        if (!used[i]) {//如果i未被使用，默认为false
            state[u] = i;//使用i到state[u]数组,u既可以看做递归的层数,也可以看做数组中指向第u位的指针
            used[i] = true;
            dfs(u + 1);
            //恢复现场
            state[u] = 0;
            used[i] = false;
        }
    }
}

int main() {
    scanf("%d", &n);
    dfs(1);
    return 0;
}


```

```c
java代码：
import java.util.Scanner;

//排列型枚举
public class Main {

    static int n;
    static int[] state = new int[10];
    static boolean[] used = new boolean[10];

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        n = scan.nextInt();
        dfs(1);
    }

    static void dfs(int u) {
        if (u > n) {
            for (int i = 1; i <= n; i++) {
                System.out.print(state[i] +" ");
            }
            System.out.println();
            return;
        }
        for (int i = 1; i <= n; i++) {
            if (!used[i]) {

                state[u] = i;
                used[i] = true;
                dfs(u + 1);
                state[u] = 0;
                used[i] = false;

            }
        }
    }

}

```



> 这里解释一下为什么要恢复现场：
>
> ​	这里的恢复现场不会导致i=1后立马恢复现场，而是i++遍历完之后，再恢复现场；
>
> ​    即同一条分支深度遍历相互之间不恢复现场，不同分支即同一层之间恢复现场;
>
> 因为排列型枚举含有for循环for(0~n),即每一层递归都含有循环每次循环都往最深处探索，即深度优先搜索，因此该队规生成树的执行顺序是先
>
> ![image-20230304104204594](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230304104204594.png)
>
> ```c
> #include<iostream>
> using namespace std;
> 
> const int N=20;
> 
> int n;
> bool vis[N];//判断是否访问过
> int pre[N];//保存该顺序的序列
> 
> void DFS(int u)
> {
>  if(u>n)
>  {
>      for(int i=1;i<=n;i++)
>      cout<<pre[i]<<" ";
>      cout<<endl;
>  }
>  else
>  {
>      for(int i=1;i<=n;i++)
>      {
>          if(!vis[i])//如果该节点没有访问过i=1
>          {
>              vis[i]=true;//访问该节点
>              pre[u]=i;//每一层对应一个数字保存
>              DFS(u+1);//递归到下一层
> 
>              vis[i]=false;//恢复现场
>              pre[u]=0;//恢复现场
>          }
>      }
>  }
> }
> int main(){
>  cin>>n;
>  DFS(1);
>  return 0;
> }
> 
> 
> 
> //通过模拟一次的递归过程，来理解为何要恢复现场
> /*
> DFS(1)
> {
>  for(i=1;i<=n;i++)
>  {
>      if(vis[]没有被访问过)i=1
>      {
>          pre[1]=1
>          vis[1]=true
>  *****************************以下为  DFS（u+1）的内容*******************************
>          DFS(2)   //递归第二层
>          {   for(i=2;i++)
>              pre[2]=2
>              vis[2]=true
>              DFS(3)
>              {
>              	for(i=3;i++)
>                  pre[3]=3
>                  vis[3]=true
>                  DFS(4)
>                  {
>                      if:输出
>                  }
>                  pre[3]=0
>                  vis[3]=false
>              }
>              pre[2]=0
>              vis[2]=false
>          }
>  ****************************以上为  DFS（u+1）的内*********************************
>          vis[1]=false    //恢复现场，不然下次循环无法进行
>          pre[1]=0   
>      }
>  }
> }
> */
> 
> ```
>
> 



#### 案例三 递归实现`组合型`枚举

> ![image-20221110094744644](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221110094744644.png)
>
> 排列和组合问题：
>
> ​		排列从数组中选择任意数进行排序输出，每种不同排序的输出都算一种方式
>
> ​		组合是从数组当中选择任意数进行输出，每种选择的方式数字组合须不相同

![image-20221110091405924](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221110091405924.png)

> ![image-20221110094734675](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221110094734675.png)
>
> 由于此题组合型枚举，我们需要使得每种方法所选数组皆不同且字典序需要从小到大输出，因此我们需要再递归中额外传入一个参数start使得从start~n补到ways[m]数组后，直到下一个递归dfs(u+1,start+1)从而解决了重复问题，使得输出的数后面永远大于前面，
>
> 因为后面的数永远大于前面的数，因此可用不需要判断可用数组
>
> 注意：剪枝表达式：`u+n-start<m`

```c
#include <iostream>
#include <algorithm>
#include "cstring"
#include "cstdio"

using namespace std;

const int N = 30;
int n, m;
int ways[N];

void dfs(int u, int start) {
    if (u + n - start < m) {//剪枝，排除start~n不及m后面剩余位数 这里剪枝可有可无
        return;
    }
    if (u > m) {
        for (int i = 1; i <= m; ++i) {
            printf("%d ", ways[i]);
        }
        puts("");
        return;
    }
    for (int i = start; i <= n; ++i) {//为了保证能输出字典序序列，且不输出重复组合，我们需定义一个start=前一个数+1，使得后面的数都比前面大
        ways[u] = i;
        dfs(u + 1, i + 1);
        ways[u] = 0;
    }
}

int main() {
    scanf("%d%d", &n, &m);
    dfs(1, 1);//根据分析我们需要传入三个变量，①三个位置ways[N];②当前该枚举哪个位置u；③start当前最小可以从哪个数开始枚举
    return 0;
}


```

```java
java代码：
import java.util.Scanner;

//排列型枚举
public class Main {
    static int n, m;
    static final int N = 30;
    static int[] state = new int[N];

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        n = scan.nextInt();
        m = scan.nextInt();
        dfs(1, 1);
    }

    static void dfs(int u, int start) {
        if (u + n - start < m) {
            return;
        }
        if (u > m) {
            for (int i = 1; i <= m; i++) {
                System.out.print(state[i] + " ");
            }
            System.out.println();
            return;
        }
        for (int i = start; i <= n; i++) {
            state[u] = i;
            dfs(u + 1, i + 1);
            state[u] = 0;
        }
    }
}

```

#### 三种递归综合理解：

> 指数型枚举：
>
> 核心代码：
>
> ```java
> st[u] = 1;//指数型枚举每个位置只有两种选择，要么选要么不选，st[u] = i = 1表示选择，st[u]=2表示不选，0表示默认
> //输出st[u]=1的值即可
> dfs(u+1);
> st[u] = 0;
> 
> st[u] = 2;
> dfs(u+1);
> st[u] = 0;
> ```

> 排列型枚举：需要注意一下，排列和枚举都需要用到循环for枚举每一个位置
>
> 核心代码：
>
> ```java
> for(int i = 1; i < n; i++){//需创建两个数组，一个用来保存输出结果的数组st[]，另一个用来保存该数字是否用过used[]
> 	if(!used[i]){
> 		st[u] = i;
> 		used[i] = true;
> 		dfs(u+1);
> 
> 		st[u] = 0;
> 		used[i] = false;
> 	}
> }
> if(u>n){//这里要注意，由于st[u]是根据层数u来添加数据的，因此不存在层数为0的st[0]，因此输出要从1开始
>     for(int i = 1; i <= n; i++){
>         sout(st[i]);
>     }
> }
> ```

> 组合型枚举核心代码：相比于排列型枚举，没有used[]确认数组，且多出一个剪枝
>
> ```java
> for (int i = start; i <= n; ++i) {//为了保证能输出字典序序列，且不输出重复组合，我们需定义一个start=前一个数+1，使得后面的数都比前面大
>    	st[u] = i;
>     dfs(u + 1, i + 1);
>     st[u] = 0;
>     
>     //组合型枚举可能含有剪枝操作,剪枝操作一般会在return语句前和其并列
>     if (u + n - start < m) {//剪枝，排除start~n不及m后面剩余位数 这里剪枝可有可无
>         return;
>     }
> ```
>
> 



### 3.简单动态DP

> ​		动态DP类似于求前缀和；给出初始化值然后不断累加得到后面每一位的值，`后面的每个值都由前面的值推出`；
>
> ​		由闫式分析法：找到一个不同点，然后通过这个不同点可以将一个整体问题分解为两块更简单的部分模块，两块模块相互独立且这两块部分并在一起恰好等于整体本身；
>
> ![image-20221116223933683](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221116223933683.png)
>
> ​		比如说拿01背包问题来理解，找唯一的不同点就是是否取第i个物品，将求解f[i,j]（`取前i个物品当中容量不超过j的最大的价值）的问题分解为所有不选择第i个物品的方案和选择第i个物品的方案，两个方法加起来刚好概括了所有的选择方案

![image-20221116220556264](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221116220556264.png)

#### 1、01背包问题

![image-20221116210249199](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221116210249199.png)

##### 1. 二维01背包问题：

> 01背包 即给出了i个物品，每个物品只有两种情况拿和不拿（0和1）；
>
> （1）状态f[i] [j]定义：前 i个物品，背包容量 j下的最优解（最大价值）：
>
> - 当前的状态依赖于之前的状态，可以理解为从初始状态f[0] [0] = 0开始决策，有 N 件物品，则需要 N次决 策，每一次对第 i 件物品的决策，状态f[i] [j]不断由之前的状态更新而来。
>
> （2）当前背包容量不够（j < v[i]），没得选，因此前 i 个物品最优解即为前 i−1个物品最优解：
>
> - 对应代码：`f[i] [j] = f[i - 1] [j]。`
>
> （3）当前背包容量够(j >= v[i])，可以选，因此需要决策选与不选第 i 个物品：
>
> 选：`f[i] [j] = f[i - 1] [j - v[i]] + w[i]`
> 不选：`f[i] [j] = f[i - 1] [j]` 
> 我们的决策是如何取到最大价值，因此以上两种情况取 max() 。
>
> ![image-20230304110352775](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230304110352775.png)

```c
#include <iostream>
#include <algorithm>
#include "cstring"
#include "cstdio"

using namespace std;

const int N = 1010;

int n, m;//物品数量n，背包容积m
int f[N][N];//选前N个物品，且容积不超过N的方案的最大价值
int v[N], w[N];//第i件物品的体积v[i]，第i件物品的价值w[i]

int main() {

    cin >> n >> m;//n物品数量，m背包容积
    for (int i = 1; i <= n; ++i) {
        cin >> v[i] >> w[i];//v[i]第i件物品的体积，w[i]表示第i件物品的价值
    }

    for (int i = 1; i <= n; ++i) {
        for (int j = 0; j <= m; ++j) {
            f[i][j] = f[i - 1][j];//不选择第i件物品，可能选择i+1件物品，因此需要取舍掉i件物品保证能有体积选到后面的物品
            if (j >= v[i]) {//如果容量体积大于第i件物品，可以选择第i件物品
                f[i][j] = max(f[i][j], f[i - 1][j - v[i]] + w[i]);//最大价值方案为在选择i件商品和不选择i件商品之间取最大值
                //选择第i件商品可以理解成选前面i-1个商品且容积不超过j-v[i]再加上i商品的价格 f[i - 1][j - v[i]] + w[i]
            }
        }
    }
    int res = 0;
    for (int i = 0; i <= m; ++i) {
        res = max(res, f[n][i]);//保存最大值
    }
    cout << res << endl;

    return 0;

}


```

##### 2. 01背包问题的化简：一维01背包问题

> 将状态f[i] [j]优化到一维f[j]，实际上只需要做一个等价变形。
>
> 为什么可以这样变形呢？我们定义的状态f[i] [j]可以求得任意合法的i与j最优解，但题目只需要`求得最终状态f[n] [m]`，因此我们只需要一维的空间来更新状态。
>
>  `f[i] [j] = max(f[i] [j], f[i - 1] [j - v[i]] + w[i]);` 
>
> ​		由这个公式可以看出，由于右边等式 `f[i] [j] = max(f[i] [j],XXX)` 相互抵消，我们重点研究 `f[i] [j] = f[i - 1] [j - v[i]] + w[i]` 这个等式，可以看出左边f[i]和右边f[i-1]有关,因此我们是否用一个滚动数组只存取f[j]，将二维数组f[i] [j]化简为一维f[j]呢：
>
> 观察两个等式： `f[i] [j] = f[i - 1] [j - v[i]] + w[i]`和 `f[j] = f[j - v[i]] + w[i]` 
>
> ​		将j从后往前遍历，方程两边就可以等价消去f[i]和f[i-1]，为什么是等价的呢？因为在i层循环当中，假设（j = 5； j >= 2; j --）;也就是这一层（第i层）会先算出来f[i] = f[5]，而f[j - v[i]] (假设为f[3])小于f[5]还未更新，因此里面存储的是上一层（i-1）层的f(3)，`因此方程左右两边的f[i]和f[i-1]是等价的可以消去`；
>
> ​		如果j从小到大更新的话，(i-1)层的f[3]就会先被i层的f[3]给覆盖，因此此时计算的等式变为了`f[i] [j] = f[i] [j - v[i]] + w[i]`而不是`f[i] [j] = f[i - 1] [j - v[i]] + w[i]` ;
>
> ​	状态转移方程为：`f[j] = max(f[j], f[j - v[i]] + w[i] 。`

```C
for(int i = 1; i <= n; i++) 
    for(int j = m; j >= 0; j--)
    {
        if(j < v[i]) 
            f[i][j] = f[i - 1][j];  // 优化前
            f[j] = f[j];            // 优化后，该行自动成立，可省略。
        else    
            f[i][j] = max(f[i - 1][j], f[i - 1][j - v[i]] + w[i]);  // 优化前
            f[j] = max(f[j], f[j - v[i]] + w[i]);                   // 优化后
    }   

```

> 实际上，只有当枚举的背包容量 >= v[i] 时才会更新状态，因此我们可以修改循环终止条件进一步优化。

```C
for(int i = 1; i <= n; i++)
{
    for(int j = m; j >= v[i]; j--)  
        f[j] = max(f[j], f[j - v[i]] + w[i]);
} 
```

##### 3.  输入优化一维01背包问题

> 我们注意到在处理数据时，我们是一个物品一个物品，一个一个体积的枚举。
>
> 因此我们可以不必开两个数组记录体积和价值，而是边输入边处理。

```c
const int MAXN = 1005;
int f[MAXN];  

int main() 
{
    int n, m;   
    cin >> n >> m;

    for(int i = 1; i <= n; i++) {
        int v, w;
        cin >> v >> w;      // 边输入边处理
        for(int j = m; j >= v; j--)
            f[j] = max(f[j], f[j - v] + w);
    }

    cout << f[m] << endl;

    return 0;
}

```



#### 2、摘花生

> 闫式分析法：
>
> ![image-20221119151016152](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221119151016152.png)
>
> 1、判断集合的状态和属性（求最大或最小或其他）；
>
> 2、找不同点（到达最后一步的前一步的走法（要么向下要么向右）分两种情况）
>
> 3、通过不同点划分集合，划分为两个集合，两个集合不重不漏，说明集合的正确划分；
>
> 4、给不同集合一个关系表达式，用来概括所有集合内的值；
>
> 5，如果是求最大值，把两个集合求max

![image-20221119151013109](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221119151013109.png)

```c
#include "iostream"
#include "cstdio"
#include "algorithm"

using namespace std;

int R, C;//行R,列C
int m[110][110], f[110][110];//m[][]表示该点的花生数，f[m][n]表示从(1,1)到(m,n)点的最大花生数；

int main() {
    int T;
    cin >> T;
    while (T--) {
        cin >> R >> C;
//        for (int i = 1; i <= R; ++i) {
//            for (int j = 1; j <= C; ++j) {
//                cin >> m[i][j];//该语句可以和下面的语句结合，使代码简介话
//            }
//        }

        for (int i = 1; i <= R; ++i) {
            for (int j = 1; j <= C; ++j) {
                cin >> m[i][j];
                f[i][j] = max(f[i - 1][j] + m[i][j], f[i][j - 1] + m[i][j]);//表达式
            }
        }

        printf("%d\n", f[R][C]);
    }
    return 0;

}
```



#### 3、最长上升子序列

![最长上升子序列](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221120100248876.png)

> 分析：
>
> ![image-20221120100434206](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221120100434206.png)
>
> ​		f[i]表示所有以a[i]结尾的严格单调上升的子序列，我们可以找倒数第二个元素a[k]为不同点，因此f[i]由所有倒数第二个不同元素a[k]组成，此时f[i] = max(f[i],f[k]+1);//需要加上本身a[i]长度为1，且需满足a[k]<a[i]；

```c
#include <iostream>

using namespace std;

const int N = 1010;

int n;
int w[N], f[N];

int main() {
    cin >> n;
    for (int i = 0; i < n; i++) cin >> w[i];

    int mx = 1;    // 找出所计算的f[i]之中的最大值，边算边找
    for (int i = 0; i < n; i++) {
        f[i] = 1;    // 设f[i]默认为1，找不到前面数字小于自己的时候就为1
        for (int j = 0; j < i; j++) {
            if (w[i] > w[j]) f[i] = max(f[i], f[j] + 1);    // 前一个小于自己的数结尾的最大上升子序列加上自己，即+1
        }
        mx = max(mx, f[i]);
    }

    cout << mx << endl;
    return 0;
}

```

#### 4、综合案例：地宫取宝

01背包+摘花生+最长上升子序列

![image-20221124103356507](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221124103356507.png)

![image-20221124103502404](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221124103502404.png)

> 状态表示：f[i,j,k,c]表示所有从起点走到（i,j)，已经取了k件物品，且最后一件物品的价值为C的合法方案的集合。
>
> 属性：求方案总数量
>
> 我们可以将集合根据是否取最后一个物品c分成两份（01背包），最后一步选择是向右还是向下分为两份（摘花生），然后在取最后一个物品当中再根据倒数第二个物品的价值细分（从0~c'）c'<c才能确保最后一个选到的物品价值为c

```c
#include <cstring>
#include "iostream"
#include "cstdio"
#include "algorithm"

using namespace std;

const int N = 55, MOD = 1000000007;

int n, m, k;
int w[N][N];
int f[N][N][13][14];//f[i,j,k,c]表示所有从起点走到（i,j)，已经取了k件物品，且最后一件物品的价值为C的合法方案的数量

int main() {
    cin >> n >> m >> k;
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            cin >> w[i][j];
            w[i][j]++;//物品的价值+1，因为存在价值为0的物品和没物品的情况，没赋值即为没物品默认也是0，因此我们把所有赋值的情况都+1，和没有赋值的情况区分；且如果第一个物品价值是0，我们默认值也是0则不能表示为不取0的情况，如果将价值改为1的话，那我们取价值为0的物品的话为1，不取的话为0
        }
    }
    f[1][1][1][w[1][1]] = 1;//初始化赋值 (1,1) 开始的时候选与不选初始化
    f[1][1][0][0] = 1;

    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {//遍历坐标(i,j)
            if (i == 1 && j == 1)continue;//已经初始化过了，直接跳过
            for (int u = 0; u <= k; ++u) {//已经取得的物品的数量u
                for (int v = 0; v <= 13; ++v) {//最后一件物品的价值v
                    int &val = f[i][j][u][v];//相当于取别名，将以下所有情况存在f[i][j][u][v]里
                    val = (val + f[i - 1][j][u][v]) % MOD;
                    val = (val + f[i][j - 1][u][v]) % MOD;
                    if (u > 0 && v == w[i][j]) {//表示在(i,j)物品上取(i,j)这个物品，如果取了则必须满足v==w[i][j];表示最后一件物品的价值c刚好为(i,j)物品的价值，即取的最后一个物品为(i,j)
                        for (int c = 0; c < v; ++c) {//累加c'<c的所有方案数
                            val = (val + f[i - 1][j][u - 1][c]) % MOD;
                            val = (val + f[i][j - 1][u - 1][c]) % MOD;
                        }
                    }
                }
            }

        }
    }
    int res = 0;
    for (int i = 0; i <= 13; i++) res = (res + f[n][m][k][i]) % MOD;//累加最后一件物品的价值为0~c的所有方案；
    cout << res << endl;
    return 0;
}
```



### 4.复杂DP

​		判断一道题目是否可以用动态DP来处理，可以观察该状态的结果是否和前面的状态有关系，如果有关系，我们可以使用DP状态方程通过之前的结果得到后面的结果；`动态规划实际上就是通过推导前面运算的结果得到一个动态规律，通过该规律推导出后面的结果；` 

#### 1、n个苹果放入m个篮子

![image-20221122174015483](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221122174015483.png)

> 题解：此题我们可以将其看成将i个苹果放入j个篮子当中，求其方案数
>
> 由于此题是`组合类型`题目，因此相同两组数的不同排列也算一种；
>
> 首先定义状态方程f[i,j]表示将i个苹果放入j个篮子当中的方案数，其划分依据就是方案里面含不含0这个数，即有没有空篮子的组合，

![339c0d23366559de00ef50489f36f18.png](https://cdn.acwing.com/media/article/image/2020/02/16/7416_aedc15d050-339c0d23366559de00ef50489f36f18.png)

![image-20221122175823589](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221122175823589.png)

```c
#include "iostream"
#include "cstdio"
#include "algorithm"

using namespace std;

const int N = 11;
int m, n;
int f[N][N];

int main() {
    int T, res;
    scanf("%d", &T);
    while (T--) {
        scanf("%d%d", &m, &n);//m表示有m个苹果，n表示有n个篮子
        f[0][0] = 1;
        for (int i = 0; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                f[i][j] = f[i][j - 1];
                if (i >= j) f[i][j] += f[i - j][j];
            }
        }
        printf("%d\n", f[m][n]);//如果断定最后一个一定是最大值的话可以直接输出，如果不一定最后是最大值，最好创建一个res取遍历f[][]当中的最大数
    }
}
```

#### 2、糖果

![image-20221123104003857](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221123104003857.png)

![image-20221123104027660](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221123104027660.png)

> 这道题有点类似于01背包问题
>
> 首先分析其状态表示：f[i,j]表示从前i个物品当中选，与k余数为j且为最大值的方案；属性是max最大值
>
> 然后分析其集合的划分依据，和01背包问题类似，划分依据是取与不取第i件物品，如果不取，f[i,j] = f[i-1,j];
>
> 如果取，则将第i件物品w与前面i-1件物品分开，则i-1物品与k的余数是(j-w)%k,所以为f[i,j] = f[i-1,(j-w)%k+w];
>
> 取两者最大值max :`f[i] [j] = max(f[i - 1] [j],f[i - 1] [(j + k - w % k) % k] + w);` 
>
> 

```c
#include <cstring>
#include "iostream"
#include "cstdio"
#include "algorithm"

using namespace std;

const int N = 110;
int n, k;
int f[N][N];//从前i个数选，且总和除以k的余数是j的所有选择的最大值

int main() {

    cin >> n >> k;

    memset(f, -0x3f, sizeof f);//将所有f[n][0]的值初始化变成无限小，因为没有意义；
    f[0][0] = 0;//初始化，前0个数选余为0的最大值为0
    for (int i = 1; i <= n; ++i) {
        int w;
        cin >> w;
        for (int j = 0; j < k; ++j) {
            f[i][j] = max(f[i - 1][j],f[i - 1][(j + k - w % k) % k] + w);//(j - w[i]) % k如果j-w[i]<0,那么它的余数也小于0,因此我们将它变形确保它大于0
        }

    }
    printf("%d\n", f[n][0]);//输出从前n个数选，余数为0的和的最大值
    return 0;
}
```



### 5.贪心

#### 普通贪心

> 贪心问题 	：贪心算法总是作出在当前看来最好的选择。也就是说贪心算法并不从整体最优考虑，它所作出的选择只是在某种意义上的局部最优选择。当然，希望贪心算法得到的最终结果也是整体最优的。

案例一：股票交易

![image-20230316184149295](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230316184149295.png)

> 此题运用到贪心的基本原理，就是要保证没两个相邻的天数后一天比前一天要盈利就卖出，就能保证总体收益最大化；

![image-20230316192018920](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230316192018920.png)

```java
import java.util.Scanner;

public class Main01 {
    //股票交易
    static int N;
    static int[] a = new int[100010];

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int sum = 0;
        N = scan.nextInt();
        for (int i = 0; i < N; i++) {
            a[i] = scan.nextInt();

            if (i >= 1) {
                if (a[i] > a[i - 1]) sum += (a[i] - a[i - 1]);
            }
        }

        System.out.println(sum);


    }
}

```

#### 区间贪心

案例：雷达设备

![image-20230316212953172](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230316212953172.png)

![image-20230316215326845](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230316215326845.png)

```c
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
#include <cmath>

using namespace std;

const int N = 1010;

int n, d;
struct Segment
{
    double l, r;
    bool operator< (const Segment& t) const
    {
        return r < t.r;
    }
}seg[N];//创建小岛坐标结构体

int main()
{
    scanf("%d%d", &n, &d);

    bool failed = false;//小岛位置是否合理
    for (int i = 0; i < n; i ++ )
    {
        int x, y;
        scanf("%d%d", &x, &y);
        if (y > d) failed = true;//即如果小岛纵坐标大于雷达探测的距离，输入有误
        else
        {
            double len = sqrt(d * d - y * y);
            seg[i].l = x - len, seg[i].r = x + len;//求该点在横坐标上所能探测到的雷达坐标区间
        }
    }

    if (failed)
    {
        puts("-1");//输入有误，输出-1
    }
    else
    {
        sort(seg, seg + n);//排列小岛位置

        int cnt = 0;
        double last = -1e20;//表示区间最右端，默认取无穷小
        for (int i = 0; i < n; i ++ )
            if (last < seg[i].l)//如果前一个数的区间最右端小于这个区间的最左端，说明两者区间没有交集
            {
                cnt ++ ;//此时所需雷达数+1
                last = seg[i].r;//更新最右端值
            }

        printf("%d\n", cnt);//输出所需雷达数
    }

    return 0;
}


```

### 6、双指针算法

案例一：日志统计`滑动窗口` `经典`

![image-20230320201105645](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230320201105645.png)

![image-20230320201303626](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230320201303626.png)

首先对这道题进行分析：

```c
for(时间段){
	for(循环该时间段i = n~n+d){
		cnt[id[i]]++;
		if(cnt[id[i]]>=k) st[id] = true;
	}
}
即首先循环时间段（每十天为一段），然后每次出现id,cnt++计数一次，判断次数是否>=k，如果成立，存入st[id]当中
```

当然这种方法明显属于暴力算法，又发现每次时间段增加，只是增加了一个开始节点和减少了一个末尾节点，其他的中间的节点无异于再做重复功，因此我们可以优化代码，仅仅只存入新的节点，删除末尾节点，类似于滑动窗口；

```java
for(int i = 0; j = 0; i < n; i++){//i为新进入的节点，j为末尾的节点；x表示时间,y表示id
	int id = i.y;//新进来的节点为i
	cnt[id]++;
	
	while(i.x-j.x >= d){//即当时间段满了之后，需要删除末节点
		cnt[j.y]--;//删除一个末尾节点
		j++;//将末尾结点移动至时间段内
	}
	if(cnt[id]>=k) st[id] = true;//判断是否满足点赞数>=k,满足记录数组
}
```

java代码展示：

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

class blog implements Comparable<blog> {
    int id, ts;

    public blog(int id, int ts) {
        this.id = id;
        this.ts = ts;
    }

    //按时间升序
    @Override
    public int compareTo(blog o) {
        return this.ts - o.ts;//从小到大
    }

    public String toString() {
        return this.ts + " " + this.id + "\n";
    }
}

public class Main03 {
    static int N = 100010;
    static blog[] q = new blog[N];
    static boolean[] st = new boolean[N];
    static int[] cnt = new int[N];

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String s[] = br.readLine().split(" ");
        int n = Integer.parseInt(s[0]), D = Integer.parseInt(s[1]), K = Integer.parseInt(s[2]);
        for (int i = 0; i < n; i++) {
            String data[] = br.readLine().split(" ");
            int a = Integer.parseInt(data[0]);
            int b = Integer.parseInt(data[1]);
            q[i] = new blog(b, a);

        }
        Arrays.sort(q, 0, n);
        for (int i = 0, j = 0; i < n; i++) {
            int id = q[i].id;
            cnt[id]++;
            while (q[i].ts - q[j].ts >= D) {
                cnt[q[j].id]--;
                j++;
            }
            if (cnt[id] >= K) {
                st[id] = true;
            }

        }
        for (int j = 0; j < N; j++) { 
            if (st[j]) System.out.println(j);
        }

    }
}

```

案例二：完全二叉树的权值

![image-20230322194751630](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230322194751630.png)

首先分析如图所示：

![image-20230322194812460](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230322194812460.png)

第一层   下标1  个数1

第二层   下标2~3  个数2

第三层   下标4~7  个数3

我们可以找到规律：

```java
for (int i = 1, j = 1, max = 0; i <= n; i *= 2, j++) {
//即i为每层的开始下标（为了计数方便这里数组下标从1开始），j为层数，这里i每次*2（1 2 4 8 16）

            for (int k = i; k < i * 2 && k <= n; k++) {//循环遍历每一层，且最后一层可能不满，即再加一个k<=n的判定条件
//                System.out.print(q[k] + " ");
                s[j] += q[k];//数组s[j]表示第j层的权数，q[]表示数组
            }
//            System.out.println();
            if (max < s[j]) {//更新每层的最大值
                max = s[j];
                res = j;
            }
        }
```

```java
import java.io.*;


public class Main01 {
    static int[] q;
    static int[] s;
    static int N = 100010;
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

    public static void main(String[] args) throws IOException {
//        Scanner scan = new Scanner(System.in);
//        int n = scan.nextInt();
        q = new int[N];
        s = new int[N];
        String firstline = br.readLine();
        int n = Integer.parseInt(firstline);
        String[] secondline = br.readLine().split(" ");
        for (int i = 1; i <= n; i++) {
            q[i] = Integer.parseInt(secondline[i - 1]);
//            q[i] = scan.nextInt();
        }
        int res = 0;
        for (int i = 1, j = 1, max = 0; i <= n; i *= 2, j++) {


            for (int k = i; k < i * 2 && k <= n; k++) {
//                System.out.print(q[k] + " ");
                s[j] += q[k];
            }
//            System.out.println();
            if (max < s[j]) {
                max = s[j];
                res = j;
            }
        }
        System.out.println(res);

        br.close();

    }
}

```

7、BFS(宽度优先搜索)



 
