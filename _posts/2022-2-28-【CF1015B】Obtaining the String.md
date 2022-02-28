---
layout: article
title: 【CF1015B】Obtaining the String
mathjax: true
---

## 题意

​	两个长度为 $n$ 的字符串 $s$ 和 $t$ ，每次操作可以交换 $s$ 相邻字符的位置，问能否在 $10^4$ 次操作内将 $s$ 变为 $t$ ，若能，输出操作序列，否则输出-1。

<br>

## 思路

​	$n$ 最大只有50，所以 $10^4$ 次操作限制毫无卵用。

​	先判断输出-1的情况，统计 $s$ 和 $t$ 每个字母的数量，如果不相等就输出-1。

​	定义两个指针p1和p2，p1指向 $s$ 和 $t$ 的末尾，如果 $s[p1]==t[p1]$ ，把p1向左移动一位，继续比对。如果 $s[p1] \neq t[p1]$ ，在 $s$ 里从左往右找到一个 $s[p2]$ ，使得 $s[p2]==t[p1]$ ，通过一系列交换将p2处的字符移动至p1处，并输出 p2，p2+1，p2+2 ...... p1-1 。重复上述操作直到 $p1==0$ 。

<br>

## code

``` c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const ll maxn=1000000+10;
int n,cnt1[30],cnt2[30];

int main()
{
    while(cin>>n)
    {
        string s,t;
        cin>>s>>t;
        memset(cnt1,0,sizeof(cnt1));
        memset(cnt2,0,sizeof(cnt2));
        for(int i=0;i<s.size();i++) cnt1[s[i]-'a']++;
        for(int i=0;i<t.size();i++) cnt2[t[i]-'a']++;
        int flag=0;
        for(int i=0;i<26;i++)
        {
            if(cnt1[i]!=cnt2[i])
            {
                flag=1;
                break;
            }
        }
        if(flag)
        {
            cout<<"-1\n";
            continue;
        }
        vector <int> ans;
        int len=s.size();
        for(int i=len-1;i>=0;i--)
        {
            if(s[i]==t[i]) continue;
            int j=0;
            while(s[j]!=t[i]) j++;
            for(;j<i;j++)
            {
                ans.push_back(j);
                swap(s[j],s[j+1]);
            }
        }
        cout<<ans.size()<<"\n";
        for(int num:ans) cout<<num+1<<" ";
        cout<<"\n";
    }
    return 0;
}
```
