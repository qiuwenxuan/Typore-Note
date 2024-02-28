###  1.判断质数`循环`

```c++

#include "cstdio"
#include "iostream"
using namespace std;
int main() {
    int n1 = 0, n2 = 1, n3;//n3 = 1,n4 = 2,n5 = 3
    int N;
    int i = 1;
    cin >> N;
    if (N == 1) printf("%d",n1);
    else if (N == 2) {
        printf("%d %d",n1,n2);
    } else if (N >= 3) {
        printf("%d %d",n1,n2);
        while (i <= N - 2) {
            n3 = n1 + n2;
            n1 = n2;
            n2 = n3;
            i++;
            printf(" %d",n3);
        }
    }

    return 0;
}

```

### 2.打印菱形（曼哈顿距离）`循环`

> 曼哈顿距离：|x1-x2|+|y1-y2|
>
> 输出对称菱形，先找出中心点坐标
>
> 如图：大小为5的菱形
>
> ![image-20221009164014932](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221009164014932.png)
>
> 每个空格对中心店的曼哈顿距离
>
> ![image-20221009164115767](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221009164115767.png)
>
> 得出规律：曼哈顿距离小于等于2的为图形格，即<=图形大小5/2向下取整
>
> 所以我们只需要将判断曼哈顿距离d为 d<=菱形大小/2输出`*`
>
> ==abs(i - cx) + abs(j - cy) <= n / 2==

```c++
#include <iostream>

using namespace std;

int main()
{
    int n;
    cin >> n;

    int cx = n / 2, cy = n / 2;

    for (int i = 0; i < n; i ++ )
    {
        for (int j = 0; j < n; j ++ )
            if (abs(i - cx) + abs(j - cy) <= n / 2)
                cout << '*';
            else cout << ' ';
        cout << endl;
    }

    return 0;
}
输出：
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

### 3.求质数的算法减少时间复杂度

![image-20221011231640411](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221011231640411.png)

```java
#include <iostream>

using namespace std;

int main() {
    int N, sum;
    cin >> N;
    while (N--) {
        int X, sum = 0;
        cin >> X;
        for (int i = 1; i * i <= X; ++i) {//这里做循环优化
            if (X % i == 0) {
                sum += i;
                if (X / i != X) {
                    sum += X / i;
                }
            }
        }
        if (sum == 1) {
            printf("%d is not perfect\n", X);
        } else if(sum == X){
            printf("%d is perfect\n", X);
        }else{
            printf("%d is not perfect\n", X);
        }
    }

    return 0;
}
这道题无非就是先求出数X的所有质数再相加判断是否等于X本身，等于则为完全数，如果直接遍历，会出现时间复杂度过大超时，因此我们可以对for循环做简要优化；
    我们在求一个数X的公因子可以不用遍历1~X所有数，如果a是X的公因子且a>X，则b=X/a一定是X的另一个大于X/2的公因子，因此我们只需要遍历a<=X/a的所有公因子，则可求出a和b=X/a的X的所有公因子，即for (int i = 1; i <= X/i; ++i),注意这里要判断X / i != X即X不为它本身，以免计入重复值;
```

### 4.平方矩阵`二维数组` `循环`

![image-20221014114332921](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221014114332921.png)

![image-20221014114350394](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221014114350394.png)

方法一：

![image-20221014115038890](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221014115038890.png)

定义对角线为1，以其为轴向两边+1递增

```c
#include <iostream>
#include "cstdio"

using namespace std;
int q[100][100];

int main() {
    int N;
    while (cin >> N, N) {//无限循环，只要有输入就为true进入循环，当N=0时不进入循环
        for (int i = 0; i < N; ++i) {
            q[i][i] = 1;//先定义对角线，使对角线值为1
            for (int j = i + 1, k = 2; j < N; ++j, ++k) q[i][j] = k;//定义同一行数组逐渐+1
            for (int j = i + 1, k = 2; j < N; ++j, ++k) q[j][i] = k;//定义同一列数组逐渐+1

        }
        for (int i = 0; i < N; ++i) {
            for (int j = 0; j < N; ++j) {
                printf("%d ", q[i][j]);
            }
            printf("\n");
        }
        printf("\n");
    }

    return 0;
}

```

方法2：先定义对角线，再往其左右依次递增遍历

![image-20221014144441093](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221014144441093.png)

```java


#include <iostream>
#include "cstdio"

using namespace std;
int q[100][100];

int main() {
    int N;
    while (cin >> N, N) {//当输入行为 N=0 时，表示输入结束，且该行无需作任何处理。
        for (int i = 0; i < N; ++i) {
            q[i][i] = 1;
            for (int j = i + 1, k = 2; j < N; ++j, ++k) {//往对角线右边遍历递增
                q[i][j] = k;
            }
            for (int j = i - 1, k = 2; j >= 0; --j, ++k) {//往对角线左边遍历递增
                q[i][j] = k;
            }
        }
        for (int i = 0; i < N; ++i) {
            for (int j = 0; j < N; ++j) {
                printf("%d ", q[i][j]);
            }
            printf("\n");
        }
        printf("\n");
    }


    return 0;
}

```

方法三：按规律`q[i][j] = abs(i-j)+1`来给数组赋值



### 5.蛇形矩阵`二维数组`

![image-20221014155515547](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221014155515547.png)

![image-20221014162337035](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221014162337035.png)

> 题解：
>
> ![image-20221019223623316](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221019223623316.png)
>
> 本题需要掌握`预变量`的应用，即不能判定接下来是否能够满足执行条件，便先定义预变量进行下一步并判断，如果的确不满足，再修改使其满足条件执行下一步。

重要的一点：创建位移变量d,`x = x+dx[x]`;`y = y+dy[y]`;

```c

#include <iostream>

using namespace std;

const int N = 110;

int n, m;
int q[N][N];

int main() {
    cin >> n >> m;
    int dx[] = {-1, 0, 1, 0}, dy[] = {0, 1, 0, -1};//当d==0时，sx[d],dy[d] = (-1,0)表示向右走坐标变化为 a = x + dx[d], b = y + dy[d];当d==1时向下;d==2向左；d == 3向上
   
    for (int x = 0, y = 0, d = 0, k = 1; i <= n * m; ++i) {
        q[x][y] = k;
        int a = x + dx[d], b = y + dy[d];
        if (a < 0 || a >= n || b < 0 || b >= m || q[a][b]) {//判断是否越界或者重复进入数组
            d = (d + 1) % 4;//转弯用d+1表示
            a = x + dx[d], b = y + dy[d];
        }
        x = a, y = b;
    }
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < m; ++j) {
            cout << q[i][j] << " ";
        }
        cout << endl;
    }
   
    return 0;
}

```

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main07 {
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static int n, m;
    static int[] dx = {0, 1, 0, -1};
    static int[] dy = {1, 0, -1, 0};
    static int[][] q = new int[10010][10010];

    public static void main(String[] args) throws IOException {
        String[] s = br.readLine().split(" ");
        n = Integer.parseInt(s[0]);
        m = Integer.parseInt(s[1]);

        for (int i = 1, x = 0, y = 0, d = 0; i <= n * m; i++) {
            q[x][y] = i;
//            System.out.print("q[x][y]:" + q[x][y]);
            int a = x + dx[d];
            int b = y + dy[d];
            if (judge(a, b) == false) {

//                System.out.print(judge(a, b));
                d = (d + 1) % 4;
                a = x + dx[d];
                b = y + dy[d];
            }
            x = a;
            y = b;
//            System.out.println("(" + x + "," + y + ") ");

        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                System.out.print(q[i][j] + " ");
            }

            System.out.println();
        }
    }

    private static boolean judge(int a, int b) {
        if (a >= n || a < 0 || b >= m || b < 0) {//数组越界
            return false;
        }
        if (q[a][b] != 0) return false;
        return true;
    }
}

```



### 6.忽略大小写比较字符串 `字符串` `foreach循环`

![image-20221018095534264](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221018095534264.png)

```c

int main() {
    string a,b;
    getline(cin,a);
    getline(cin,b);
    for(auto &c:a) c = tolower(c);
    for(auto &c:b) c = tolower(c);//使用foreach取地址改变原始a,和b
    if(a == b) cout << '=';
    if(a > b) cout << '>';
    if(a < b) cout << '<';

    return 0;
}

```

### 7.循环相克（狗熊，猎人和枪) `字符串`

![image-20221018102647342](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221018102647342.png)

> 题解：
>
> 猎人怕狗熊，狗熊怕枪，枪怕猎人
>
> 我们可以定义猎人为0，狗熊为1，枪为2
>
> 因此可以用0<1,1<2,2<0的大小关系来表示相克关系
>
> 即定义一个x,y代表两个玩家，当`x=(y+1)%3`时刚好为相克关系，此时play1获胜，以此推类

```c
#include <iostream>
#include "cstdio"
#include "cstring"
#include <string.h>

using namespace std;

//Hunter 0, Bear 1, Gun 2
int main() {
    int T, a, b;
    cin >> T;
    while (T--) {
        string x, y;
        cin >> x >> y;
        if (x == "Hunter") {
            a = 0;
        } else if (x == "Bear") {
            a = 1;
        } else {
            a = 2;
        }
        if (y == "Hunter") {
            b = 0;
        } else if (y == "Bear") {
            b = 1;
        } else {
            b = 2;
        }
        if (a == (b + 1) % 3) {
            cout << "Player1" << endl;
        } else if (b == a) {
            cout << "Tie" << endl;
        } else {
            cout << "Player2" << endl;
        }
    }

    return 0;
}

```

### 8.去掉多余的空格 `双指针` `字符串`

![image-20221018113236575](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221018113236575.png)

> 题解1（双指针解法）：
>
> ![image-20221018113251984](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221018113251984.png)
>
> 循环遍历字符串，如果a[i]!=' ',b+=a[i];
>
> 如果a[i]==' ',再创建一个指针循环 j = i ，如果s[j+1] 仍然等于空格' ',遍历下一个直到不是空格为止，此时令i=j;i跳过所有空格

```c
#include <iostream>

using namespace std;

int main()
{
    string s;
    getline(cin, s);

    string r;
    for (int i = 0; i < s.size(); i ++ )
        if (s[i] != ' ') r += s[i];
        else
        {
            r += ' ';
            int j = i;
            while (j < s.size() && s[j] == ' ') j ++ ;//创建双指针跳过空格,注意条件j不能越界超过字符串最大长度
            i = j - 1;
        }

    cout << r << endl;

    return 0;
}

```

> 题解2：利用cin过滤多余的空格

```c
using namespace std;

int main()
{
    string s;
    while (cin >> s) cout << s << ' ' ;

    return 0;
}
```

### 9.单词替换 `字符串` `字符串输出流`

![image-20221019155429757](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221019155429757.png)

> 题解：引入输入流stream，将s放在输入流ssin内依次读出与a做对比，相同则在后面输出b，不相同则输出a

```c
#include "iostream"
#include "sstream"//记得引入输入流头文件

using namespace std;

int main() {

    string s, a, b;
    getline(cin, s);
    cin >> a >> b;
    stringstream ssin(s);
    string str;
    while (ssin >> str) {
        if (str == a) cout << b << ' ';
        else cout << str << ' ';
    }
    return 0;
}

```

> 重点解析：		
>
> ​		本题需要掌握的是"sstream"库函数里面的stringstream输入流，定义一个stringstream ssin类似于cin用法，读入一个满足要求的字符串。
>
> ​		`~a`等价于`a!=-1`

### 10.最长单词

![image-20221020154437159](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221020154437159.png)

> 题解：
>
> ​		本题需学会str.back()和str.pop_back()函数就可以了；
>
> ​		还需要注意cin函数读入字符串遇到空格暂停；一般用while(cin>>str)从含多个字符串的句子中读入单个字符串

```c

#include <iostream>

using namespace std;

int main() {
    string res, str;
    while (cin >> str) {
        if (str.back() == '.')str.pop_back();//str.back()函数表示获取字符串最后一个字符，str.pop_back()表示删除字符串最后一行字符；
        if (str.size() > res.size())res = str;
    }
    cout << res << endl;
    return 0;
}

```

### 11.字符串移位包含问题 `字符串匹配` 

![image-20221023164144850](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221023164144850.png)

> 题解：
>
> ​		`判断两个字符串的包含关系：`
>
> ```c
> for(int i = 0, j = 0; i + b.size() <= a.size(); ++i){
> 	for(; j < b.size(); j++){
> 		if(a[i+j]!=b[j]) break;			
> 	}
> 	if(j==b.size()){
> 		puts("true");
> 		return 0;
> 	}
> }
> 
> ```
>
> ![image-20221023164223185](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221023164223185.png)

```c

#include <iostream>
using namespace std;

int main() {
    string a, b;
    cin >> a >> b;
    if (a.size() < b.size())swap(a, b);
    for (int i = 0; i < a.size(); ++i) {
        a = a.substr(1) + a[0];
        for (int j = 0, k = 0; j + b.size() <= a.size(); ++j) {
            for (; k < b.size(); ++k) {
                if (a[k + j] != b[k]) break;
            }
            if (k == b.size()) {
                puts("true");
                return 0;
            }
        }
    }
    puts("false");
    return 0;
}

```

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;


public class Main01 {
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

    public static void main(String[] args) throws IOException {
        String[] s = br.readLine().split(" ");
        String str1 = s[0];
        String str2 = s[1];
        int n = str1.length();
        String str3 = str1;
        System.out.println(str3);
        for (int i = 0; i < n; i++) {
            str3 = str3.substring(1, n) + str1.charAt(0);
            System.out.println(str3);
            if (judge(str3, str2)) {
                System.out.println(true);
                return;
            }
        }
        System.out.println(false);
    }

    private static boolean judge(String str3, String str2) {
        int n1 = str3.length();
        int n2 = str2.length();
        for (int i = 0, j = 0; n1 - i >= n2 - j; i++) {
            if (str3.charAt(i) == str2.charAt(j)) {
                j++;
            }
            if (j == n2) {
                return true;
            }
        }
        return false;
    }
}

```



### 12.字符串最大跨距`字符串匹配  经典`

![image-20221026091920918](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221026091920918.png)

> 题解：
>
> ​		利用两次字符串匹配查找两端符合的最远跨距
>
> ​		首先寻找最左端left的匹配的字符串的位置，循环i从0开始到s.size()结束，此时找到的第一个匹配位置即为最左端位置left，退出循环；
>
> ​		再往字符串s右边开始寻找right，循环变量i从s.size()-1开始到0结束，这样找到的第一个即为最右匹配字符的位置right，退出循环；
>
> ​		最后通过计算求出最远跨距。

```c
#include <iostream>
using namespace std;

int main() {
    string s, s1, s2;

    getline(cin,s,',');
    getline(cin,s1,',');
    getline(cin,s2);//输入方法，遇到','分割存入字符串


    if (s.size() < s1.size() || s.size() < s2.size()) puts("-1");
    else {
        int l = 0;
        while (l + s1.size() <= s.size()) {
            int k = 0;
            while (k < s1.size()) {
                if (s[l + k] != s1[k]) break;
                k++;
            }
            if (k == s1.size()) break;
            l++;
        }

        int r = s.size() - s2.size();
        while (r >= 0) {
            int k = 0;
            while (k < s2.size()) {
                if (s[k + r] != s2[k])break;
                k++;
            }
            if (k == s2.size())break;
            r--;
        }

        l += s1.size() - 1;
        if (l >= r)puts("-1");
        else {
            printf("%d\n", r - l - 1);
        }
    }
    return 0;
}

```

### 13.高精度加法 `STL容器 函数`

![image-20221105193619015](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221105193619015.png)

> `高精度加法`模板：
>
> main函数部分：
>
> ![image-20221105194444979](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221105194444979.png)
>
> 函数部分：
>
> ![image-20221105194855640](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221105194855640.png)
>
> `高精度减法`模板：
>
> ![image-20221105203321513](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221105203321513.png)

```c
#include <iostream>
#include "vector"

using namespace std;

vector<int> add(vector<int> &A, vector<int> &B) {
    vector<int> C;

    int t = 0;
    for (int i = 0; i < A.size() || i < B.size(); ++i) {
        if (i < A.size())t += A[i];
        if (i < B.size())t += B[i];

        C.push_back(t % 10);
        t /= 10;
    }
    if (t)C.push_back(1);
    return C;
}

int main() {
    string a, b;
    vector<int> A, B;

    cin >> a >> b;

    for (int i = a.size() - 1; i >= 0; i--) {
        A.push_back(a[i] - '0');
    }
    for (int i = b.size() - 1; i >= 0; i--) {
        B.push_back(b[i] - '0');
    }
    auto C = add(A, B);

    for (int i = C.size() - 1; i >= 0; i--) {
        cout << C[i];
    }
    return 0;
}

```

### 14.前缀和 `前缀和思想`

![image-20221105204909219](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20221105204909219.png)

> 

15.进制转换（含手动偏移）

![image-20230306193308704](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230306193308704.png)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main03 {
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static String ss = "0123456789ABCDEFG";

    public static void main(String[] args) throws IOException {
        String[] s = br.readLine().split(" ");
        int num = Integer.parseInt(s[0]);
        int n = Integer.parseInt(s[1]);

        String res = exChange(num, n);
        System.out.println(res);
    }

    private static String exChange(int num, int n) {
        if (num == 0) {
            return "";
        }
        return exChange(num / n, n) + ss.charAt(num % n);
    }
}

```



### 15.循环取数组的每一位 `模板`



```java
for (int i = 0; i <= n; ++i) {
        int x = i;
        while (x) {
            int t = x % 10;//t为当前数字最后一位的位数
            x /= 10;
        }
    }
```



### 16、把字符串变成数字`模板`

```c
'2019'->2019
int x = 0;
for(int i = 0;i<=str.size();i++){
	x = x*10 + str[i]-'0';
}
```

### 17.回字排列（移动距离） `求曼哈顿距离`

![image-20230313203631776](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230313203631776.png)

> ![image-20230313204859597](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230313204859597.png)

```c
import java.util.Map;
import java.util.Scanner;

//import math;
public class Main {
    static int w, m, n;//w为排号宽度，m、n为计算的楼号
    static int x1, y1, x2, y2;

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        w = scan.nextInt();
        m = scan.nextInt();
        n = scan.nextInt();

        m--;
        n--;

        x1 = m / w;
        y1 = m % w;
        x2 = n / w;
        y2 = n % w;

        if (x1 % 2 != 0) y1 = w - y1 - 1;
        if (x2 % 2 != 0) y2 = w - y2 - 1;

        int sum = Math.abs(x1 - x2) + Math.abs(y1 - y2);
        System.out.println(sum);
    }
}

```



### 18、判断闰年的方法 `年月日进制转换`

> 闰年：能被4整除，不能被100整除；
>
> ​			能被400整除；

```c
if(month == 2 && !(year%4) && year%100 || !(year%400))
```

例：高斯日记

![image-20230315143101847](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230315143101847.png)

![image-20230315144258266](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230315144258266.png)

```c
import java.util.Scanner;

public class Main02 {
    static int year, month, day;
    static int[] a = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

    public static void main(String[] args) {

        year = 1777;
        month = 4;
        day = 30;
        Scanner scan = new Scanner(System.in);
        int num = scan.nextInt();
        for (int i = 1; i < num; i++) {
            if ((year % 4 == 0 && year % 100 != 0) || (year % 400 == 0)) {
                a[2] = 29;
            } else {
                a[2] = 28;
            }
            day = day % a[month] + 1;
            if (day == 1) {
                month = month % 12 + 1;
                if (month == 1) {
                    year += 1;
                }
            }
        }
        System.out.printf("%d-%d-%d\n", year, month, day);
    }
}

```

### 19、二分、前缀和 实例 `二分法` `前缀和`

![image-20230306213415724](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230306213415724.png)

> 本题首先拿到手我们考虑暴力求解，即使用三个循环分别遍历A,B,C三个数组，然后判断条件为（a[i]<b[j]<c[k]）成立，方法数res++;输出最终结果即可；由此可见复杂度过高，我们需要换一种思维；
>
> 受方法一的启发，因为b数组可以很好地与A，C建立连接，我们可以先遍历b[i]数组，只需要判断a[i]<b[j]、c[i]>b[j]个数即可，将三层循环简化成两层循环，然后再将方法数相乘求出所有a[i]<b[j]<c[k]的情况；
>
> ​	在方法二的基础上，我们可以先将a[],c[]数组sort()排序得到有序数组，然后再通过二分法或前缀和方法找出a[]、c[]中小于/大于b[]的个数，这样就将二层循环简化成查找的复杂度，复杂度大大降低；
>
> 前缀和求a[]、c[]中小于/大于b[]的个数：
>
> 先存入数组a[],b[],c[],再创建一个计数数组cnt(),for(int i = 0,i<n;i++) cnt[a[i]]++,将数组a[]中与a[i]相同的数的个数用cnt[a[i]]存取；
>
> 然后再将cnt[a[i]]转化为前缀和s[i]，s[i] = s[i-1]+cnt[i];使得s[i]表示a[]当中所有小于i的元素的个数即s[i] = cnt[0]+cnt[1]+...+cnt[i];因此a[]数组中小于任意b[j]的元素个数都可以用s[b[j]]表示；直接通过s[b[j]]得出小于等于b[j]的个数
>
> c[]数组同理
>
> 

```c
#include <iostream>
#include "cstdio"
#include "cstring"
#include"algorithm"

using namespace std;

const int N = 100010;

int n;//数组长度
int a[N], b[N], c[N];//三个数组
int as[N];//a数组cnt的前缀和
int cs[N];//c数组cnt的前缀和
int cnt[N], s[N];//计数数组cnt,前缀和容器数组s[]

int main() {
    scanf("%d", &n);
    //输入三个数组a,b,c
    for (int i = 0; i < n; ++i) {
        scanf("%d", &a[i]);
        a[i]++;

    }
    for (int i = 0; i < n; ++i) {
        scanf("%d", &b[i]);
        b[i]++;

    }
    for (int i = 0; i < n; ++i) {
        scanf("%d", &c[i]);
        c[i]++;

    }

    //求as[]
    for (int i = 0; i < n; ++i) {
        cnt[a[i]]++;
    }
    for (int i = 1; i < N; ++i) {//求a的cnt的前缀和s[]
        s[i] = s[i - 1] + cnt[i];
    }
    for (int i = 0; i < n; ++i) {
        as[i] = s[b[i] - 1];
    }

    //重新清空两个工具数组
    memset(cnt, 0, sizeof cnt);
    memset(s, 0, sizeof s);

    //求cs[]
    for (int i = 0; i < n; ++i) {
        cnt[c[i]]++;
    }
    for (int i = 1; i < N; ++i) {
        s[i] = s[i - 1] + cnt[i];
    }
    for (int i = 0; i < n; ++i) {
        cs[i] = s[N - 1] - s[b[i]];

    }
    //遍历求方法数之和
    int sum = 0;
    for (int i = 0; i < n; ++i) {
        sum += (cs[i] * as[i]);
    }
    printf("%d", sum);
}



```



### 20、螺旋折线 `找规律`

![image-20230317202436142](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230317202436142.png)

> 我们碰到这种题目有几种解决方法，1、模拟法 （类似于蛇形矩阵）2、找规律
>
> 由于该题目数据范围过大，因此我们只能通过找规律来解题
>
> 通过分析题目，得到每条边和各个角的规律
>
> ![image-20230317202630464](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230317202630464.png)

> 每个角的规律与圈数n有关，比如说右上方的角的步数为（2n）^2，又因为该点是右边的起点，因此我们只需要求出该边任一点与右上角点的偏移量就行，就能得出该边上任一点的步数
>

```c
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;

int main()
{
    int x, y;
    cin >> x >> y;

    if (abs(x) <= y)  // 在上方
    {
        int n = y;
        cout << (LL)(2 * n - 1) * (2 * n) + x - (-n) << endl;
    }
    else if (abs(y) <= x)  // 在右方
    {
        int n = x;
        cout << (LL)(2 * n) * (2 * n) + n - y << endl;
    }
    else if (abs(x) <= abs(y) + 1 && y < 0)  // 在下方
    {
        int n = abs(y);
        cout << (LL)(2 * n) * (2 * n + 1) + n - x << endl;
    }
    else  // 在左方
    {
        int n = abs(x);
        cout << (LL)(2 * n - 1) * (2 * n - 1) + y - (-n + 1) << endl;
    }

    return 0;
}

```

### 21.走方格 `dfs递归` `动态规划`

![image-20230327211444715](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230327211444715.png)

第一种方法：dfs（会显示超时）

```c++
#include<iostream>
#include<cstring>
#include<cstdio>
using namespace std;
int st[32][32],dx[2]={0,1},dy[2]={1,0};
bool book[32][32];
int n,m,ans,ss;
void dfs(int x,int y)
{
    if(x>n||y>m)return;
    if(x==n&&y==m)ans++;
    for(int i=0;i<2;i++)
    {
        int xx=x+dx[i],yy=y+dy[i];
        if(book[xx][yy]||(xx%2==0&&yy%2==0))continue;
        if(xx<1||yy<1||xx>n||yy>m)continue;
        book[xx][yy]=true;
        dfs(xx,yy);
        book[xx][yy]=false;
    }
}
int main()
{
    cin>>n>>m;
    dfs(1,1);
    cout<<ans;
    return 0;
}


```

Java代码：

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Main06 {
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static Boolean[][] used;
    static int[] dx = new int[]{0, 1};
    static int[] dy = new int[]{1, 0};
    static int n, m, res;

    public static void main(String[] args) throws IOException {
        String[] s = br.readLine().split(" ");
        n = Integer.parseInt(s[0]);
        m = Integer.parseInt(s[1]);
        used = new Boolean[32][32];
        for (int i = 0; i < used.length; i++) {//给boolean used数组全部赋值为false,否则会出现used为null的异常
            Arrays.fill(used[i], false);
        }

        dfs(1, 1);
        System.out.println(res);
        br.close();
    }

    private static void dfs(int x, int y) {
        if (x > n || y > m) return;
        if (x == n && y == m) res++;

        for (int i = 0; i < 2; i++) {
            int xx = x + dx[i];
            int yy = y + dy[i];

            if (used[xx][yy] == true || (xx % 2 == 0 && yy % 2 == 0)) continue;
            if (xx > n || yy > m || xx < 1 || yy < 1) continue;
            used[xx][yy] = true;
            dfs(xx, yy);
            used[xx][yy] = false;
        }
    }
}

```

第二种方法：动态规划

```c
#include <iostream>
#include <cstring>

using namespace std;

const int N = 40;

int n, m;
int f[N][N];

int main()
{
    cin >> n >> m;
    f[1][1] = 1;
    for (int i = 1; i <= n; i ++ )
        for (int j = 1; j <= m; j ++ )
        {
            if (i == 1 && j == 1) continue;//已经初始化跳过
            if (i % 2 || j % 2)//排除坐标都为偶数的情况
                f[i][j] = f[i - 1][j] + f[i][j - 1];//向下走或向右走
        }

    cout << f[n][m] << endl;

    return 0;
}

```

java代码：

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

//f[i][j]表示走到（i,j）上的方案数count
public class Main07 {
	static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	static int[][] f = new int[40][40];
	static int n, m;

	public static void main(String[] args) throws IOException {
		String[] s1 = br.readLine().split(" ");
		n = Integer.parseInt(s1[0]);
		m = Integer.parseInt(s1[1]);
		f[1][1] = 1;
		for (int i = 1; i <= n; i++) {
			for (int j = 1; j <= m; j++) {
				if (i == 1 && j == 1)
					continue;
				if (!(i % 2 == 0 && j % 2 == 0)) {

					f[i][j] = f[i - 1][j] + f[i][j - 1];
				}
			}
		}
		System.out.println(f[n][m]);
	}
}

```

