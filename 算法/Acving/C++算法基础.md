## 第一章 算法基础

### 快速排序（分治算法）

> ![image-20221103195241314](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221103195241314.png)
>
> 思想：
>
> 1. 确定分界点
> 2. 调整区间
> 3. 递归排序左右两边

模板

![image-20221103203238431](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221103203238431.png)



> 快速排序的原理就是：快排quick_sort(q,0,n-1),三个参数分别是数组的两端，然后函数体内需要取别名l = start,r = end,因为这样可以保证在操作数据的同时不改变start和end的值；
>
> 然后定义一个基准base = q[l] （一般取最左边或者中间，这里取左边），左右两个指针r--和l++直到找到base>q[l]和base[r]<q[r]，判断（l<r）即两个指针没有错位，此时交换swap(q[l],q[r]),while循环下去直到(l>=r),循环接收此时大于base的数全在其后面，小于base的数全在前面，再对base两边数组进行递归快排，`quick_sort(q,start,r)`和`quick_sort(q,r+1,end);`(这里有几个注意点，开始和结尾的start和end不能变，中间的r和r+1必须用别名且只能用r避免死循环，即我们只要死记硬背即可)；（见java版代码）
>
> ![image-20230320122151660](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230320122151660.png)

```c

#include <iostream>
#include "algorithm"


using namespace std;
const int N = 1e6 + 10;
int n;
int q[N];

void quick_sort(int q[], int l, int r) {
    if (l >= r)return;
    int x = q[l], i = l - 1, j = r + 1;
    while (i < j) {
        do i++; while (q[i] < x);
        do j--; while (q[j] > x);
        if (i < j)swap(q[i], q[j]);
    }
    quick_sort(q, l, j); //这里的r一定要等于j而不能等于i要不然会出现死循环，因为如果x取q[l],快排范围(l~i)如果刚好i=l-1=a[l],满足不了q[i+1] < x(q[l])条件，会导致左端数组越界和死循环，相同如果x = q[r] ,为quick_sort(q, l, j-1);避免右端数组越界。我们可以这样理解，如果x取哪边，我们的范围就往边界拉大一点，x = q[l],递归范围(l~j)(j+1,r);如果x = q[r],递归范围(l~i)(i+1,r);要么x = q[l+r>>1]避免边界问题；
    quick_sort(q, j + 1, r);
}


int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; ++i) {
        scanf("%d", &q[i]);
    }
    quick_sort(q, 0, n - 1);
    for (int i = 0; i < n; ++i) {
        printf("%d ", q[i] );
    }
    return 0;
}

```

```java
public class QuickSort {//快排
	private void swap(int[] arr, int i, int j) {
		int temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
	}
	
	public void quickSort(int[] arr, int start, int end) {
		if (start >= end)
			return;
		int base = arr[start];
		int i = start, j = end;
		while (i != j) {
			while (i < j && arr[j] >= base)
				--j;
			swap(arr, i, j);
			while (i < j && arr[i] <= base)
				++i;
			swap(arr, i, j);
		}
		quickSort(arr, start, i - 1);
		quickSort(arr, i + 1, end);
	}
	
	public static void main(String[] args) {
		int[] arr = {5, 2, 6, 9, 1, 3, 4, 8, 7, 10};
		new QuickSort().quickSort(arr, 0, arr.length - 1);
		System.out.println(Arrays.toString(arr));
	}
}
```



### 归并排序（分治算法）

![image-20221104201824504](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221104201824504.png)

> 题解：
>
> ​       归并排序就是`先递归排序`，把一个数组分成两个数组a,b，再从两个数组左端开始设置指针i,j;i指向a的左端，j指向b的左端；取a[i]和b[j]指针较小的数存入容器tmp[]当中，然后存入较小的数的指针往下移,继续和另外的数组指针比较，小的存入数组tmp[]，直到一方指针全部遍历，剩下的一方将未遍历的数组接在tmp[]后，

![image-20221103212039696](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221103212039696.png)

```c

#include <iostream>


using namespace std;
const int N = 1e6 + 10;
int n;
int q[N], tmp[N];

void merge_sort(int q[], int l, int r) {
    if (l >= r)return;
    int mid = r + l >> 1;
    merge_sort(q, l, mid), merge_sort(q, mid + 1, r);
    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r) {
        if (q[i] <= q[j])tmp[k++] = q[i++];
        else tmp[k++] = q[j++];
    }
    while (i <= mid)tmp[k++] = q[i++];
    while (j <= r)tmp[k++] = q[j++];
    for (i = l, j = 0; i <= r; i++, j++) {
        q[i] = tmp[j];
    }


}

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; ++i) {
        scanf("%d", &q[i]);
    }
    merge_sort(q, 0, n - 1);
    for (int i = 0; i < n; ++i) {
        printf("%d ", q[i]);
    }
    return 0;
}

```

输出和题目和上一题快速排列相同；

### 冒泡排序

> 冒泡排序是一种基础的排序算法，它的原理是通过相邻元素之间的比较和交换，将较小的元素往前移动，较大的元素往后移动，逐步将序列中最大的元素移到序列末尾，最终得到有序的序列。
>
> 冒泡排序就是通过双层循环遍历n-1遍数组，每次遍历都将数组前n-i最大或最小的数放在数组最后，当遍历了n-1遍之后数组排序成功
>
> 

```java
public class BubbleSort {
    public static void main(String[] args) {
        int[] arr = {5, 2, 9, 1, 5, 6};
        bubbleSort(arr);
        System.out.println(Arrays.toString(arr));
    }

    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) //这里只能循环到n-1防止数组越界
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {//与+1作比较，和前面循环到n-1是为了防止数组越界
                    // 交换 arr[j] 和 arr[j+1]
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }
    }
}

```



### 二分法

使用题型：`具有单调性的数组，查找某一字段或数值` 

![image-20221104090613359](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221104090613359.png)

> 二分法有两个模板，本质上是为了`排除死循环问题`
>
> 1. 寻找一个条件x将数组分为两个部分
> 2. 定义mid = (r+l)/2或（r+l+1)/2 `如果l = mid;则mid= r+l+1>>1;`
> 3. 判断a[mid]是否满足条件，然后更新相应的左右端口

`整数二分` 需要考虑上下取整问题，实数二分不需要`二分法的两个模板`：本质上就一个`mid = (r+l)/2或（r+l+1)/2的区别`

![image-20221104110439021](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221104110439021.png)

> `用二分法查找最大值`
>
> ```c
> while (l < r) {//查找小于等于x的最大值
>      int mid = r + l + 1 >> 1;
>      if (a[mid] <= x)l = mid;//查找最大值记忆其判断条件为小于等于（即a[mid]<=x）
>     我们可以假设a[mid]是查找的结果，我们需要查找a[mid]最大值，因此直接写小于等于：a[mid] <= x为条件判断即可；是一个快捷记忆的技巧；
>      else r = mid - 1;
>  }
> ```
>
> `用二分法查找最小值` 
>
> ```c
> while (l < r) {//查找大于等于x的最小值
>      int mid = r + l >> 1;
>      if (a[mid] >= x)r = mid;//查找最小值判断条件为大于等于其最小值
>   查找大于等于x最小值条件就是a[mid]>=x，快捷记忆
>      else l = mid + 1;
>  }
> ```
>
> 

题目：

![image-20221104105115222](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221104105115222.png)

> ![image-20221104110122721](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221104110122721.png)
>
> 查找左右极端值的模板：
>
> 模板1:
>
> ```c
> while (l < r)//大于等于求最小值，即含相同数区间第一个查找到的元素
>      {
>          int mid = l + r >> 1;
>          if (q[mid] >= x) r = mid; 其实当找到r=mid，我们就可以很快写出l = mid+1：
> 我们可以将其看成以mid为分界线的两个区间，当满足条件时，r = mid的区间为[l,mid],因此如果不满足便是区间[mid+1,r]所以可以很快求出else语句当中l=mid+1;
>          else l = mid + 1;
>      }
> ```
>
> 模板2
>
> ```c
> while (l < r)//小于等于求最大值，即含相同区间最后一个查找到的元素
>          {
>              int mid = l + r + 1 >> 1;//避免死循环l = mid;所以mid+1;
>              if (q[mid] <= x) l = mid;
>              else r = mid - 1;
>          }
> ```
>
> 我是这样理解的：首先在图中标记需要求的x的位置，将a[mid]的位置脑补成可变量，就有：
>
> ![image-20221104110953448](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221104110953448.png)
>
> 同理求最大下标x也是：
>
> ![image-20221104111203711](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221104111203711.png)

```c
#include <iostream>

using namespace std;

const int N = 100010;

int n, m;
int q[N];

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i ++ ) scanf("%d", &q[i]);

    while (m -- )
    {
        int x;
        scanf("%d", &x);

        int l = 0, r = n - 1; //定义左右端点下标
        while (l < r)//寻找最左端第一个等于x的数的下标
        {
            int mid = l + r >> 1;//默认mid = (r+l)/2;如果条件判断之后有l = mid;则补上+1防止死循环
            if (q[mid] >= x) r = mid;//求左区间模板1
            else l = mid + 1;
        }
        //r==l时循环退出 如果a[]数组中存在等于x的值，则此时q[r]=q[l]=x
        if (q[l] != x) cout << "-1 -1" << endl;//因为二分一定会返回值，如果返回的值不为x,a[]则不存在x值
        else
        {
            cout << l << ' ';

            int l = 0, r = n - 1;
            while (l < r)//寻找最右端的最后一个等于x的数的下标
            {
                int mid = l + r + 1 >> 1;//先默认mid = (r+l)/2;如果条件判断之后有l = mid;则补上+1
                if (q[mid] <= x) l = mid;//存在l=mid,mid+1;右区间模板2
                else r = mid - 1;
            }

            cout << l << endl;
        }
    }

    return 0;
}


```

```java
import java.util.Scanner;

public class Main {

    static int[] a = new int[100010];
    static int n, q, k;

    public static void main(String[] args) {
        Scanner cin = new Scanner(System.in);

        n = cin.nextInt();
        q = cin.nextInt();

        for (int i = 0; i < n; i++) {
            a[i] = cin.nextInt();
        }

        for (int i = 0; i < q; i++) {
            int l = 0;
            int r = n - 1;
            k = cin.nextInt();
            sort(l, r);
        }
    }

    private static void sort(int l, int r) {
        for (int i = 0; i < n; i++) {
            if (a[i] == k) break;
            else {
                if (i != n - 1) continue;
                System.out.println("-1 -1");
                return;
            }
        }

        while (l < r) {
            int mid = l + r >> 1;
            if (a[mid] >= k) r = mid;
            else l = mid + 1;
        }
        System.out.print(l);

        l = 0;//这里要注意，l和r需要初始化
        r = n - 1;
        while (l < r) {
            int mid = r + l + 1 >> 1;
            if (a[mid] <= k) l = mid;
            else r = mid - 1;
        }
        System.out.println(" " + l);
    }
}
```

### 前缀和

> ​		`前缀和`就是定义一个数组S[],在已知存在的数组a[]当中，s[i]等于a[i]~a[0]的值相加，我们可以利用此前缀和数组去求a[r]~a[l]区间数组的和
>
> ​		即a[l]~a[r]的和=s[r-(l-1)],举个例子a[1]~a[10]的和 = s[10]-s[0],我们一般定义s[0]==0方便计算；
>
> ![image-20221106102533620](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106102533620.png)
>
> ​		`注意：`前缀和数组定义是从1开始的默认s[0]=0,因此数组也必须默认从1开始赋值，否则会出现s[0]=a[0]非空的情况，因此有关前缀和问题的数组a[i]从赋值等都是从i=1开始到i=n结束  `for（i = 1;i<=n;i++）`

```c
前缀和数组的定义和赋值：
for (int i = 1; i <= n; ++i) {//这里从1开始表示s[1]有值，且s[0]默认为0
        s[i] = s[i - 1] + a[i]; 我们定义s[i]从1开始还有一个原因，为了防止i-1出现数组越界问题，即在后面的循环问题当做涉及到i-1的问题一般都从1开始，未涉及的一般从0开始
    }
```

```c
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5 + 10;
int n, m;
int a[N], s[N];

int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; ++i) {//这里特别注意：前缀和数组定义是从1开始的默认s[0]=0,因此数组也必须默认从1开始赋值，否则会出现s[0]=a[0]非空的情况
        scanf("%d", &a[i]);
    }
    for (int i = 1; i <= n; ++i) {//赋值前缀和数组s[]
        s[i] = s[i - 1] + a[i];
    }
    while (m--) {
        int l, r;
        scanf("%d%d", &l, &r);

        printf("%d\n", s[r] - s[l - 1]);//这里要想表示a[l~r]区间的值，避免减去临界值a[l],应该为s[r]-s[l-1]
        
    }

    return 0;
}

```

#### 案例二 K倍区间

![image-20221112110847602](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221112110847602.png)

> 首先进行分析：1~5数组当中K倍区间：
>
> 长度为1：2,4
>
> 长度为2：空
>
> 长度为3：[1,3] [3,5]
>
> 长度为4：[1,4] [2,5]
>
> 长度为5: 空
>
> 分析一下：先两层for循环遍历左右端点l~r,然后通过前缀和求得区间[l~r]的和sum，判断sum%K==0,res++
>
> 输出res

```c
c++代码：
#include <iostream>
#include <algorithm>
#include "cstring"
#include "cstdio"

using namespace std;

const int M = 100010;
int N, K, res, sum;

int a[M], s[M];

int main() {

    scanf("%d%d", &N, &K);

    for (int i = 1; i <= N; ++i) {

        scanf("%d", &a[i]);
    }

    for (int i = 1; i <= N; ++i) {
        s[i] = s[i - 1] + a[i];
    }
    for (int i = 1; i <= N; ++i) {
        for (int j = N; j >= i; --j) {
            int l = i, r = j;
//            cout << "l = " << i << " " << "r = " << j << endl;
            sum = s[r] - s[l - 1];
            if (sum % K == 0) {
                res++;
            }
        }
    }
    printf("%d\n", res);
    return 0;
}


```

```java
java代码：
import java.util.Scanner;


//排列型枚举
public class Main {
    static int N, K, res, sum;
    static final int M = 100010;
    static int[] a = new int[M];
    static int[] s = new int[M];

    public static void main(String[] args) {
        Scanner cin = new Scanner(System.in);

        N = cin.nextInt();
        K = cin.nextInt();

        for (int i = 1; i <= N; i++) {
            a[i] = cin.nextInt();
        }

        //求前缀和
        for (int i = 1; i <= N; i++) {
            s[i] = s[i - 1] + a[i];
        }

        //循环找左右区间
        for (int i = 1; i <= N; i++) {
            for (int j = N; j >= i; j--) {
                int l = i;
                int r = j;
                sum = s[r] - s[l - 1];//前缀和求（l~r）区间和
                if (sum % K == 0) {
                    res++;
                }
            }
        }
        System.out.println(res);
    }
}
```



### 二维前缀和

![image-20221106100646925](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106100646925.png)

> 二维前缀和相当于一维前缀和的推论
>
> 二维前缀和s[i] [j]表示区域a[1~i] [1~j]内的所有值的集合
>
> ![image-20221106103213085](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106103213085.png)
>
> 二维前缀和计算模板：
>
> ![image-20221106101137860](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106101137860.png)
>
> 因为前缀和公式是不用考虑s[0] [0]的，因此数组赋值的话都是直接i=1,j=1开始赋值

> 二维子矩阵前缀和计算公式：
>
> 我们可以利用二维前缀和计算二维数组区域内的值，即
>
> ![image-20221106102104705](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106102104705.png)
>
> 由于我们算的子矩阵s包含了a[x1,y1]和a[x2,y2],因此我们做计算的时候要避免减去这些临界值，因此s=s[x2] [y2]-s[x2] `[y1-1]` - s`[x1-1]` [y2] + s`[x1-1]` `[y1-1]` ,(标红的地方都排除了临界值)
>
> 对二维子矩阵前缀和边界问题的理解
>
> ![image-20221112093815271](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221112093815271.png)

![image-20221106101420703](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106101420703.png)

```c
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1010;
int n, m, q;
int a[N][N], s[N][N];

int main() {
    int n, m, q;
    scanf("%d%d%d", &n, &m, &q);
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            scanf("%d", &a[i][j]);
        }
    }

    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            s[i][j] = s[i][j - 1] + s[i - 1][j] - s[i - 1][j - 1] + a[i][j];
        }//二维前缀和计算公式
    }
    while (q--) {
        int x1, y1, x2, y2;
        scanf("%d%d%d%d", &x1, &y1, &x2, &y2);
        printf("%d\n", s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1]);


    }
    return 0;
}

```

### 差分

> 即存在一个数组a[],差分就是存在一个数组b[]使得b[1~n]数组之和等于a[],a[]为b[]的前缀和数组，b[]是a[]的差分数组；

```c
一维数组的差分函数：
void insert(int l, int r, int c)
{
    b[l] += c;
    b[r + 1] -= c;
}
```

![image-20221106111401111](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106111401111.png)

```c
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1e5+10;
int n, m;
int a[N], b[N];

void insert(int l, int r, int c) {//差分函数
    b[l] += c;
    b[r + 1] -= c;
}

int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; ++i) {
        scanf("%d", &a[i]);
    }
    for (int i = 1; i <= n; ++i) {
        insert(i, i, a[i]);//差分函数的初始化，如果输入的l和r都是相同的数，则相当于在b[l]插入一个c，b[k+1]减去一个c将b[]初始化
    }

    while (m--) {
        int l, r, c;
        scanf("%d%d%d", &l, &r, &c);
        insert(l, r, c);
    }
    for (int i = 1; i <= n; ++i) {
        b[i] += b[i - 1];
    }
    for (int i = 1; i <= n; ++i) {
        printf("%d ", b[i]);
    }
    return 0;
}

```

### 二维差分

> 二维差分利用可以在区域内加上或减去固定的数

```c
二维数组的差分函数：
    void insert(int x1, int y1, int x2, int y2, int c)
{
    b[x1][y1] += c;
    b[x2 + 1][y1] -= c;
    b[x1][y2 + 1] -= c;
    b[x2 + 1][y2 + 1] += c;
}
```

过于复杂，我们只需要了解其函数模板即可

### 双指针

暴力算法for循环嵌套复杂度O(n^2)：

![image-20221106145756398](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106145756398.png)

> 优化为双指针算法复杂度O(n),看上去for里面嵌套一个while（）循环，但j永远不超过i的范围；i最多走n步，那么j最多也走n次，总共为2n次。因此复杂度为O(n):

双指针模板：

![image-20221106145631571](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106145631571.png)

> 能用双指针算法解出来的问题都能用暴力算法解出，双指针算法类似于暴力算法的优化，即我们可以先写出其暴力算法然后`判断两个循环变量i和j有没有单调对应关系`，如果存在单调关系则可以使用双指针代码模板优化，简化其时间复杂度到O(n);

例题：最长连续不重复子序列

![image-20221106153845219](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106153845219.png)

> ![image-20221106154017485](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106154017485.png)
>
> ![image-20221106161719309](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106161719309.png)
>
> 这里重点是`使用一个工具类s[]用来存取a[j~i]内出现相同数字的次数`，当a[i++]时， s[q[i]]++;

```c
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;


const int N = 1e5 + 10;

int n;
int q[N], s[N];

int main() {
    scanf("%d", &n);
    for (int i = 0; i < n; ++i) {
        scanf("%d", &q[i]);
    }
    int res = 0;
    for (int i = 0, j = 0; i < n; ++i) {
        s[q[i]]++;//s[]用来存取a[j~i]内出现相同数字的次数
        while (j < i && s[q[i]] > 1)s[q[j++]]--;//s[q[j++]]--相当于在a[j~i]中j指针的元素次数-1，然后j++,相当于依次删除a[]数组左端的值
        res = max(res, i - j + 1);
    }
    cout << res << endl;
    return 0;
}

```

### 位运算

![image-20221106164001666](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106164001666.png)

> 一个二进制数n下标是从低位开始计数
>
> 如果需要求一个二进制第k位数是多少，先向右位移k然后&1得出结果

```c
int main() {
    int n = 10;
    for (int k = 3; k >= 0; --k) {
        cout << (n >> k & 1);
    }
    return 0;
}
得出的是10的四位二进制数表示
1010
```

> lowbit(x) 返回x二进制数的最后一位1,相当于x&(~x+1)=x&(-x)

### 离散化

![image-20221106202729589](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106202729589.png)

> 离散化的本质是建立了一段数列到自然数之间的映射关系（value -> index)，通过建立新索引，来缩小目标区间，使得进行一系列运算更加简洁快捷
>
> 离散化首先需要排序去重：
>
> ![image-20221106203755308](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106203755308.png)
>
> ```c
> 1. 排序：sort(alls.begin(),alls.end())
> 2. 去重：alls.earse(unique(alls.begin(),alls.end()),alls.end());
> ```
>
>  `unique()函数的底层原理`
>
> ```c
> vector<int>::iterator unique(vector<int> &a) {
>     int j = 0;
>     for (int i = 0; i < a.size(); ++i) {
>         if (!i || a[i] != a[i - 1])//如果是第一个元素或者该元素不等于前一个元素，即不重复元素，我们就把它存到数组前j个元素中
>             a[j++] = a[i];//每存在一个不同元素，j++
>     }
>     return a.begin() + j;//返回的是前j个不重复元素的下标
> }
> ```
>
> 

![image-20221106202733937](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106202733937.png)

> 由于本题可能有多组数据是针对同一个数组下标操作的，因此我们可以将所有用到的数组下标装在一个下标容器alls内去重，然后再逐一为相同的数组下标增加数值c，再通过对应前缀和相减求得区间 l~r 之间的数的值

![image-20221106211106354](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221106211106354.png)

```c
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

typedef pair<int, int> PII;

const int N = 300010;

int n, m;
int a[N], s[N];

vector<int> alls;//存入下标容器
vector<PII> add, query;//add增加容器，存入对应下标和增加的值的大小
//query存入需要计算下标区间和的容器
int find(int x)
{
    int l = 0, r = alls.size() - 1;
    while (l < r)//查找大于等于x的最小的值的下标
    {
        int mid = l + r >> 1;
        if (alls[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return r + 1;//因为使用前缀和，其下标要+1可以不考虑边界问题
}

int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i ++ )
    {
        int x, c;
        cin >> x >> c;
        add.push_back({x, c});//存入下标即对应的数值c

        alls.push_back(x);//存入数组下标x=add.first
    }

    for (int i = 0; i < m; i ++ )
    {
        int l, r;
        cin >> l >> r;
        query.push_back({l, r});//存入要求的区间

        alls.push_back(l);//存入区间左右下标
        alls.push_back(r);
    }

    // 区间去重
    sort(alls.begin(), alls.end());
    alls.erase(unique(alls.begin(), alls.end()), alls.end());

    // 处理插入
    for (auto item : add)
    {
        int x = find(item.first);//将add容器的add.secend值存入数组a[]当中，
        a[x] += item.second;//在去重之后的下标集合alls内寻找对应的下标并添加数值
    }

    // 预处理前缀和
    for (int i = 1; i <= alls.size(); i ++ ) s[i] = s[i - 1] + a[i];

    // 处理询问
    for (auto item : query)
    {
        int l = find(item.first), r = find(item.second);//在下标容器中查找对应的左右两端[l~r]下标，然后通过下标得到前缀和相减再得到区间a[l~r]的和
        cout << s[r] - s[l - 1] << endl;
    }

    return 0;
}


```

### 区间合并

![image-20221107210322466](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221107210322466.png)

例题：

![image-20221107210423197](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221107210423197.png)

> 将所有的区间装入pair容器排序再输出（离散化）
>
> 依次输出排序后的区间，更新对应的区间存入工具容器res
>
> 将工具容器res更新到原容器segs,输出大小就是独立区间的个数

```c
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

typedef pair<int, int> PII;

void merge(vector<PII> &segs)
{
    vector<PII> res;//创建一个工具类容器存储区间

    sort(segs.begin(), segs.end());//区间排序，默认对pair容器按照左端点进行排序，不会出现下一个数组左端点超过前一个数组左端点的情况

    int st = -2e9, ed = -2e9;//设置两个默认无穷小的值，因为-2e9超出范围系统认定为无穷小
    for (auto seg : segs)//输出左端点从小到大排序完的数，排序后输出的好处是避免后面又出现重复区间从而重新维护前面的区间
        if (ed < seg.first)//不在一个区间的情况
        {
            if (st != -2e9) res.push_back({st, ed});//更新左右区间，如果不在一个区间加入容器
            st = seg.first, ed = seg.second;
        }
        else ed = max(ed, seg.second);//在一个区间内或是交叉区间，我们只需要更新右区间即可

    if (st != -2e9) res.push_back({st, ed});//特判st != -2e9排除空区间，然后将最后一个区间更新到容器（pair相同的first元素不同second会覆盖前者，因此不用考虑最后一个区间之前重复加入问题）

    segs = res;//更新容器
}

int main()
{
    int n;
    scanf("%d", &n);

    vector<PII> segs;
    for (int i = 0; i < n; i ++ )
    {
        int l, r;
        scanf("%d%d", &l, &r);
        segs.push_back({l, r});//运用到区间合并的题目，由于会用到重复的区间，因此我们需要先将所有区间存储起来进行sort左区间排序
    }

    merge(segs);

    cout << segs.size() << endl;

    return 0;
}


```



第二章 动态规划

背包问题：

> 0 1背包：每件物品最多只能用一次
>
> 完全背包：每件物品有无限个
>
> 多重背包：每件物品i有不同数量Si（存在优化）
>
> 分组背包:不同物品分为不同组，比如水果和蔬菜，你在水果那组选了苹果就不能选葡萄了，组之间存在互斥关系

1、01背包问题

![image-20230305101804158](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230305101804158.png)
