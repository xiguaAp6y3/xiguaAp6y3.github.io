---
title: 算法学习Day1: 牛客周赛97与做题总结
published: 2025-06-23
description: "记录牛客周赛97的参赛过程、6道题目分析及3道补充题目，包含算法思路和代码实现"
image: "./cover.png"
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
https://ac.nowcoder.com/acm/contest/112078/A
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
https://ac.nowcoder.com/acm/contest/112078/B
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

- **题目描述**：暴力题
https://ac.nowcoder.com/acm/contest/112078/C
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

- **题目描述**：贪心题
https://ac.nowcoder.com/acm/contest/112078/D
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

- **题目描述**：贪心题
https://ac.nowcoder.com/acm/contest/112078/E
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
