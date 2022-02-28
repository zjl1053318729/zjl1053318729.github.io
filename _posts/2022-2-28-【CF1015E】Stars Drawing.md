---
layout: article
title: 【CF1015E】Stars Drawing
mathjax: true
---

## 题意

​	给你一副由 '.' 和 '*' 组成的图，问你能否用任意多个由’\*‘组成的十字组成该图，十字大小任意，且可以互相交叉、覆盖。

​	十字不能是一个点，也就是十字最小是：

​	![image-20210417001632472](https://picgozjl.oss-cn-beijing.aliyuncs.com/img/image-20210417001632472.png)

<br>

## 思路

​	没看出easy和hard本质的区别（可能hard有点卡常？），暴力，遍历整张图，遇到可以放十字的地方就放个尽可能大的十字，记录十字的位置和大小，并标记被十字覆盖的地方。遍历后与所给的图对比，看是否有'*'但该处未被标记，若有则输出-1。

<br>

## code

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const ll maxn=1000+10;
int cnt[maxn][maxn],n,m;
string g[maxn];
struct node
{
    int i,j,len;
    node(int ii,int jj,int len1) {i=ii,j=jj,len=len1;}
};
vector <node> ans;

int check(int i,int j)
{
    if(g[i][j]!='*') return 0;
    if(i==0||j==0||i==n-1||j==m-1) return 0;
    if(g[i+1][j]!='*'||g[i-1][j]!='*'||g[i][j+1]!='*'||g[i][j-1]!='*') return 0;
    return 1;
}
void solve(int i,int j)
{
    int maxx=1,k=1,res=0x3f3f3f3f;
    while(i+k<n&&g[i+k][j]=='*') maxx=max(maxx,k),k++;
    res=min(res,maxx),k=1,maxx=1;
    while(i-k>=0&&g[i-k][j]=='*') maxx=max(maxx,k),k++;
    res=min(res,maxx),k=1,maxx=1;
    while(j+k<m&&g[i][j+k]=='*') maxx=max(maxx,k),k++;
    res=min(res,maxx),k=1,maxx=1;
    while(j-k>=0&&g[i][j-k]=='*') maxx=max(maxx,k),k++;
    res=min(res,maxx),k=1,maxx=1;
    for(int l=0;l<=res;l++)
    {
        cnt[i+l][j]=1;
        cnt[i-l][j]=1;
        cnt[i][j+l]=1;
        cnt[i][j-l]=1;
    }
    ans.push_back(node(i,j,res));
}

int main()
{
    ios::sync_with_stdio(0);
    while(cin>>n>>m)
    {
        for(int i=0;i<n;i++) cin>>g[i];
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                if(check(i,j)) solve(i,j);
            }
        }
        int flag=0;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                if(g[i][j]=='*'&&cnt[i][j]==0)
                {
                    flag=1;
                    break;
                }
            }
            if(flag) break;
        }
        if(flag) cout<<"-1\n"; 
        else
        {
            cout<<ans.size()<<"\n";
            for(node a:ans) cout<<a.i+1<<" "<<a.j+1<<" "<<a.len<<"\n";
        }
    }
    return 0;
}
```
