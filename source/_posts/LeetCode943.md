---
title: 最短超级串
date: 2018-11-23 13:10:00
categories:
  - LeetCode
tags:
  - 数学
  - DP
  - 状压DP
  - 字符串
---

[传送门](https://leetcode-cn.com/contest/weekly-contest-111/problems/find-the-shortest-superstring/)

## 题面

给定一个字符串数组 A，找到以 A 中每个字符串作为子字符串的最短字符串。

我们可以假设 A 中没有字符串是 A 中另一个字符串的子字符串。

## 数据范围

1 <= A.length <= 12

1 <= A[i].length <= 20

---

## 题解

一开始想当然先预处理两两字符串之间的重叠部分

e[i][j] 表示 字符串i的后缀 与 字符串j的前缀 的最大重叠长度

找不到什么骚操作,暴力求解了

```cpp
for(int i = 0; i < A.size(); ++i)
{
    for(int j = 0; j < A.size(); ++j)
    {
        if(i == j) continue;
        int flag = 0;
        for(int si = 0; si < A[i].length(); ++si)
        {
            int p = si, q = 0;
            while(p < A[i].length() && q < A[j].length() && A[i][p] == A[j][q]) p++, q++;
            if(p == A[i].length())
            {
                flag = A[i].length()-si;
                break;
            }
        }
        e[i][j] = flag;
    }
}
```

之后感觉问题的模型已经出来了

求**最长路**

于是我以 Dijiskra 为原型写了四五遍,最终

发现做不出来...唉,所以比赛就结束了

后来发现最短路的什么三角形法则根本无法适用于最长路

于是又尝试 Bellmand

就发现啊,这是个带0边还有正环的有向图

额,0边没影响,有向也没事,可是正环...不存在最长路...

最终提炼出来的模型是:**带正环的图,每个点只能经过一次,求最长路,输出路径**

怎么办,最短路算法都没辙,那就...暴力

毕竟数据这么小,想也不是N方算法跑出来的

想法1:枚举每种连接的方案,全排列 O(n!)

```cpp
void dfs(int cur, int last, vector<string> &A, string now)
{
    if(now.length() > ans.length()) return;
    if(cur >= A.size())
    {
        if(now.length() < ans.length()) ans = now;
        return;
    }
    
    for(int i = 0; i < A.size(); ++i)
    {
        if(vis[i]) continue;
        vis[i] = 1;
        dfs(cur+1, i, A, now+A[i].substr(e[last][i]));
        vis[i] = 0;
    }
}
```

妥妥T了

想法2: 状压DP O(N^2*2^N)

一直卡了一星期终于做出来...

定义状态 dp[i][s]

s表示二进制下当前以及访问过的点,i表示当前状态下最后访问的点,存储的值表示当前最小长度

状态转移方程: `dp[i][s] = min( dp[j][s^(1<<i)] + A[j].length() - e[j][i] )`

二进制的骚操作不多说了,其中j是s中访问过的某个点

初始化: `dp[i][1<<i] = A[i].length()`, 其余设为 `INF`(无穷大)

最终结果: `max( dp[i][(1<<A.size())-1])`

```cpp
memset(dp, 0x3f, sizeof dp);
memset(last, -1, sizeof last);
for(int i = 0; i < A.size(); ++i) dp[i][0] = 0, dp[i][1<<i] = A[i].length();
for(int i = 1; i < (1<<A.size()); ++i)
    for(int now = 0; now < A.size(); ++now)
        if(i&(1<<now))
            for(int pre = 0; pre < A.size(); ++pre)
                if((i&(1<<pre)) && now != pre)
                {
                    int tmp = dp[pre][i^(1<<now)]+A[now].length()-e[pre][now];
                    if(dp[now][i] > tmp)
                    {
                        dp[now][i] = tmp;
                        last[now][i] = pre;
                    }
                }

int resi = 0, nows = (1<<A.size())-1;
for(int i = 1; i < A.size(); ++i)
    if(dp[i][nows] < dp[resi][nows])
        resi = i;
cout << dp[resi][nows] << endl; //////
```

然后还有一个头疼的地方是输出路径

用 `last[i][s]` 记录一下前驱,就是记录 `j`

倒过来找,再翻回去,这样就求出了字符串的排列

```cpp
vector<int> ress;
while(resi != -1)
{
    cout << resi << " ";// << nows << endl; ///////
    ress.push_back(resi);
    // 此处需注意 resi 和 nows 是否改变
    nows ^= 1<<resi;
    resi = last[resi][nows|(1<<resi)];
}
reverse(ress.begin(), ress.end());
```

最后是拼起来,这个也容易

仔细观察两个字符串拼起来的就是 `A[i]+A[j].substr(e[i][j])`

---

## 代码

```cpp
class Solution {
public:
    static const int Maxn = 13;
    int e[Maxn][Maxn], dp[Maxn][1<<Maxn], last[Maxn][1<<Maxn];
    
    string shortestSuperstring(vector<string>& A) {
        for(int i = 0; i < A.size(); ++i)
        {
            for(int j = 0; j < A.size(); ++j)
            {
                if(i == j) continue;
                int flag = 0;
                for(int si = 0; si < A[i].length(); ++si)
                {
                    int p = si, q = 0;
                    while(p < A[i].length() && q < A[j].length() && A[i][p] == A[j][q]) p++, q++;
                    if(p == A[i].length())
                    {
                        flag = A[i].length()-si;
                        break;
                    }
                }
                e[i][j] = flag;
            }
        }
        /*/debug
        for(int i = 0; i < A.size(); ++i)
        {
            for(int j = 0; j < A.size(); ++j)
                cout << e[i][j] << " ";
            cout << endl;
        }
        //debug */
        memset(dp, 0x3f, sizeof dp);
        memset(last, -1, sizeof last);
        for(int i = 0; i < A.size(); ++i) dp[i][0] = 0, dp[i][1<<i] = A[i].length();
        for(int i = 1; i < (1<<A.size()); ++i)
            for(int now = 0; now < A.size(); ++now)
                if(i&(1<<now))
                    for(int pre = 0; pre < A.size(); ++pre)
                        if((i&(1<<pre)) && now != pre)
                        {
                            int tmp = dp[pre][i^(1<<now)]+A[now].length()-e[pre][now];
                            if(dp[now][i] > tmp)
                            {
                                dp[now][i] = tmp;
                                last[now][i] = pre;
                            }
                        }
        
        int resi = 0, nows = (1<<A.size())-1;
        vector<int> ress;
        string res;
        
        for(int i = 1; i < A.size(); ++i)
            if(dp[i][nows] < dp[resi][nows])
                resi = i;
        cout << dp[resi][nows] << endl; //////
        while(resi != -1)
        {
            cout << resi << " ";// << nows << endl; ///////
            ress.push_back(resi);
            nows ^= 1<<resi;
            resi = last[resi][nows|(1<<resi)];
        }
        reverse(ress.begin(), ress.end());
        res = A[ress[0]];
        for(int i = 1; i < ress.size(); ++i)
            res += A[ress[i]].substr(e[ress[i-1]][ress[i]]);
        return res;
    }
};
```