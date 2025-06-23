---
title: 算法学习Day1: 牛客周赛97与做题总结
published: 2025-06-23
description: "记录牛客周赛97的参赛过程、6道题目分析及3道补充题目，包含算法思路和代码实现"
image: "./cover.jpg"
tags: ["算法", "竞赛", "牛客", "编程"]
category: Algorithm
draft: false
---

# 算法学习Day1: 牛客周赛97与做题总结

## 前言

今天参加了牛客周赛97，收获颇丰。本文记录参赛过程、题目分析及个人总结。
赛时AC了5道，后面会进行补充。

## 比赛过程

- 比赛时间：2025年6月22日
- 参赛平台：牛客网
- 题目数量：6题

## 比赛题目分析

### A题

- **题目描述**：签到题  
### 题目描述
Bingbong 给定一个字母集合 $S$，包括三个字母 $s_1, s_2, s_3$，请你判断这三个字母能否随机打乱组成一个回文串。

### 输入描述
在一行上输入三个小写字母 $s_1, s_2, s_3$。字符间使用单个空格分隔。

### 输出描述
如果可以组成回文串，输出 YES，否则输出 NO。
- **解题思路**：
- **代码实现**：
    ```cpp
    // 代码实现
#include <bits/stdc++.h>
#define endl "\n"
const int N = 1e5 + 10;
using namespace std;
typedef long long  ll;
typedef pair<int,int> pii;
int n;
int main()
{
	ios::sync_with_stdio(false);
	cin.tie(0),cout.tie(0);
	char s1,s2,s3;
	cin>>s1>>s2>>s3;
	if(s1==s2||s2==s3||s1==s3) cout<<"YES";
	else cout<<"NO";
}
    ```

### B题

- **题目描述**：签到题
### 题目描述
Bingbong 认为，一个年份若满足：其各个数位的乘积是一个完全平方数，那么这就是特别的一年。例如：2025 是特别的一年，因为 $2 \times 0 \times 2 \times 5 = 0$是一个完全平方数。

现在给定你一个新的年份$n$，请你判断其是不是特殊的一年。

#### 【名词解释】
完全平方数：一个数如果可以表示为某个整数的平方，那么这个数就是完全平方数。例如，前十个完全平方数是 $0,1,4,9,16,25,36,49,64,81$。

### 输入描述
在一行上输入一个整数 $n (1 \leq n \leq 10^9)$，表示给定的年份。

### 输出描述
如果 $n$ 是特殊的一年，输出 YES，否则输出 NO。
- **解题思路**：
- **代码实现**：
    ```cpp
    // 代码实现
#include <bits/stdc++.h>
#define endl "\n"
const int N = 1e5 + 10;
using namespace std;
typedef long long  ll;
typedef pair<int,int> pii;
string a;
int main()
{
	ios::sync_with_stdio(false);
	cin.tie(0),cout.tie(0);
	cin>>a;
	ll aa = 1;
	for(int i = 0;i<int(a.length());i++)
	{
		aa *= a[i]-'0';
	}
	if(ll(sqrt(aa))*ll(sqrt(aa))==aa) cout<<"YES";
	else cout<<"NO";
}
    ```

### C题

- **题目描述**：### 题目描述
Bingbong 给定一条数轴上的一条道路，道路被划分为 $n$个位置，编号从$1$到$n$。每个位置要么为空地（使用 `'.'` 表示），要么为障碍点（使用 `'#'` 表示）。空地可自由通行，障碍不可通行。

对于一次从 $x$到$y$的移动，Bingbong 可以预先选择一个整数$k (k \geq 0)$，并在路径区间 $[\min(x, y), \max(x, y)]$上指定一个长度为$k$的连续子区间。在该子区间内，他可以无视障碍，将其视为空地；区间外只能在空地上行走。

求得使得 Bingbong 能够从$x$到$y$的最小整数$k$。

### 输入描述
- 第一行输入两个整数 $n, q (2 \leq n, q \leq 5 \times 10^5)$，分别表示道路长度和查询次数。
- 第二行输入一个长度为 $n$，仅由字符 `'.'` 和 `'#'` 构成的字符串 $s$，表示道路每个位置的初始情况。
- 此后 $q$行，第$i$行输入两个整数$x_i, y_i (1 \leq x_i, y_i \leq n)$，表示第 $i$次询问，保证$s_{x_i} = '.'$。

### 输出描述
对于每一次询问，新起一行输出一个整数，表示使得 Bingbong 能够从 $x$到$y$的最小整数$k$。
- **解题思路**：暴力实现
- **代码实现**：
    ```cpp
    // 代码实现
#include <bits/stdc++.h>
#define endl "\n"
const int N = 5e5 + 10;
using namespace std;
typedef long long  ll;
typedef pair<int,int> pii;
int n,q;
//int sum[N];
bool a[N];
int main()
{
	ios::sync_with_stdio(false);
	cin.tie(0),cout.tie(0);
	cin>>n>>q;
	char t;
	for(int i =1;i<=n;i++)
	{
	cin>>t;
		if(t=='.') a[i] = 0;
		else a[i] = 1;
	//	sum[i]= sum[i-1] + a[i];
	}
	while(q--)
	{
		int tmp1,tmp2;
		cin>>tmp1>>tmp2;
		int l  = min(tmp1,tmp2);
		int r  = max(tmp1,tmp2);
		bool flag = 0;
		for(int i = l;i<=r;i++)
		{
			if(a[i]==1) {l=i;flag = 1;break;}
		}
		if(!flag) {cout<<0<<endl;continue;}
		else
		{
			for(int i = r;i>=l;i--)
			if(a[i]==1) {r=i;break;}
			cout<<r-l+1<<endl;
		}
	}
}
    ```

### D题

- **题目描述**：### 题目描述

Bingbong 给定了一个长度为 $n$，仅由小写字母构成的字符串 $s$。你需要选择一个非空子串 $[1]$，并对该子串内所有字符同时进行若干次（可以为 $0$次）向右循环移位$[2]$操作。

Bingbong 希望使得修改过后的$s$在字典序$[3]$上最大化，请你帮助他求出操作后能得到的字典序最大的字符串。

**【名词解释】**$$\begin{align*}
&[1] \text{ 子串：从原字符串中，连续的选择一段字符（可以全选）得到的新字符串。} \\
&[2] \text{ 向右循环移位：将字符串中的每个字符变为其字母表中的后继字母。即，} a \to b, b \to c, \cdots, z \to a \text{。} \\
&[3] \text{ 相同长度字符串的字典序比较：从字符串的第一个字符开始逐个比较，直至发现第一个不同的位置，比较这个位置字符的字母表顺序，字母序更大的字符串字典序也更大。}
\end{align*}$$### 输入描述

第一行输入一个整数$n (1 \leq n \leq 2 \times 10^5)$，表示字符串长度。

第二行输入一个长度为 $n$、仅由小写字母构成的字符串 $s$，表示初始字符串。

### 输出描述

输出一个字符串，表示操作后能得到的字典序最大的字符串。
- **解题思路**：贪心，能贪就贪，从第一个不是z的字母(暂且称为a)开始，然后这个字母加到z,继续遍历到第一个比这个a大的字母，把前面的字符串（a~都加上 'z'-a;这个时候可以让整个字符串字典序最大。
- **代码实现**：
    ```cpp
    // 代码实现
#include <bits/stdc++.h>
#define endl "\n"
const int N = 1e5 + 10;
using namespace std;
typedef long long  ll;
typedef pair<int,int> pii;
int main()
{
	ios::sync_with_stdio(false);
	cin.tie(0),cout.tie(0);
	string a;
	int len;
	cin>>len>>a;
	int l  = 0;
	for(int i = 0;i<len;i++)
	{
		if(a[i]!='z') {l = i;break;}
	}
	int r = 0;
	int flag = 0;
	for(int i = l+1;i<len;i++)
	{
		if(a[i]>a[l]) {r= i;flag = 1; break;}
	}
	if(!flag) r = len;
	int num = 'z' - a[l];
	for(int i = l;i<r;i++)
	{
		a[i]+=num;
	}
	cout<<a;
}
    ```

### E题

- **题目描述**：### Bingbong 的「平衡排列」问题

Bingbong 称一个长度为 $n$的排列$p$为「平衡排列」，当且仅当存在一个索引$mid$（$1 \leq mid < n$），满足：

$$\sum_{i = 1}^{mid} p_i = \sum_{i = mid + 1}^{n} p_i$$现在给定整数$n$，请你构造一个长度为 $n$ 的「平衡排列」$p_1, p_2, \ldots, p_n$。若存在多解，输出任意一种；若无解，输出 $-1$。

#### 名词解释

长度为 $n$的排列：由$1, 2, \ldots, n$这$n$ 个整数，按任意顺序组成的数组（每个整数均恰好出现一次）。例如，$\{2, 3, 1, 5, 4\}$是一个长度为$5$的排列，而$\{1, 2, 2\}$和$\{1, 3, 4\}$ 都不是排列，因为前者存在重复元素，后者包含了超出范围的数。
- **解题思路**：贪心，先把所有数字都放到l 容器，然后找大的数字挪到左边，直到 sum(vector l) = sum(vector r);
- **代码实现**：
    ```cpp
    // 代码实现
#include <bits/stdc++.h>
#define endl "\n"
const int N = 1e5 + 10;
using namespace std;
typedef long long  ll;
typedef pair<int,int> pii;
int main()
{
	ios::sync_with_stdio(false);
	cin.tie(0),cout.tie(0);
	int t;
	cin>>t;
	while(t--)
	{
		ll n = 0;
		cin>>n;
		vector<int> l,r;
		for(int i = 1;i<=n;i++)
			l.push_back(i);
		ll sum = n*(n+1);
		sum/=2;
		if(sum%2!=0) {cout<<-1<<endl;continue;}
		sum/=2;
		bool flag = 0;
		for(int i = n-1;i>=0;i--)
		{
			if(sum>=l[i])
			{
				sum-=l[i];
				r.push_back(l[i]);
				l[i] = 0;
			}
			if(sum==0) {flag = 1;break;}
		}
		if(flag)
		{
			for(int i = 0 ;i<l.size();i++)
				if(l[i]!=0) cout<<l[i]<<' ';
			for(auto i:r) cout<<i<<' ';
		}
		else cout<<-1;
		cout<<endl;
	}
}
    ```

### F题

- **题目描述**：
- **解题思路**：
- **代码实现**：
    ```cpp
    // 代码实现
    ```

## 补充题目

### 补充题1

- **题目描述**：
- **解题思路**：
- **代码实现**：
    ```cpp
    // 代码实现
    ```

### 补充题2

- **题目描述**：
- **解题思路**：
- **代码实现**：
    ```cpp
    // 代码实现
    ```

### 补充题3

- **题目描述**：
- **解题思路**：
- **代码实现**：
    ```cpp
    // 代码实现
    ```

## 总结与反思

- 赛时心态要平稳，遇到难题及时切换思路
- 需要加强对常见算法和数据结构的掌握
- 赛后要及时复盘，查漏补缺

## 后续计划

- 每天坚持刷题，巩固基础
- 记录错题与易错点，持续进步