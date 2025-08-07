[TOC]

# 一、基础算法

## 1.1 火车头

```C++
#include<bits/stdc++.h>
#define int long long
#define ll long long
#define QwQ return 0;
#define awa return
#define fios ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
#define EPS 1e-8
#define INF 0x7fffffff
#define DEBUG "L= "<< setw(2)<< __LINE__ << ":  "
#ifdef DEBUG
#define print(a) cout<<DEBUG<<#a << ": " <<(a)<<endl
#define prin2(a, b) cout <<DEBUG<<":  "<< #a<<": "<< (a)<<" "<<#b<< ": "<<(b)<<endl
#define prinl(a) do{cout<<DEBUG<<endl;int s=min((int)a.size(),(int)16);for(int i=0;i<s;i++)cout<<setw(4)<<i<<" \n"[i==s-1];int num=0;for(auto i:a){cout<<setw(4)<<i<<"\n "[bool(++num%s)];}cout<<endl;}while(0)
#endif
using namespace std;
const int mod=998244353;
const int maxn=2e5+1e3;

void solve()
{

}

signed main()
{
	fios
	int _=1;
	cin>>_;
	while(_--) solve();
	QwQ
}
```

## 1.2 二分

### 1.2.1 整数域



```c++

```



### 1.2.2 实数域



```c++

```



## 1.3 三分

### 1.3.1 整数域



```c++

```



### 1.3.2 实数域



```c++

```





# 二、图论

## 2.1 常见概念



```c++

```



## 2.2 单源最短路径

### 2.2.1 （正权稀疏图）动态数组+Dijkstra



```c++

```



### 2.2.2 （负权图）SPFA



```c++

```



## 2.3 多源最短路径

### 2.3.1 （稠密图）邻接矩阵+Floyd



```c++

```



## 2.4 平面图最短路（对偶图）



```c++

```



## 2.5 最小生成树

### 2.5.1 （稀疏图）Prim



```c++

```



### 2.5.2 （稠密图）Kruskal



```c++

```



## 2.6 缩点（Tarjan）

### 2.6.1 （有向图）强连通分量缩点



```c++

```



### 2.6.2 （无向图）割边缩点-边双连通



```c++

```



### 2.6.3 （无向图）割点缩点-点双连通



```c++

```



## 2.7 染色法判定二分图（DFS）



```c++

```



## 2.8 链式前向星建图与搜索



```c++

```



## 2.9 性质与例题



```c++

```



# 三、树论与数据结构

## 3.1 封装（直径+重心+中心）



```c++

```



## 3.2 树的直径



```c++

```



## 3.3 点分治 / 树的重心



```c++

```



## 3.4 最近公共祖先 LCA

### 3.4.1 树链剖分解法



```c++

```



### 3.4.2 树上倍增解法



```c++

```



## 3.5 树上启发式合并（DSU on tree）



```c++

```



## 3.6 并查集

每次操作时间复杂度 $\Theta(\alpha(n))$ 。（可理解为是一个很小的常数）

并查集在经过修改后可以支持单个元素的删除、移动；使用动态开点线段树还可以实现可持久化并查集。

**初始化**

```c++
void init()
{
    for(int i=1;i<=n;i++)  //n个点
		f[i]=i;
}
```

 **查询**

```C++
int find(int x)    //路径压缩
{
	if(f[x]==x)               //如果我爸爸是我自己
		return x;             //返回自己
	return f[x]=find(f[x]);   //否则继续找爸爸的爸爸
}
```

**合并**

```c++
void union(int x,int y)
{
    int fx=find(x),fy=find(y);  //先找祖宗
    if(fx!=fy)					//如果俩祖宗不一样
    	f[fx]=fy;				//就让祖宗杂交到一起
}
```

**启发式合并**

合并时，选择哪棵树的根节点作为新树的根节点会影响未来操作的复杂度。我们可以将节点较少或深度较小的树连到另一棵，以免发生退化，更加优化时间复杂度，常见于可持久化并查集和线段树分治中。

```c++
void union(int x,int y)
{
    int fx=find(x),fy=find(y);  
    if(fx==fy)				
    	return;
    if(size[x]<size[y])
        swap(x,y);
    f[fy]=fx;           //小连大，fx成为fy的祖宗
    size[x]+=size[y];   //维护size数组记录不同根节点的子树大小
}
```

**删除**

对于删除操作，在完美的并查集中（所有节点都直接连接在根节点上）理论上只要把要删除的节点的上级重新指向自己就可以了。但是实际情况中，我们的并查集形成的树的形态都是不可预估形态的，如果直接将一个节点指向自己可能会将他的“下级”和他一块删除，这就和我们的想法违背了。所以在一个需要删除的并查集中初始化时就要处理一下：

- 首先每个节点不再指向自己而是另一个比不会出现的节点，类似于将每个节点放到一个盒子中
- 这样，删除时只需要把这个节点从当前盒子拿出来，放到另一个盒子中
- 由于节点之间都是通过盒子来确定关系的，所以盒子中元素是否存在并不影响节点之间的关系

```C++
//提前开好两倍空间
void init()
{
    for(int i=1;i<=n;i++)     //每个元素i的父节点是盒子节点n+i
        f[i]=n+i;		 //此时普通节点为1~n,盒子节点为n+1~2*n,合并时通过盒子节点合并
    for(int i=n+1;i<=2*n;i++)   //盒子节点指向自己就像一般的并查集一样
        f[i]=i;
}
void delete(int x)
{
    int fx=find(x);
    //size[x]--;      //维护子树大小
    f[n+x]=x;
    f[x]=x;
}
```

**移动**

使用 $find$ 函数分别找到 $x$ 和 $y$ 的根节点，分别存储在 $fx$ 和 $fy$ 中，如果 $x$ 和 $y$ 已经在同一个集合中（即 $fx == fy$），函数直接返回，因为不需要移动。

* 更新父节点：

将 $x$ 的父节点直接设置为 $fy$，这样 $x$ 现在属于 $y$ 的集合。这是移动操作的核心，直接修改父节点可以快速改变元素的集合归属。

* 更新集合大小：

从 $x$ 原来的集合的大小中减去 $1$，因为 $x$ 被移出了这个集合。向 $y$ 的集合的大小中加上 $1$ ，因为 $x$ 被加入到了这个集合。

```c++
void move(int x,int y)    //将x移入y
{
    int fx=find(x),fy=find(y);
    if (fx==fy) 
        return;  // 如果x和y已经在同一个集合中，则无需移动
    f[x]=fy;  // 将x的父节点直接设置为y的根节点
    //--size[fx];  // 从x原来的集合中减去1
    //++size[fy];  // 将x加入到y的集合中
}
```

**并查集的应用**

**带权并查集**

在对并查集进行路径压缩和合并操作时，这些权值具有一定属性，即可将他们与父节点的关系，变化为与所在树的根结点关系。

也就是说，权值代表着**当前节点与父节点的某种关系（即使路径压缩了也是这样）**，通过两者关系，也可以将同一棵树下两个节点的关系表示出来。

由于合并时一起传递了权值，思想类似于前缀和，常见时间复杂度 $\Theta(n\alpha(n))$ 。

```c++
int find(int x)
{
    if(f[x]==x)      
		return x;
	else
	{
		f[x]=find(f[x]);
		value[x]+=value[f[x]];  //更新权值
	}
	return f[x];
}
```

```c++
void union(int x,iny y)
{
    int fx=find(x),fy=find(y);  
    if(fx!=fy)
    {
        f[fx]=fy;
        value[fx] = -value[x] + value[y] + s;  //显然x到py两条路径的权值之和应该相同，即可推出该式
    }
}
```

![noi](C:\Users\Fengmi123\Pictures\Camera Roll\372e16896717bb2e74463f5ed5a17241.jpeg)

**种类并查集**

并查集可以很方便地维护联通关系，但对于想要表达**对立关系**的情况来说，正常的并查集就很难满足我们的需求。

考虑开 $k$ 倍的并查集，利用在不同种类的并查集中合并的意义来维护**对立关系**

```C++
/*
	** 每次union都要进行n次 **
*/
void friends(int x,int y)   //在同类并查集间建边
{
	for(int i=0;i<k;i++)	// if(find(x)==find(y) -> friend  // if(find(x)==find(i*y) -> conflict
    {
        union(x+i*n,y+i*n);
    }
}

void conflict(int x,int y)	//在不同类并查集间建边
{
    for(int i=0;i<k;i++)
    {
        for(int j=0;j<k;j++)
        {
            if(i==j)
                continue;
            union(x+i*n,y+j*n);
        }
    }
}
```



## 3.7 树状数组

### 3.7.1 封装



```c++

```



### 3.7.2 求解逆序对



```c++

```



### 3.7.3 二维树状数组



```c++

```



## 3.8 线段树

### 3.8.1 区间加、改、最值、合并

**全局维护内容与主程序**

```c++
#define ls root<<1			//左儿子==rt*2
#define rs root<<1|1		//右儿子==rt*2+1
const int maxn=1100000;		
struct node{
	int b,e,s,lazy,lazyc;	//区间begin、end、总和sum、加法意义lazy、乘法意义lazy 等
}T[4*maxn];					//不要忘开4倍空间
int n,q,p,ans=0; 			//在本题中，经典数列共n项，q次询问，模数为p

/*0*/signed main()
{
	cin>>n>>q>>p;
	creat(1,1,n);			//建线段树 /*1.0*/
	for(int i=1;i<=q;i++)
	{
		int op,x,y,k;
		cin>>op;
		if(op==1)
		{
		      cin>>x>>y>>k;
		      update_cheng(1,x,y,k);//更新区间累加积 
        }
		if(op==2)
		{
			cin>>x>>y>>k;
			update_jia(1,x,y,k);	//更新区间累加和 
		}
		if(op==3)
		{
			cin>>x>>y;
			ans=0; 
			sum(1,x,y);				//区间求和 
			cout<<ans%p<<endl;
		}
	}
	return 0;
} 
```

**建线段树—— $creat()$**

```C++
/*1.0*/void creat(int root,int l,int r)//建线段树
{
	//初始化T[root] 
	T[root].b=l;
	T[root].e=r;
	T[root].lazy=0;
	T[root].lazyc=1;
	//点树 
	if(l==r)
	{
		cin>>T[root].s;
		return;
	}
	//递归二分子树 
	int mid=l+r>>1;
	creat(ls,l,mid);//左孩子 
	creat(rs,mid+1,r);//右孩子 
	T[root].s=(T[ls].s+T[rs].s)%p;//初始化累加和 
}
```

**更新区间累加和—— $updatejia()$**

```c++
/*1.2*/void update_jia(int root,int l,int r,int k)//更新区间累加和(递归)
{
	//1.相离 →没关系return 
	if(T[root].e<l || r<T[root].b)
		return;
	//2.所求区间覆盖节点区间 →更新lazy 
	if(l<=T[root].b && T[root].e<=r)
	{
		T[root].s=(T[root].s+k*(T[root].e-T[root].b+1))%p; 
            T[root].lazy=(T[root].lazy+k)%p;
		return;
	}
	//3.相交 →传给左右孩子看是否覆盖 
	pushdown(root,T[root].b,T[root].e);//从根节点向下压lazy 
	update_jia(ls,l,r,k);//左孩子 
	update_jia(rs,l,r,k);//右孩子 
	pushup(root);//从根节点收集孩子的lazy
}
```

**区间求和—— $sum()$**

```c++
/*1.3*/void sum(int root,int l,int r)//区间求和(递归)
{
	//1.相离 →没关系return 
	if(T[root].e<l || r<T[root].b)
		return;
	//2.所求区间覆盖节点区间 →累加和
	if(l<=T[root].b && T[root].e<=r)
	{
		ans+=T[root].s;//累加ans值 
		return;
	}
	//PS:此时仍在递归，向下传送时带上lazy 
	pushdown(root,T[root].b,T[root].e);//从根节点向下压lazy 
	//3.相交 →传给左右孩子看是否覆盖（开始回溯） 
	sum(ls,l,r);
	sum(rs,l,r);
	//PS:三情况处理完毕，向上回溯时处理更新sum值（lazy在到达叶子时变为sum） 
	pushup(root);
} 
```

### 3.8.2 区间乘、取模

**更新区间累加积—— $updatecheng()$**

```c++
/*1.1*/void update_cheng(int root,int l,int r,int k)//更新区间累加积(递归)
{
	//1.相离 →没关系return 
	if(T[root].e<l || r<T[root].b)
		return;
	//2.所求区间覆盖节点区间 →更新lazy 
	if(l<=T[root].b && T[root].e<=r)
	{
	      //一定要先乘后加，把爸爸的加法lazytag同时乘上k！！！ 
            T[root].s=(T[root].s*k)%p;
            T[root].lazyc=(T[root].lazyc*k)%p;
            T[root].lazy=(T[root].lazy*k)%p;
		return;
	}
	//3.相交 →传给左右孩子看是否覆盖 
	pushdown(root,T[root].b,T[root].e);//从根节点向下压lazy 
	update_cheng(ls,l,r,k);//左孩子 
	update_cheng(rs,l,r,k);//右孩子 
	pushup(root);//从根节点收集孩子的lazy
}
```

**上传—— $pushup()$**

```c++
/*2.1*/void pushup(int root)//从根节点收集孩子的lazy
{
	T[root].s=(T[ls].s+T[rs].s)%p;
}
```

**下传—— $pushdown()$**

```c++
/*2.2*/void pushdown(int root, int l, int r)
{
    int m=l+r>>1;
    //根据我们规定的优先度，先乘后加
    //儿子的值=此刻儿子的值*爸爸的乘法lazytag+儿子的区间长度*爸爸的加法lazytag
    T[ls].s = (T[ls].s*T[root].lazyc + T[root].lazy*(m-l+1))%p;
    T[rs].s = (T[rs].s*T[root].lazyc + T[root].lazy*(r-m))%p;
    //维护lazytag
    T[ls].lazyc = (T[ls].lazyc*T[root].lazyc)%p;
    T[rs].lazyc = (T[rs].lazyc*T[root].lazyc)%p;
    T[ls].lazy = (T[ls].lazy*T[root].lazyc + T[root].lazy)%p;
    T[rs].lazy = (T[rs].lazy*T[root].lazyc + T[root].lazy)%p;
    //把父节点的值初始化
    T[root].lazyc=1;
    T[root].lazy=0;
    return;
}
```

## 3.9 坐标压缩和离散化

 

```c++

```



## 3.10 分数运算



```c++

```



## 3.11 ST表



```c++

```



## 3.12 性质与例题



```c++

```



# 四、动态规划

## 4.1 背包

### 4.1.1 01背包



```c++

```



### 4.1.2 完全背包



```c++

```



### 4.1.3 多重背包



```c++

```



### 4.1.4 混合背包



```c++

```



### 4.1.5 二维费用背包



```c++

```



### 4.1.6 分组背包



```c++

```



### 4.1.7 有依赖背包



```c++

```



### 4.1.8 求方案数



```c++

```



### 4.1.9 求具体方案



```c++

```



## 4.2 线性DP



```c++

```



## 4.3 区间DP



```c++

```



## 4.4 数位DP



```c++

```



## 4.5 树形DP



```c++

```



## 4.6 状压DP



```c++

```



# 五、字符串

## 5.1 字符串哈希



```c++

```



## 5.2 KMP



```c++

```



## 5.3 Trie树



```c++

```



# 六、数论

## 6.1 常见结论

### 6.1.1 调和级数



```c++

```



### 6.1.2 素数密度与分布



```c++

```



### 6.1.3 因数数量与极大个数



```c++

```



### 6.1.4  gcd 与 lcm



```c++

```



## 6.2 线性筛与质因数分解

```c++
/*
	bool v[maxn];//标记是否为合数
	int pri[maxn];//存放筛出来的素数
*/
void Eulerprime(int n)
{
	memset(v,true,sizeof(v));
	v[1]=false;
	int cnt=0;
    for(int i=2;i<=n;i++) 
    {
        if(v[i]==true) 
        	pri[++cnt]=i;
	    for(int j=1;j<=cnt;j++) 
	    {
	        if(i*pri[j]>n) 
	            break;
	        v[i*pri[j]]=false;
	        if(i%pri[j]==0) 
	            break;
	    }
    }
}
/*
	运行完后pri数组里面便是n以内的所有素数
*/
void factor(int x)
{
    for(int i=1;pri[i]*pri[i]<=x;i++)
    {
        if(x%pri[i]==0)
        {
            int s=0;
            while(x%pri[i]==0)
            {
                x/=pri[i];
                s++;
            }
            while(s--)
            {
                q.push(pri[i]);
            }
        }
    }
    if(x>1)
    {
        q.push(x);
    }
}
/*
	运行完后q队列里面便是x的所有质因数
*/
```

## 6.3 快速幂与分数取模

```c++
int quickpower(int a,int b,int p)    //a^b (mod p) = ans
{
	int ans=1;
	while(b>0)
    {
		if(b%2==1)
        {
            ans*=a;ans%=p;
        }
		a*=a;a%=p;
		b/=2;
	}
	return ans%p;
} 

//answer=quickpower(a,b,p);

int inv(int a,int b)
{
	return a * quickpower(b, mod - 2, mod) % mod;
}
```

## 6.4 高精度

转化为数组，优点是可以统一处理进位。

**加法**

```C++
//int a[maxn],b[maxn],ans[maxn];
string add(string A,string B)
{
	string Ans;
	int dig1=A.length(),dig2=B.length();
	int dig3=max(dig1,dig2);
	for(int i=1;i<=dig1;i++)   a[dig1-i+1]=A[i-1]-'0'; //倒序存储
	for(int i=1;i<=dig2;i++)   b[dig2-i+1]=B[i-1]-'0';
    // ******************************************************************** //
	for(int i=1;i<=dig3;i++) ans[i]=a[i]+b[i]; //加和
	for(int i=1;i<=dig3;i++) ans[i+1]+=ans[i]/10,ans[i]%=10; //进位
	dig3+=(ans[dig3+1]) ? 1 : 0 ;
    // ******************************************************************** //
	for(int i=dig3;i>=1;i--) Ans+=to_string(ans[i]);
	return Ans;
}
```

**减法**

```C++
//int a[maxn],b[maxn],ans[maxn];
bool cmp(string A,string B)//A>B(T)
{
	int dig1=A.length(),dig2=B.length(); 
	if(dig1>dig2)
		return true;
	else if(dig1<dig2)
		return false;
	else
		return A>=B;
}

string sub(string A,string B)
{
	int f=0;
	if(!cmp(A,B)) swap(A,B),f=1;						//交换大小并记录符号
	int dig1=A.length(),dig2=B.length(); 
	int dig3=max(dig1,dig2);
	for(int i=1;i<=dig1;i++)   a[dig1-i+1]=A[i-1]-'0'; 	//倒序存储
	for(int i=1;i<=dig2;i++)   b[dig2-i+1]=B[i-1]-'0';
    // ******************************************************************** //
	for(int i=1;i<=dig3;i++) ans[i]=a[i]-b[i]; 			//加和
	for(int i=1;i<=dig3;i++)
	{
		ans[i+1]-=signbit(ans[i]);	//借位
		if(ans[i]<0) 
			ans[i]+=10; 
	} 
	while(!ans[dig3]) dig3--;     	//删除前导0
    // ******************************************************************** //
	for(int i=dig3;i>=1;i--) Ans+=to_string(ans[i]);
	if(f) Ans='-'+Ans;				//加符号
	if(Ans.empty()) Ans+='0';		//若相等为0
	return Ans;
}
```

## 6.5 防爆模乘



```c++

```



## 6.6 裴蜀定理



```c++

```



## 6.7 逆元



```c++

```



## 6.8 欧拉函数



```c++

```



## 6.9 组合与质数

### 6.9.1 debug



```c++

```



### 6.9.2 质因数分解



```c++

```



### 6.9.3 试除法判断质数



```c++

```



### 6.9.4 杨辉三角



```c++

```



## 6.10 性质与例题



```c++

```





# 七、几何、概率与博弈论

## 7.1 平面几何初始化



```c++

```



# 八、常见例题

## 8.1 逆序对（归并排序）



```c++

```



# 九、STL与库函数

## 9.1 vector

```c++
// 初始化
vector<int> vec(n,0);
vector<int> vec[2000];
vector< vector<int> > vec2(n,vector<int>(m,0));

// 操作
vec.clear();
vec.push_back(x);
vec.pop_back(x);

sort(v.begin(),v.end(),cmp);
reverse(v.begin(),v.end());

// lower_bound返回小于等于 upper_bound返回大于 找不到返回end();
int t=lower_bound(v,begin(),v.end(),val)-v.begin();
upper_bound();

// 数组去重
n=unique(v.begin(),v.end())-v.end();// 返回去重后的重复部分头指针
v.erase(unique(v.begin(),v.end()),v.end());

// 迭代器
// 遍历

// 求前缀和
partial_sum(s.begin(),s.end(),back_inserter(d));
v.front();
v.back();
```

## 9.2 string

```c++
string s;

// 操作
s+=s;
s-=s;

s.clear();
s.size();

// 查找第一次出现的位置 找不到返回string::npos
s.find(str,pos);
// 求从idx下标 长度为len的子串
string a=s.substr(idx,len)

// 插入/删除
s.insert(idx,cnt,ch);
s.insert(idx,str);
s.erase(idx,cnt);
s.replace(pos,count,str);
s.replace(first,last,str);
```

## 9.3 stack queue

```c++
// stack top pop push size
// queue front pop push size
// deque push_back push_front pop_back pop_front
// 小根堆
priority_queue<int,vector<int>,greater<int>> q;
// 大根堆 默认
priority_queue<int,vector<int>,less<int>> q;
```

## 9.4 set map

```c++
// 红黑树实现 自动排序去重

se.insert(val);
se.erase(val);
se.begin();
se.end();
se.empty();
se.size();

mp.insert({idx,val});
mp[idx]=val;
mp.erase(idx);
mp.find(idx); // 返回指针
mp.count(idx);
mp.lower_bound();
mp.size();

// auto遍历/迭代器遍历
```

## 9.5 pair

```c++
pair<int,int> p;
p.first;
p.second;

// 离散化
// map<pair<int,int>,int>
```

## 9.6 Hash

```c++
// 离散化 取模
// 无排序有去重 O(1)查询是否存在 退化为O(n)
// 卡哈希
unordered_set<int> use;
unordered_map<int,int> ump;
use.insert();
use.cound(val);
```

## 9.7 math

```C++
//绝对值、符号(1负0正)
abs(x); fabs(x); signbit(x)
//四舍五入、下取整、上取整、截断向0取整
round(x); floor(x); ceil(x); trunc(x);
//平方根、立方根、根号下x^2+y^2
sqrt(x); cbrt(x); hypot(x,y);
//三对指
sin(rad); asin(x); sinh(x); asinh(x); //cos tan
ln(x)=log(x); log10(x); log2(x); 
e^x=exp(x); 2^x=exp2(x); pow(a,b);
```

## 9.8 #define

```C++
//定义宏 eg.
#define UPCASE(c) ( ((c)>='a' && (c)<='z') ? ((c)-0x20) : (c) )    	//小写转大写
#define fios ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);   	//快读
#define EPS 1e-8
#define INF 0x7fffffff
//debug
#define DEBUG "L= "<< setw(2)<< __LINE__ << ":  "
#ifdef DEBUG
#define print(a) cout<<DEBUG<<#a << ": " <<(a)<<endl
#define prin2(a, b) cout <<DEBUG<<":  "<< #a<<": "<< (a)<<" "<<#b<< ": "<<(b)<<endl
#define prinl(a) do{cout<<DEBUG<<endl;int s=min((int)a.size(),16);for(int i=0;i<s;i++)cout<<setw(4)<<i<<" \n"[i==s-1];int num=0;for(auto i:a){cout<<setw(4)<<i<<"\n "[bool(++num%s)];}cout<<endl;}while(0)
#define prinSq(a)
```

## 9.9 全排列 $next\_permutation$

> 将当前排列更改为 全排列中的下一个排列。如果当前排列已经是 全排列中的最后一个排列（元素完全从大到小排列），函数返回 false 并将排列更改为 全排列中的第一个排列（元素完全从小到大排列）；否则，函数返回 true

```c++
// 输出字典序全排列 处理后数组复原
do{
    for(int i=0;i<n;i++){
        cout<<a[i];
    }
}while(next_permutation(a,a+n));
prev_permutation(a,a+n)
   
```

## 9.10 二分查找

### 9.10.1 $lower\_bound$ & $upper\_bound$

``lower`` 表示 $≥$  ``upper`` 表示 $>$  **需排序**

```c++
//返回a数组[start,end)区间中第一个>=x的地址
cout<<lower_bound(a+start,a+end,x);
//返回a数组[start,end)区间中第一个>=x的元素下标
cout<<lower_bound(a+1,a+n+1,x)-a;
//返回k在a数组[start,end)区间中出现了几次
upper_bound(a+1,a+n+1,k)-lower_bound(a+1,a+n+1,x);
```

### 9.10.2 $shuffle$

```c++
mt19937 rnd(chrono::steady_clock::now().time_since_epoch().count());
shuffle(a+1,a+n+1,rnd);
```

### 9.10.3 $binary\_search$

用于查找某一元素是否在容器中，相当于 find 函数，**需排序**

```c++
//查找a数组[start,end)区间x是否存在，返回bool值
cout<<binary_search(a+start,a+end,x);
```

## 9.11 批量递增赋值 $iota$

对容器进行递增初始化

```c++
//将a数组[start,end)区间赋值为"x, x+1 ,x+2, ..."
iota(a+1,a+n+1,x);
```

## 9.12 数组去重 $unique$

**需排序**，不断将不重复的元素移至前面，所以**数组容器大小不变**，返回**去重后不重复序列的最后一个元素的下一个元素**。

```c++
//将a数组去重并返回迭代器
int *it=unique(a+1,a+n+1);
for(int i=1;i<*it;i++)  {}
//与erase结合实现去重+删除 (vector)
a.erase(unique(a.begin(),a.end()),a.end());
```

## 9.13 位运算 $\_\_builtin\_$



```c++

```



## 9.14 数字转字符串转数字

### 9.14.1 $to\_string$



```c++

```



### 9.14.2 $stoi$



```c++

```



### 9.14.3 $sto\_$



```c++

```



## 9.15 判断非递减 $is\_sorted$



```c++

```



## 9.16 累加 $accumulate$



```c++

```



## 9.17 元组 $tuple$



```c++

```



## 9.18 位运算 $bitset$



```c++

```



# 十、其他杂类

## 10.1 快读

日常用的 $cin$ 由于需要与 $scanf$ 进行同步流，所以会很慢。

遇到数据较多的题目时，用 $scanf$ 或者 $cin$ 肯定是不够快的。由于 $getchar()$ 有速度快的特性，有了这样的快速读入函数。

```C++
void read(int &x)//快读 
{ 
	int f=1,x=0;
    char s=getchar();
	while(s<'0' || s>'9')	{if(s=='-')f=-1; s=getchar();}
	while(s>='0'&& s<='9')  {x=x*10+s-'0';   s=getchar();}
	x*=f;
}
```

~~虽然懒得写了用这一行也能做到  $ cin == scanf $  的速度~~

```C++
#define fios ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
```

## 10.2 对拍

用来查验 **不一定正确的优化程序** 正确性的一种方法，共由四个文件组成：

### $solve$

 **不一定正确的优化程序**

### $std$

**一定正确的暴力程序**

### $data$

**自己造随机输入数据的程序**

```c++
//活用以下函数
srand(time(0));
x=rand()%p;
//
mt19937 rnd(chrono::steady_clock::now().time_since_epoch().count());

```

### $compare$

**对拍模板本体**

```c++
#include<bits/stdc++.h>
using namespace std;
int main()
{
	int t=1;
	while(1)
	{
		cout<<"test: "<<t++<<endl;
		system("data.exe > data.in");
		system("std.exe < data.in > std.out");
		system("solve.exe < data.in > solve.out");
		if(system("fc std.out solve.out > diff.log"))
			cout<<"WA"<<endl,break;
		else
			cout<<"AC"<<endl;
	}
	return 0;
}
```

达到的效果就是，直接运行 $compare$ 后，程序会一直运行显示AC，如果发生不同，程序自动停止，可以从 $data.in$ 看到当前输入数据，从 $diff.log$ 看到当前两个不同的输出数据

## 10.3 Python常用语法



```c++

```



## 10.4 速度测试



```c++

```

