---
layout: article
title: 【CF1015D】Walking Between Houses
mathjax: true
---

## 题意

​	有一行房子，编号从 $1$ 到 $n$ ，起始位于房子1，每次移动可以到达除当前房子外任何一个（不能原地不动），移动距离为起点编号与终点编号差的绝对值。

​	你需要进行 $k$ 次移动，使移动距离的总和等于 $s$ ，如果可行输出YES并输出每次移动的终点编号，否则输出NO。

<br>

## 思路

​	先考虑不可行的情况，也就是 $s$ 过大或 $s$ 过小。

* 每次移动都移动尽可能多的距离，也就是从1到 $n$ 或是从 $n$ 到1，$k$ 次移动，最多可移动 $k(n-1)$ ，若 $s>k(n-1)$ ，则不可行。
* 每次移动尽量少的距离，也就是每次移动距离为1，$k$ 次移动距离就是 $k$ ，若$s<k$ ，也不可行。



​	若 $k\leq s\leq k(n-1)$ ，则可行：

​	首先算出最少需要几次移动可以使移动的总距离达到 $s$ ，也就是每次移动都移动尽可能多的距离。

​	假设至少需要 $a$ 次移动可以达到 $s$ ，此时必定 $a\leq k$ ，对于$a<k$ 的情况，我们可以将一次移动拆解为多次移动。比如某次移动为从1到 $n$ ，则可以拆解为从1到2和从2到 $n$，不难发现，一次距离为 $l$ 的移动最多可以被拆解为 $l$ 次距离为1的移动。我们可以通过拆解，将 $a$ 次移动变为 $k$ 次移动。

​	思路不难，主要是代码实现的细节需要考虑下。

<br>

## code

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const ll maxn=1000000+10;
ll n,k,s;

int main()
{
    while(cin>>n>>k>>s)
    {
        if(k*(n-1)<s||k>s)
        {
            cout<<"NO\n";
            continue;
        }
        vector <int> ans,ans1;
        int x=s/(n-1),flag=1;
        for(int i=0;i<x;i++)
        {
            if(flag) ans.push_back(n);
            else ans.push_back(1);
            flag^=1;
        }
        if(x*(n-1)!=s)
        {
            if(x&1) ans.push_back(n-(s-x*(n-1)));
            else ans.push_back((s-x*(n-1))+1);
        }
        int len=ans.size(),from=1,to=ans[0],pos=0;
        while(len<k)
        {
            for(int i=from<to?from+1:from-1;i!=to;i>to?i--:i++)
            {
                if(len>=k) break;
                ans1.push_back(i);
                len++;
            }
            ans1.push_back(ans[pos]);
            pos++;
            from=to,to=ans[pos];
        }
        for(;pos<ans.size();pos++) ans1.push_back(ans[pos]);
        cout<<"YES\n";
        for(int num:ans1) cout<<num<<" ";
        cout<<"\n";
    }
    return 0;
}
```
