---
title: 全排列和全排列II
date: 2022-07-14 15:05:21
tags: 
  - 回溯
categories:
  - 刷题
---

## 全排列

给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

### 示例

**输入：**nums = [1,2,3] 

**输出：**[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

### 思路

本题与我以前做的可以使用回溯来解决的题不同，以前做的大多都是关于组合的题，而本题是一道排列题；一般解组合题都是每次查找从下一个位置开始来消除重复组合，而不同顺序的排列是不一样的，所以可以从初始位置开始查找，但是需要排除已经选择过的元素。这里我们使用一个与`nums`长度相同的`used`数组来表示元素是否被选择过，`used[i]==1`表示被选择过。

![QQ截图20220714153605](https://s2.loli.net/2022/07/14/HzSPfg6UIcKdVAj.png)

* 递归函数参数：结果集`ans`，一次排列`per`，数组`nums`，`used`数组标记已选择元素
* 终止条件：排列`per`长度等于`nums`的长度
* 单层逻辑：遍历数组，如果元素没有被选择过，即`used[i]==0`，则将`nums[i]`加入排列`per`，再进入下一层选择下一个元素

### 实现代码

~~~java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> ans=new ArrayList<>();
        List<Integer> per=new ArrayList<>();
        int[] used=new int[nums.length];
        dfs(ans,per,nums,used);
        return ans;
    }

    public void dfs(List<List<Integer>> ans,List<Integer> per,int[] nums,int[] used) {
        if(per.size()==nums.length) {
            ans.add(new ArrayList<>(per));
            return;
        }
        for(int i=0;i<nums.length;i++) {
            if(used[i]==0) {
                per.add(nums[i]);
                used[i]=1;
                dfs(ans,per,nums,used);
                per.remove(per.size()-1);
                used[i]=0;
            }
        }
    }
}
~~~

### 不使用used数组的思路

在选择了`x`个元素后，还需要选择`n-x`个元素，那么我们可以将数组`nums`分为两部分，已选择的和未选择的，如图，在每次要选择元素时，将要选择的元素与待选元素序列中的第一个元素交换，选择后，数组自然分为了前半部分是已选择的，后半部分是未选择的，然后下一次选择就从后半部分开始。

![QQ截图20220713214134](https://s2.loli.net/2022/07/14/HUrlJEybAjYkSuG.png)

这样不仅可以避免了used数组的使用，并且还可以少一层递归，因为最后一个元素必定在最后。

* 递归函数参数：结果集`ans`，一次排列`per`，数组`nums`，标记选择开始位置`i`
* 终止条件：可以判断`per.size()==nums.length-1`或者`i==nums.length-1`，最后要将最后一个元素加入排列
* 单层逻辑：遍历`nums`，将元素与位置`i`的元素交换，将元素加入排列，`i`加1，进行下一层的选择

### 实现代码2

~~~java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> ans=new ArrayList<>();
        List<Integer> per=new ArrayList<>();
        dfs(ans,per,nums,0);
        return ans;
    }

    public void dfs(List<List<Integer>> ans,List<Integer> per,int[] nums,int i) {
        if(i==nums.length-1) {
            per.add(nums[i]);
            ans.add(new ArrayList<>(per));
            per.remove(per.size()-1);
            return;
        }
        for(int j=i;j< nums.length;j++) {
            int tmp=nums[i];
            nums[i]=nums[j];
            nums[j]=tmp;
            per.add(nums[i]);
            dfs(ans,per,nums,i+1);
            int tmp1=nums[i];
            nums[i]=nums[j];
            nums[j]=tmp1;
            per.remove(per.size()-1);
        }
    }
}
~~~

## 全排列II

给定一个可包含重复数字的序列 `nums` ，***按任意顺序*** 返回所有不重复的全排列。

### 示例

**输入：**nums = [1,1,2] 

**输出：** [[1,1,2], [1,2,1], [2,1,1]]

### 思路

一般对于需要去重的问题我们先排个序，然后按照上边全排列的解法能得到这样一棵树：

![image-20220714191921288](https://s2.loli.net/2022/07/14/wcFZtpWv4EPdKH8.png)

可以看到有重复的排列，如何去除重呢？从这张图来看，重复的排列是因为在同一层选择了相同的数，所以只需要避免在同一层选择相同的数就行了。

![image-20220714192418138](https://s2.loli.net/2022/07/14/AOIaN5ZW3fq4Kpb.png)

如何避免呢，当将要选择相同数时，因为排过序，所以肯定有`nums[i]==nums[i-1]`，如何判断前一个数是否被选择过呢，如果`used[i-1]==0`，那么它肯定会在这一层被选中，所以只需要在满足这两个条件时跳过本轮循环。

### 实现代码

~~~java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> ans=new ArrayList<>();
        List<Integer> per=new ArrayList<>();
        int[] used=new int[nums.length];
        dfs(ans,per,nums,used);
        return ans;
    }

    public void dfs(List<List<Integer>> ans,List<Integer> per,int[] nums,int[] used) {
        if(per.size()==nums.length) {
            ans.add(new ArrayList<>(per));
            return;
        }
        for(int i=0;i<nums.length;i++) {
            if(i-1>=0&&nums[i]==nums[i-1]&&used[i-1]==0)
                continue;
            if(used[i]==0) {
                per.add(nums[i]);
                used[i]=1;
                dfs(ans,per,nums,used);
                per.remove(per.size()-1);
                used[i]=0;
            }
        }
    }
}
~~~

