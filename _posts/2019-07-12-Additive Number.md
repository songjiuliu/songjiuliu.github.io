---
layout:     post
title:      Additive Number 
subtitle:   力扣304
date:       2019-07-12
author:     songjiu
header-img: img/post-bg-2015.jpg
catalog: true
tags:
    - 算法
---

累加数是一个字符串，组成它的数字可以形成累加序列。
### 题目概述
[题目概述](https://leetcode-cn.com/problems/additive-number/)
### 题目分析
dfs并减枝，利用栈，当里面不满足至少有两个数字的时候，就直接入栈，满足的话就进行检查，将合法的数字放到栈里面，不合法就出栈，继续遍历。（因为都是正整数，所以序列后面的数字长度肯定大于等于他前面的，这样dfs的时候可以减少一定的数量，但是我还没写。）**注意数字0不能作为首位**，因此当他出现在首位的时候，如果指示器已经遍历到非首位的位置了，直接return False。
### 代码
```python
    def dfs(self,nums):
        if len(nums)==0:
            if len(self.res)>2:
                for j in range(len(self.res)-2):
                    if self.res[j]+self.res[j+1]!=self.res[j+2]:
                        return False
                return True
            else:
                return False
        for i in range(len(nums)):
            if nums[0]=='0':#注意数字0不能作为首位，因此当他出现在首位的时候，如果指示器已经遍历到非首位的位置了，直接return False
                if i!=0:
                    return False
            if len(self.res)<2:
                self.res.append(int(nums[:i+1]))
                #print(self.res)
                if self.dfs(nums[i+1:]):
                    return True
                else:
                    self.res.pop(-1)
            else:
                
                if self.res[-2]+self.res[-1]==int(nums[:i+1]):
                    self.res.append(int(nums[:i+1]))
                    if self.dfs(nums[i+1:]):
                        return True
                    else:
                        self.res.pop(-1)
                else:
                    continue
        return False
    def isAdditiveNumber(self, num: str) -> bool:
        self.res=[]
        return self.dfs(num)
                
```