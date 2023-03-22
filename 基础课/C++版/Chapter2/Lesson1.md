### Lesson1（数组模拟数据结构）

> **链表与邻接表（数组模拟）**
>
> 单链表&邻接表（n个链表）：邻接表主要用于存储图与树
>
> * e[N]:值	ne[N]:next
> * e与ne数组通过下标关联，空结点下标用-1表示，链表最后一个有效结点ne为-1
>
> ```C++
> #include <iostream>
> 
> using namespace std;
> 
> const int N = 100010;
> 
> // head表示头结点的下标
> // e[i]表示结点i的值
> // ne[i]表示结点i的后一个结点下标
> // idx表示当前已经使用到了哪个点
> int head, e[N], ne[N], idx;
> 
> //初始化
> void init() {
>     head = -1, idx = 0;
> }
> 
> //将x插到头结点
> void add_to_head(int x) {
>     e[idx] = x, ne[idx] = head, head = idx++;
> }
> 
> //将x插到第k个结点之后
> void add(int k, int x) {
>     e[idx] = x, ne[idx] = ne[k], ne[k] = idx++;
> }
> 
> //将下标是k的点的后面的点删掉
> void remove(int k) {
>     ne[k] = ne[ne[k]];
> }
> 
> int main(void) {
>     int m;
>     scanf("%d", &m);
>     
>     init();
>     
>     while (m--) {
>         int k, x;
>         char op;
>         // scanf("%c", &op);
>         cin >> op;
>         
>         if (op == 'H') {
>             scanf("%d", &x);
>             add_to_head(x);
>         }
>         else if (op == 'D') {
>             scanf("%d", &k);
>             if (!k) head = ne[head];
>             else remove(k-1);
>         }
>         else {
>             scanf("%d%d", &k, &x);
>             add(k-1, x);
>         }
>     }
>     
>     for (int i=head; i!=-1; i=ne[i]) printf("%d ", e[i]);
>     
>     return 0;
> }
> ```
>
> 
>
> 双链表：优化某些特定问题
>
> * 在单链表基础上增加指向前一个结点的指针
> * l[N], r[N]
> * 不定义头结点与尾结点，0：head     1：tail
>
> ```C++
> #include <iostream>
> 
> using namespace std;
> 
> const int N = 100010;
> 
> int l[N], r[N], e[N], idx;
> 
> //初始化双链表
> void init() {
>     //0表示最左端结点，1表示最右边结点，起标志作用，不能被删除或修改
>     //idx从2开始
>     r[0] = 1, l[1] = 0;
>     idx = 2;
> }
> 
> //在第k个插入的结点右侧插入一个结点
> void add(int k, int x) {
>     e[idx] = x;
>     l[idx] = k, r[idx] = r[k];
>     l[r[k]] = idx, r[k] = idx++;
> }
> 
> //删除第k个插入的结点
> void remove(int k) {
>     r[l[k]] = r[k];
>     l[r[k]] = l[k];
> }
> 
> int main(void) {
>     int m;
>     scanf("%d", &m);
>     
>     init();
>     
>     while (m--) {
>         int k, x;
>         string op;
>         cin >> op;
>         
>         if (op == "L") {
>             scanf("%d", &x);
>             add(0, x);
>         }
>         else if (op == "R") {
>             scanf("%d", &x);
>             add(l[1], x); 
>         }
>         else if (op == "D") {
>             scanf("%d", &k);
>             remove(k+1);    //注意从第k个插入的数下标为k+1
>         }
>         else if (op == "IL") {
>             scanf("%d%d", &k, &x);
>             add(l[k+1], x);
>         }
>         else {
>             scanf("%d%d", &k, &x);
>             add(k+1, x);
>         }
>     }
>     
>     for (int i=r[0]; i!=1; i=r[i]) printf("%d ", e[i]);
>     
>     return 0;
> }
> ```
>
> 
>
> 邻接表（多个单链表）
>
> * head[1]->…
> * head[2]->…
> * …
> * head[3]->…



> **栈**（先进后出）
>
> ```C++
> //stk表示栈，tt表示栈顶
> int stk[N], tt;
> 
> //插入x
> stk[++tt] = x;
> 
> //弹出栈顶
> tt--;
> 
> //判断是否为空
> if (tt > 0) not empty
> else empty
> 
> //栈顶
> stk[tt];
> ```
>
> * e1:
>
> ```C++
> #include <iostream>
> 
> using namespace std;
> 
> const int N = 100010;
> 
> int stk[N], tt;
> 
> int main(void) {
>     int m;
>     scanf("%d", &m);
> 
>     while (m--) {
>         int x;
>         string op;
>         cin >> op;
> 
>         if (op == "push") {
>             scanf("%d", &x);
>             stk[++tt] = x;
>         }
>         else if (op == "pop") tt--;
>         else if (op == "empty") {
>             if (tt > 0) puts("NO");
>             else puts("YES");
>         }
>         else printf("%d\n", stk[tt]);
>     }
> 
>     return 0;
> }
> ```
>
> * e2: 表达式求值
>
> ```C++
> #include <iostream>
> #include <cstring>
> #include <unordered_map>
> 
> using namespace std;
> 
> const int N = 100010;
> 
> char str[N];
> int num[N];     //数字栈
> char op[N];     //表达式栈
> int tt1, tt2;
> 
> void eval() {
>     int b = num[tt1--];
>     int a = num[tt1--];
>     char c = op[tt2--];
>     
>     int x = 0;
>     if (c == '+') x = a+b;
>     else if (c == '-') x = a-b;
>     else if (c == '*') x = a*b;
>     else if (c == '/') x= a/b;
>     
>     num[++tt1] = x;
> }
> 
> int main(void) {
>     scanf("%s", str);
>     
>     unordered_map<char, int> pr{{'+', 1}, {'-', 1}, {'*', 2}, {'/', 2}};
>     
>     for (int i=0; str[i]; i++) {
>         auto c = str[i];
>         
>         if (isdigit(c)) {
>             int x = 0, j = i;
>             while (str[j] && isdigit(str[j])) {
>                 x = x*10+str[j++]-'0';
>             }
>             
>             num[++tt1] = x;
>             i = j-1;
>         }
>         else if (c == '(') op[++tt2] = c;
>         else if (c == ')') {
>             while (op[tt2] != '(') eval();
>             tt2--;
>         }
>         else {
>             while (tt2 && op[tt2]!='(' && pr[op[tt2]]>=pr[c]) eval();
>             op[++tt2] = c;
>         }
>     }
>     
>     while (tt2) eval();
>     printf("%d", num[tt1]);
>     
>     return 0;
> }
> ```
>
> 



> **队列**（先进先出）
>
> ```C++
> //在队尾插入元素，在队头弹出元素
> int q[N], hh, tt = -1;
> 
> //插入
> q[++tt] = x;
> 
> //弹出
> hh++;
> 
> //判断是否为空
> if (hh <= tt) not empty
> else emtpy
> 
> //取队头
> q[hh]
> ```
>
> * e1
>
> ```C++
> #include <iostream>
> 
> using namespace std;
> 
> const int N = 100010;
> 
> int q[N], hh, tt = -1;
> 
> int main(void) {
>     int m;
>     cin >> m;
>     
>     while (m--) {
>         int x;
>         string op;
>         cin >> op;
>         
>         if (op == "push") {
>             scanf("%d", &x);
>             q[++tt] = x;
>         }
>         else if (op == "pop") hh++;
>         else if (op == "empty") {
>             if (hh <= tt) puts("NO");
>             else puts("YES");
>         }
>         else cout << q[hh] << endl;
>     }
>     
>     return 0;
> }
> ```



> **单调栈**
>
> 常见模型：在一个序列中，找出每一个数左边离它最近的且满足某种性质（如 最大最小）的数的位置
>
> 先想暴力算法，再利用单调性进行优化
>
> ```C++
> #include <iostream>
> 
> using namespace std;
> 
> const int N = 100010;
> 
> int stk[N], tt;
> 
> int main(void) {
>     int m;
>     scanf("%d", &m);
>     
>     while (m--) {
>         int x;
>         scanf("%d", &x);
>         
>         while (tt && stk[tt] >= x) tt--;
>         if (tt) printf("%d ", stk[tt]);
>         else printf("-1 ");
>         
>         stk[++tt] = x;
>     }
>     
>     return 0;
> }
> ```



> **单调队列**
>
> 经典应用：求滑动窗口中最大值or最小值
>
> ```C++
> #include <iostream>
> 
> using namespace std;
> 
> const int N = 1000010;
> 
> int n, k;
> int a[N];
> //q中存储的是下标
> int q[N], hh, tt = -1;
> 
> int main(void) {
>     scanf("%d%d", &n, &k);
>     
>     for (int i=0; i<n; i++) scanf("%d", &a[i]);
>     
>     //打印所有最小值
>     for (int i=0; i<n; i++) {
>         //窗口容量已满，出队
>         if (hh<=tt && i-k+1>q[hh]) hh++;
>         //单调性优化
>         while (hh<=tt && a[q[tt]]>=a[i]) tt--;
>         //入队
>         q[++tt] = i;
>         
>         if (i-k+1 >= 0) printf("%d ", a[q[hh]]);
>     }
>     
>     puts("");
>     
>     //最大值
>     hh = 0, tt = -1;
>     for (int i=0; i<n; i++) {
>         if (hh<=tt && i-k+1>q[hh]) hh++;
>         while (hh<=tt && a[q[tt]]<=a[i]) tt--;
>         q[++tt] = i;
>         
>         if (i-k+1 >= 0) printf("%d ", a[q[hh]]);
>     }
>     
>     return 0;
> }
> ```



> **KMP**
>
> 习惯下标从1开始
>
> 1. 暴力算法如何做
>
> ```C++
> S[N], p[M];
> 
> //朴素做法
> for (int i=1; i<=n; i++) {
> 	bool flag = true;
> 	for (int j=1; j<=m; j++) {
> 		if (s[i+j-1] != p[j]) {
> 			flag = false;
> 			break;
> 		}
> 	}
> 
>     if (flag) 匹配成功
> } 
> ```
>
> 
>
> 2. 如何去优化
>
> * **next[i] 表示以i为终点，且后缀和前缀相等的子串的最大长度。**
>
>   next[i] = j		p[1, j] = p[i-j+1, i]
>
>
> ```C++
> #include <iostream>
> 
> using namespace std;
> 
> const int N = 100010, M = 1000010;
> 
> int n, m;
> char p[N], s[M];
> int ne[N];
> 
> int main(void) {
>     scanf("%d%s%d%s", &n, p+1, &m, s+1);
> 
>     for (int i=2, j=0; i<=n; i++) {
>         while (j && p[i]!=p[j+1]) j = ne[j];
>         if (p[i] == p[j+1]) j++;
>         ne[i] = j;
>     }
> 
>     for (int i=1, j=0; i<=m; i++) {
>         while (j && s[i]!=p[j+1]) j = ne[j];
>         if (s[i] == p[j+1]) j++;
>         if (j == n) {
>             printf("%d ", i-n);
>             j = ne[j];
>         }
>     }
> 
>     return 0;
> }
> 
> ```
>
> 作用
>
> * 可用于求循环节（字符串哈希无法使用）



