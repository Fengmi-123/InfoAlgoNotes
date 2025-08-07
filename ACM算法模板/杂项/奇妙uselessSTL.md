## vector
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

## string
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

## next_permutation
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

## stack queue deque

```c++
// stack top pop push size
// queue front pop push size
// deque push_back push_front pop_back pop_front
```

## priority_queue

> 插入删除数 维护最大值/最小值
```c++
// 小根堆
priority_queue<int,vector<int>,greater<int>> q;
// 大根堆 默认
priority_queue<int,vector<int>,less<int>> q;
```

## set map

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

## pair
```c++
pair<int,int> p;
p.first;
p.second;

// 离散化
// map<pair<int,int>,int>
```

## Hash

```c++
// 离散化 取模
// 无排序有去重 O(1)查询是否存在 退化为O(n)
// 卡哈希
unordered_set<int> use;
unordered_map<int,int> ump;
use.insert();
use.cound(val);
```

## math

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

## #define

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

