---
layout: article
title: 【题解】Codeforces Round #699 (Div. 2) (A~C)
mathjax: true
---


<br/>

[Dashboard - Codeforces Round #699 (Div. 2) - Codeforces](https://codeforces.com/contest/1481)

## A. Space Navigation

### 题意

​	从坐标（0，0）出发，给你终点的坐标，以及一个字符串，字符串由U、D、R、L四个大写字母组成，代表向四个方向移动，可任意删去字符串中字母，也可不删去，问经过删除后是否能恰好抵达终点。

<br/>

### 思路

​	记录四个字母的数量，然后根据终点在四个象限分别判断即可。

<br/>

### code

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;



int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    int t;
    cin>>t;
    while(t--)
    {
        int px,py,len,U=0,D=0,R=0,L=0,flag=0;
        cin>>px>>py;
        string s;
        cin>>s;
        len=s.size();
        for(int i=0;i<len;i++)
        {
            if(s[i]=='U') U++;
            else if(s[i]=='D') D++;
            else if(s[i]=='R') R++;
            else if(s[i]=='L') L++;
        }
        if(px>=0&&py>=0&&U>=py&&R>=px) flag=1;
        else if(px<=0&&py>=0&&U>=py&&L>=-px) flag=1;
        else if(px<=0&&py<=0&&D>=-py&&L>=-px) flag=1;
        else if(px>=0&&py<=0&&D>=-py&&R>=px) flag=1;
        if(px==0&&py==0) flag=1;
        if(flag)
            cout<<"YES"<<"\n";
        else
            cout<<"NO"<<"\n";
    }
    return 0;
}
```

<br/>

<br/>

## B. New Colony

### 题意

​	一个数组 $h$ ，下标从1开始，每次操作找到从左往右第一个下标 $i$ 使得$h_i<h_{i+1}$ ，然后给 $h_i$ 加上1，问第 $k$ 次操作找到的 $i$ 是多少，若 $i$ 不存在则输出-1。

<br/>

### 思路

考虑到数据范围较小，直接暴力模拟。

<br/>

### code

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int h[110];

int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    int t;
    cin>>t;
    while(t--)
    {
        int n,k,flag=1,maxx=0,ans=-1;
        cin>>n>>k;
        for(int i=1;i<=n;i++)
        {
            cin>>h[i];
        }
        for(int i=2;i<=n;i++)
        {
            if(h[i]>h[i-1])
            {
                flag=0;
                break;
            }
        }
        if(flag||k>10000)
        {
            cout<<"-1\n";
            continue;
        }
        while(1)
        {
            int fuck=1;//过于文明
            for(int i=2;i<=n;i++)
            {
                if(h[i]>h[i-1])
                {
                    fuck=0;
                    k--;
                    if(k<=0)
                    {
                        ans=i-1;
                        break;
                    }
                    h[i-1]++;
                    break;
                }
            }
            if(fuck)
                break;
            if(ans!=-1)
                break;
        }
        cout<<ans<<"\n";
    }
    return 0;
}
```

<br/>

<br/>

## C. Fence Painting

### 题意

​	一个数组 $a$ 表示初始数组，一个数组 $b$ 表示目标数组，以及一个数组 $c$ ，按照下标的顺序将数组 $c$ 中的元素覆盖数组 $a$ 中任意位置的元素，使得数组 $a$ 变为数组 $b$ ，$c$ 中每个数都要使用，且只能使用一次，可以重复覆盖，若可以达到目的则输出"YES"并输出 $c$ 中每个数所覆盖的 $a$ 中的值的下标，若不能输出"NO"。

<br/>

### 思路

​	先判断什么情况下无法达到目的:

- $a$ 中需要修改的元素数量超过 $c$ 的元素数量
- $b$ 中的某元素在 $a$ 和 $c$ 中都不存在
- $c$ 最后一个元素在 $b$ 中不存在
- 需要将 $a$ 中 $x$ 个数修改为 $b_i$ 但 $c$ 中 $b_i$ 的数量少于 $x$

​	出现以上情况任何一种直接输出"NO"。

​	我们先找到 $c$ 中最后一个数$c_m$ ，并找到在 $a$ 中它应覆盖的元素，记录其下标，因为 $c_m$ 在最后才使用，所以 $a$ 中这个位置之前不管是什么值最后都会被 $c_m$ 覆盖，因此这个位置可以当作一个“垃圾桶”，使 $c$ 中没用的数都覆盖这个位置。

<br/>

### code

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int a[100000+10],b[100000+10],c[100000+10],vis[100000+10],vis1[100000+10],ans[100000+10];
set <int> se[100000+10];


int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    int t;
    cin>>t;
    while(t--)
    {
        map <int,int> want,painter;
        memset(vis,0,sizeof(vis));
        memset(vis1,0,sizeof(vis1));
        int n,m,flag=0,p,index=-1,tot=0,len=0;
        cin>>n>>m;
        for(int i=0;i<=max(n,m);i++)
            se[i].clear();
        for(int i=1;i<=n;i++)
        {
            cin>>a[i];
        }
        for(int i=1;i<=n;i++)
        {
            cin>>b[i];
            want[b[i]]++;
            vis1[b[i]]=1;
        }
        for(int i=1;i<=m;i++)
        {
            cin>>c[i];
            painter[c[i]]++;
            vis[c[i]]=1;
        }
        p=c[m];
        for(int i=1;i<=n;i++)
        {
            if(a[i]==b[i])
            {
                want[b[i]]--;
                continue;
            }
            if(vis[b[i]]==0)
            {
                flag=1;
                break;
            }
            if(p==b[i])
                index=i;
            len++;
            se[b[i]].insert(i);
        }
        for(auto it=want.begin();it!=want.end();it++)
        {
            if(it->second>painter[it->first])
            {
                flag=1;
                break;
            }
        }
        if(vis1[p]==0)
            flag=1;
        if(flag||len>m)
        {
            cout<<"NO\n";
            continue;
        }
        for(int i=1;i<=n;i++)
        {
            if(index!=-1) break;
            if(b[i]==p)
            {
                index=i;
                break;
            }
        }
        for(int i=1;i<m;i++)
        {
            if(want[c[i]]==0)
            {
                ans[i]=index;
            }
            else
            {
                auto it=se[c[i]].begin();
                ans[i]=*it;
                want[c[i]]--;
                se[c[i]].erase(it);
            }
        }
        ans[m]=index;
        cout<<"YES\n";
        for(int i=1;i<=m;i++)
        {
            cout<<ans[i]<<" ";
        }
        cout<<"\n";
    }
    return 0;
}
```

<br/>

<br/>

剩下的不会，摸了。
