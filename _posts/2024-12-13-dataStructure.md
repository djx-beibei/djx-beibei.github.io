---
title: data structure 
categories: [acm]
comments: true
---

# 基础知识

万能头文件：

```c++
#include<bits/stdc++.h>
```

## 输入scanf

https://blog.csdn.net/uytguytgf/article/details/125537357

**1.scanf**

#include<stdio.h>

scanf("%d",&n);

数据表示：

%d 表示整数

%f 表示浮点数（小数）

%lf 表示双精度浮点数

 %c 表示一个字符

 %s 表示一个字符串

**2.cin**

遇到空格、TAB、换行读取结束，不能读取空格、TAB、换行

cin>>a;

**3.cin.get()**

遇换行符结束，可以接受空格

char ch = cin.get();

char ch; cin.get(ch); 

char ch[20]; cin.get(ch,20);

**cin.getline()**

可以接收空格，遇到换行结束，不接收换行（自己设定则接收），可以指定个数

char ch[20];

cin.getline(ch,5); //接收5个字符

cin.getline(ch, 5,'a');  //当输入jlkjkljkl时输出jklj，输入jkaljkljkl时，输出jk

**getline()**

可以接收空格。遇到换行结束，不接收换行（自己设定则接收），不可指定个数

#include<string>

getline(cin,str);

getline(cin,ch,'\n');

while(getline(cin,line)) 这个语句中，while判断语句的真实判断对象是cin的状态，也就是判断当前是否存在有效的输入流。（注意：这里默认回车符停止读入,按Ctrl+Z(Windows)(Ctrl+D(Linux))或键入EOF(参考MSDN)回车即可退出循环。）
**4.gets()**

可以接收空格。遇到换行结束，不接收换行

#include<string>

char ch[20];

gets(ch);

**5.getchar()**

接收一个字符的输入，包含空格，遇到换行符结束

#include<string>

char ch;

ch=getchar();

## 输出printf

保留2位小数

1. #include<iomanip>

   cout<<fixed<<setprecision(2)<<a<<endl;

2. printf("%.2f",a);

用3位输出一个整数，不够三位用0补齐

printf("%03d",a);

小数点前保留1位，小数点后保留3位

printf("%1.3d",a);

## 数学函数cmath

#include<cmath>

开根号sqrt()



## 字符数组函数string.h

#include<string.h>

数组清零 memset(num,0,sizeof(num));

# 数据结构



## stl模板


原文链接：https://blog.csdn.net/LiXinLong_LXL_13/article/details/140803870

标准模板库STL（‌Standard Template Library）‌是C++语言中的一个重要组成部分，‌它提供了一套通用的数据结构和算法，‌旨在提高软件开发的效率和可重用性。‌ STL主要由容器、‌算法和迭代器三个核心部分组成，‌这些组件共同构成了C++标准程序库的一部分，‌尽管STL本身并不是C++标准程序库的全部。‌

+ **容器container**：‌STL容器是用于存储数据的类，‌包括向量（‌vector）‌、‌双端队列（‌deque）‌、‌表（‌list）‌、‌队列（‌queue）‌、‌堆栈（‌stack）‌、‌集合（‌set）‌、‌多重集合（‌multiset）‌、‌映射（‌map）‌和多重映射（‌multimap）‌等。‌每种容器都有其独特的特点和适用场景，‌例如，‌向量适合频繁访问元素且可能在尾部进行大量插入和删除操作的场景，‌而集合则用于存储不重复的元素。‌
+ **算法algorithm**：‌STL算法是一组用于解决常见问题的有限步骤函数，‌它们不依赖于特定的容器类型，‌而是通过迭代器访问容器中的元素。‌这些算法可以高度通用，‌能够应用于多种不同的容器类型，‌例如排序（‌sort）‌、‌查找（‌find）‌、‌替换等。‌
+ **迭代器iterator**：‌迭代器是STL中的一个核心概念，‌它提供了一种统一的方法来访问容器中的元素。‌迭代器类似于指针，‌但比指针更加安全，‌因为它们被设计为只能进行有限的、‌安全的操作。‌

几乎所有的代码都采用了模板类和模板函数的方式，‌这相比于传统的由函数和类组成的库来说提供了更好的代码重用机会。‌STL的出现，‌使得C++编程语言在有了同Java一样强大的类库的同时，‌保有了更大的可扩展性。‌



### 向量vector

vector容器提供了许多操作，‌如push_back、‌pop_back、‌insert、‌erase等，‌以方便用户对容器中的元素进行添加、‌删除和修改。‌此外，‌vector还支持随机访问，‌即可以通过索引直接访问容器中的元素。‌

#include<vector>

定义容器 vector<int> v;

v.push_back(tmp);

v.pop_back()

v.begin()

v.end()



### 数组array

array是一个固定大小的数组容器，‌它提供了比原生数组更安全、‌更易于使用的接口

size()

front()

back()

data()



### 链表list

list是一个双向链表容器，‌它提供了一种灵活且高效的方式来管理对象的集合。其是一种线性数据结构，‌它允许在序列中的任何位置进行插入和删除操作，‌而不需要移动其他元素。‌这种特性使得list特别适合于需要频繁在中间位置进行插入和删除的操作场景。‌与数组和vector等线性数据结构相比，‌list的随机访问性能较差，‌因为它不支持通过索引直接访问元素。‌然而，‌它在处理需要动态添加和删除元素的情况时表现出色。‌

push_back 和 push_front：‌在列表的末尾和开头添加元素。‌
pop_front 和 pop_back：‌从列表的开头和末尾删除元素。‌
front 和 back：‌分别返回列表开头的元素和末尾的元素的引用。‌
begin 和 end：‌返回指向列表开始和结束的迭代器。‌
clear：‌删除列表中的所有元素。‌
访问prev和next指针





### 堆栈stack

stack是一种特殊的线性数据结构，其中元素的插入和删除操作都只能在容器的一端进行，即栈顶。它遵循**后进先出（LIFO）**的原则，意味着最后插入的元素将是第一个被删除的元素。这种数据结构非常适合用于实现函数调用、括号匹配等场景‌。

#include<stack>

定义栈 stack<int> s

进栈 s.push(tmp)

出栈 s.pop()

栈空 s.empty()

栈顶 s.top()

```c++
//洛谷 P1427 小鱼的数字游戏
#include<iostream>
#include<stack>
using namespace std;
int main()
{
    stack<int> s;
    int k;
    while(cin>>k){
        if(k==0) break;
        else
        {
            s.push(k);
        }
    }
    while(!s.empty())
    {
        cout<<s.top()<<' ';
        s.pop();
    }
    return 0;
}
```





```c++
//洛谷 P1739 表达式括号匹配 
#include<iostream>
#include<stack>
using namespace std;
int main()
{
	int n;
	cin>>n;
	stack<char> s;
	char ch;
	int flag=1;
	while(1)
	{
		ch=getchar();
		if(ch=='(') s.push(ch);
		else if(ch==')')
		{
			if(!s.empty()) s.pop();
			else
			{
				flag=0;
			}
		}
		else if(ch=='@')
		{
			if(!s.empty()) flag=0;
			break;
		}
	}
	if(flag) cout<<"YES"<<endl;
	else cout<<"NO"<<endl;
	return 0;
}
```





```c++
//洛谷 CF1997C Even Positions
#include<iostream>
#include<stack>
using namespace std;
int main()
{
	int n;
	cin>>n;
	
	char ch;
	while(n--)
	{
		int m;
		cin>>m;
		getchar();
		int ans=0;
		stack<char> s;
		for(int i=0;i<m;i++)
		{
			ch=getchar();
			
			if(ch=='(') s.push(i);
			else if(ch==')')
			{
				ans+=i-s.top();
				s.pop();
			}
			else if(ch=='_')
			{
				if(!s.empty())
				{
					ans+=i-s.top();
					s.pop();
				}
				else s.push(i);
			}
		}
		cout<<ans<<endl;
	}
	return 0;
}
```





### 队列queue

queue是一种单向的数据结构，元素只能按照它们进入队列的顺序依次出队，提供了一种**先进先出（FIFO）**的数据结构。其允许在队列的尾部添加元素（通过push操作），并在队列的头部移除元素（通过pop操作）。这种特性使得queue成为实现队列行为的理想选择。queue不允许遍历或随机访问其元素，除了在队列尾部添加元素和在队列头部移除元素外，没有任何其他方法可以存取queue的其他元素。

#include<queue>

定义队列 queue<int> q;

入队 q.push(tmp)

出队 q.pop()

队空 q.empty()

队首 q.front()



```c++
//洛谷 B3616 【模板】队列 
//不需要实现队列，直接使用stl模板里的queue函数
#include<iostream>
#include<queue>
using namespace std;
int main()
{
	int n;
	cin>>n;
	queue<int> q;
	while(n--)
	{
		int num,x;
		cin>>num;
		if(num==1)
		{
			cin>>x;
			q.push(x);
		}
		else if(num==2)
		{
			if(!q.empty()) q.pop();
			else cout<<"ERR_CANNOT_POP"<<endl;
		}
		else if(num==3)
		{
			if(!q.empty()) cout<<q.front()<<endl;
			else cout<<"ERR_CANNOT_QUERY"<<endl;
		}
		else if(num==4) cout<<q.size()<<endl;
	}
	return 0;
}
```





```c++
//洛谷 P1996 约瑟夫问题 
//又称约瑟夫斯置换，循环报数出圈
#include<iostream>
#include<queue>
using namespace std;
int main()
{
	int m,n;
	cin>>m>>n;
	queue<int> q;
	for(int i=1;i<=m;i++) q.push(i);
	int out=1;
	while(!q.empty())
	{
		if(n==out)
		{
			cout<<q.front()<<' ';
			q.pop();
			out=1;
		}
		else
		{
			out++;
			q.push(q.front());
			q.pop();
		}
	}
	return 0;
}
```



```c++
//P1540 [NOIP2010 提高组] 机器翻译 
//计算机内存和缓存的关系，cache的作用
#include<iostream>
#include<string.h>
#include<queue>
using namespace std;
int main()
{
	int m,n;
	cin>>m>>n;
	queue<int> q;
	int ans=0;
	int num[1010];
	memset(num,0,sizeof(num));
	while(n--)
	{
		int tmp;
		cin>>tmp;
		if(!num[tmp])//在队列中不存在 
		{
			if(q.size()==m)//队列已满 
			{	
				num[q.front()]=0;
				q.pop();
			}
			num[tmp]=1;
			q.push(tmp);
			ans++;
		}
	}
	cout<<ans<<endl;
	return 0;
}
```



### 集合set

https://blog.csdn.net/weixin_72357342/article/details/142545721

set 是一个容器，‌其中所包含的元素的值是唯一的。‌这意味着在set中，‌每个元素只能出现一次。‌set中的元素按一定的顺序排列，‌并被作为集合中的实例。‌

定义集合 set<int> s;

判断空 s.empty()

大小 s.size()

插入 s.insert(num)

返回set中值为x的元素的位置**iterator** it=s.find(3)

返回set中值为x的元素是否存在 ans=s.count(1)【如果容器包含等效于val的元素返回为1，否则为0】

删除 s.erase(it); s.erase(3); s.erase(s.begin(),s.find(5));

清空 s.clear()

```C++
//洛谷 P11227 扑克牌
//最不利构造问题，总数-所有不是的情况
#include<iostream>
#include<set>
using namespace std;
int main()
{
	int n;
	cin>>n;
	set<string> s;
	while(n--)
	{
		string tmp;
		cin>>tmp;
		s.insert(tmp);
	}
	cout<<52-s.size()<<endl;
	return 0;
}
```

multiset

允许重复键值，与set类似，‌但允许集合中的元素值重复。‌这意味着在multiset中，‌同一值可以出现多次。‌multiset同样提供了查找、‌插入和删除操作，‌并且这些操作的时间与multiset中元素个数的对数成比例关系。‌与set不同的是，‌multiset中的元素可以重复，‌这使得它在处理需要存储重复值的数据集时非常有用。‌


### 映射map

**关联式容器**

map用于存储键值对，‌不允许重复的键，容器中的元素根据键值进行排序存储

#include<map>

定义映射组 map<int,int> mp

插入 mp.insert()

mp.begin()

mp.end()

空映射 mp.empty()

映射组数量 mp.size()



multimap允许重复的键。‌



### 迭代器iterator

原文链接：https://blog.csdn.net/a_fool_fis/article/details/132215393

所有的容器都定义有自己的iterator类型，因比如果单单使用容器，只需要包含对应容器的头文件即可。不过有些特殊的iterator,被定义在头文件<iterator>中

定义迭代器 

```c++
//向量容器
#include <vector>
vector<int> nums = {1, 2, 3, 4, 5};
vector<int>::iterator it;
//链表
#include <list>
list<string> words = {"Hello", "World", "C++"};
list<string>::iterator it;
//队列
#include <deque>
deque<int> nums = {1, 2, 3, 4, 5};
deque<int>::iterator it;
//数组容器
#include <array>
array<int, 5> arr = {1, 2, 3, 4, 5};
array<int, 5>::iterator it;
//映射容器
#include <map>
map<int, string> myMap = {{1, "one"}, {2, "two"}, {3, "three"}};
map<int, string>::iterator it;
//栈
stack<int>::iterator it;
//···
```

使用 begin() 函数获取容器的起始迭代器。

使用 end() 函数获取容器的终止迭代器。

使用 * 运算符解引用迭代器，访问迭代器当前指向的元素。

使用 ++ 运算符递增迭代器，使其指向下一个元素。

使用 -- 运算符递减迭代器，使其指向前一个元素（适用于双向迭代器和随机访问迭代器）

```c++
//输入迭代器：
//用于从容器中读取元素，支持逐个元素地前进。
fot(it=nums.begin();it!=nums.end();++it)
{
    int value=*it;
    cout<<value<<" ";
}
//输出迭代器：
//用于向容器中写入元素，支持逐个元素地前进。
fot(it=nums.begin();it!=nums.end();++it)
{
    *it=rand()%10;
}
//正向迭代器：
//兼具输入和输出迭代器的功能，可以逐个元素地读取和写入，并支持递增和递减迭代器。

//双向迭代器
//拥有正向迭代器的所有功能，并且可以递减迭代器，即向前遍历容器中的元素。
fot(it=nums.begin();it!=nums.end();++it)
{
    cout<<*it<<" ";
}
fot(it=nums.end();it!=nums.begin();--it)
{
    cout<<*(it-1)<<" ";
}
//随机访问迭代器
//具备双向迭代器的所有功能，并且可以通过索引直接访问容器中的元素。支持迭代器的算术操作，例如 +、-、[]、+=、-= 等。
it = arr.begin() + 2;
cout << *it << endl;
it += 2;
cout <<it << endl;
it -= 1;
cout << *it << endl;
```

## 



