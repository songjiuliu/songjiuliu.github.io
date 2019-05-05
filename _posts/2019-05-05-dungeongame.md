---
layout:     post
title:      Dungeon game
subtitle:   leetcode 174
date:       2019-05-05
author:     BY songjiu
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - 算法
--- 

## 题目描述
![](/img/dxcyxp1.jpg)

## 解题思想
  dp的思想,构建一个和原list相同维度的dp表，其中的每一位代表“走到该位置所需要的最小剩余生命值”，因此当构建完成该dp表之后，dp[0][0]就代表了英雄出发的最小所需要生命值。  
  开始构建dp表，由于需要得到dp[0][0]因此需要倒叙构建dp表，分**以下三种情况**：  
  1.**dp表最后一位**：也就是目的地需要的生命值，当该位置为正数时，只要生命值大于1就可以成功，该位置为负数时，需要生命值大于**1-该位置的值**以确保存活。  
  2.**dp表的最后一行与最后一列**。该位置处于特殊位置，因为最后一行与最后一列的位置，只能往**右边**走或者往**下边**走，因此计算：本身的位置-相应的右边一个的位置的值或者下面的对应位置的值（即dp[i][j]-dp[i+1][j]或dp[i][j]-dp[i][j+1]）。如果该值为正数，那么说明只要生命值大于1，就可以走到该位置，因此代价为1，如果算出来是负数，说明勇者至少还要有更多的生命值才能走到该位置，而勇者所必需的生命值为：abs（刚才算出来的值）。  
  3.**dp表的其他位置**。思想与2.类似，此时可以选择走两个方向：向下或者向右，因此分别计算该两个位置的代价(dp[i][j]-dp[i+1][j]和dp[i][j]-dp[i][j+1])，如果其中任何一个值大于0，那么说明勇者只要有1生命值就可以走到该位置存活，因此值为1，当二者计算出来都为负数的时候，选择其中绝对值更小的一个作为本位置的代价（即走到该位置仍然存活的最小生命值要求）。  

## 代码
```python
def calculateMinimumHP(self, dungeon: List[List[int]]) -> int:
        for i in range(len(dungeon)-1,-1,-1):
            for j in range(len(dungeon[i])-1,-1,-1):
                if j==len(dungeon[i])-1 and i<len(dungeon)-1:
                    if dungeon[i][j]-dungeon[i+1][j]>=0:
                        dungeon[i][j]=1
                    else:
                        dungeon[i][j]=-1*(dungeon[i][j]-dungeon[i+1][j])
                if i==len(dungeon)-1 and j<len(dungeon[i])-1:
                    if dungeon[i][j]-dungeon[i][j+1]>=0:
                        dungeon[i][j]=1
                    else:
                        dungeon[i][j]=-1*(dungeon[i][j]-dungeon[i][j+1])
                if i!=len(dungeon)-1 and j!=len(dungeon[i])-1:
                    temp1=dungeon[i][j]-dungeon[i+1][j]
                    temp2=dungeon[i][j]-dungeon[i][j+1]
                    if temp1>=0 or temp2>=0:
                        dungeon[i][j]=1
                    else:
                        dungeon[i][j]=-1*max(temp1,temp2)
                if i==len(dungeon)-1 and j==len(dungeon[i])-1:
                    dungeon[i][j]=1 if dungeon[i][j]>0 else 1-dungeon[i][j]
        return dungeon[0][0]
```