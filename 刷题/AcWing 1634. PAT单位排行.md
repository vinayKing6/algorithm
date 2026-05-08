---
tags:
  - 模拟
  - mid
date: 2026-03-14
练习次数: 1
---
# 题目

https://www.acwing.com/activity/content/problem/content/2206/

每次 PAT 考试结束后，考试中心都会发布一个考生单位排行榜。

本题就请你实现这个功能。

#### 输入格式

输入第一行给出一个正整数 N，即考生人数。

随后 N 行，每行按下列格式给出一个考生的信息：

```
准考证号 得分 学校
```

其中 `准考证号` 是由 6 个字符组成的字符串，其首字母表示考试的级别：`B` 代表乙级，`A` 代表甲级，`T` 代表顶级；`得分` 是 [0,100] 区间内的整数；学校是由不超过 6 个英文字母组成的单位码（大小写无关）。

注意：题目保证每个考生的准考证号是不同的。

#### 输出格式

首先在一行中输出单位个数。随后按以下格式非降序输出单位的排行榜：

```
排名 学校 加权总分 考生人数
```

其中 `排名` 是该单位的排名（从 1 开始）；`学校` 是**全部按小写字母**输出的单位码；`加权总分` 定义为 `乙级总分/1.5 + 甲级总分 + 顶级总分*1.5` 的**整数部分**；`考生人数` 是该属于单位的考生的总人数。

学校首先按加权总分排行。如有并列，则应对应相同的排名，并按考生人数升序输出。如果仍然并列，则按单位码的字典序从小到大输出。

#### 数据范围

1≤N≤$10^5$

#### 输入样例：

```
10
A57908 85 Au
B57908 54 LanX
A37487 60 au
T28374 67 CMU
T32486 24 hypu
A66734 92 cmu
B76378 71 AU
A47780 45 lanx
A72809 100 pku
A03274 45 hypu
```

#### 输出样例：

```
5
1 cmu 192 2
1 au 192 3
3 pku 100 1
4 hypu 81 2
4 lanx 81 2
```

# 思路
常规大模拟,注意浮点数精度问题

# 代码
```c++
#include<bits/stdc++.h>
using namespace std;
const int N=1e5+10;
int n,idx;
unordered_map<string,int> id;

struct School{
  string name;
  double score;
  int score_pro;
  int num;
};

School sch[N];

string lower(string s){
    for(int i=0;i<s.size();i++){
        int of=s[i]-'A';
        if(of>=0 && of<=25)
            s[i]=of+'a';
    }
    return s;
}

bool cmp(School s1,School s2){
    if(s1.score_pro != s2.score_pro){
        return s1.score_pro>s2.score_pro;
    }else if(s1.num != s2.num){
        return s1.num<s2.num;
    }else{
        return s1.name<s2.name;
    }
}


int main(){
    cin>>n;
    for(int i=0;i<n;i++){
        string lic;
        double score;
        string s_name;
        cin>>lic>>score>>s_name;
        score+=1e-9;
        s_name=lower(s_name);
        if(!id.count(s_name)){
            sch[idx].name=s_name;
            id[s_name]=idx++;
        }
        int s_id=id[s_name];
        if(lic[0] == 'B')
            sch[s_id].score+=(score/1.5);
        else if(lic[0] == 'A')
            sch[s_id].score+=score;
        else
            sch[s_id].score+=score*1.5;
        sch[s_id].num++;
    }
    
    for(int i=0;i<idx;i++){
        sch[i].score_pro=int(sch[i].score);
    }
    sort(sch,sch+idx,cmp);
    
    cout<<idx<<endl;
    int rank=1;
    int same=1;
    for(int i=0;i<idx;i++){
        if(i && sch[i].score_pro == sch[i-1].score_pro){
            same++;
        }else if(i){
            rank+=same;
            same=1;
        }
        cout<<rank<<" "<<sch[i].name<<" "<<sch[i].score_pro<<" "<<sch[i].num<<endl;
    }
}
```