---
title: 子集I和子集II
date: 2022-07-13 13:51:51
tags:
  - 回溯
categories:
  - 刷题
---

## 子集问题

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

说明：解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

### 示例

**输入：**nums = [1,2,3] 

**输出：**[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

### 思路

从数组中选一个数，再从这个数后的数组元素中选下一个数，一直重复，直到没有待选数组元素，如图：

![image-20220713141304299](https://s2.loli.net/2022/07/13/CeQlVmfboPAx39t.png)

为什么要在选中数后的数组元素中选呢，因为这是一个组合问题，如果在整个数组中选，会导致重复的结果出现。

这个问题与我以前做过的可以用回溯解决的题比如组合、[分割回文串](https://mizore.site/article/分割回文串/)等题有些不同，前面的题都是搜索到叶子节点才将搜索结果加入结果集，本题在每个节点都要将搜索结果加入结果集。

* 递归函数参数：要求子集的数组`nums`，待选数组元素开始位置`start`，结果集`ans`，子集`subset`
* 终止条件：没有待选数组元素，也就是`start==nums.length`
* 单层逻辑：从待选数组元素中依次选择一个数加入子集，并且将子集加入结果集，将`start`移到选择的数位置后一位，然后进行下一层选择，本次循环最后将加入到子集的数移除再进入下一次循环

### 实现代码

~~~java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> ans=new ArrayList<>();
        List<Integer> subset=new ArrayList<>();
        ans.add(new ArrayList<>(subset));
        backtracking(nums,0,ans,subset);
        return ans;
    }

    public void backtracking(int[] nums,int start,List<List<Integer>> ans,List<Integer> subset) {
        if(start==nums.length)
            return;
        for(int i=start;i<nums.length;i++) {
            subset.add(nums[i]);
            ans.add(new ArrayList<>(subset));
            backtracking(nums,i+1,ans,subset);
            subset.remove(subset.size()-1);
        }
    }

}
~~~

## 子集II

给你一个整数数组 `nums` ，其中可能包含**重复元素**，请你返回该数组所有可能的子集（幂集）。

说明：解集 **不能** 包含重复的子集。返回的解集中，子集可以按 **任意顺序** 排列。

### 示例

**输入：**nums = [1,2,2]

**输出：**[[],[1],[1,2],[1,2,2],[2],[2,2]]

### 思路

本题与子集I不同的地方在于数组中包含重复元素，首先按照子集I的解法来解：

![image-20220713152910955](https://s2.loli.net/2022/07/13/LxYobzsW495Jjer.png)

可以看到结果集中会有重复，出现重复的关键在于选择了相同的数

![image-20220713153507534](https://s2.loli.net/2022/07/13/jX1MxWqJuUFsiLt.png)

所以只需要在子集I的解法基础上，先将数组排序，使重复数聚集，再跳过选择重复数就行了

### 实现代码

~~~java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> ans=new ArrayList<>();
        List<Integer> subset=new ArrayList<>();
        ans.add(new ArrayList<>(subset));
        Arrays.sort(nums);
        backtracking(nums,0,ans,subset);
        return ans;
    }

    public void backtracking(int[] nums,int start,List<List<Integer>> ans,List<Integer> subset) {
        if(start==nums.length)
            return;
        for(int i=start;i<nums.length;i++) {
            // 遇到重复数跳过
            if(i-1>=start&&nums[i]==nums[i-1])
                continue;
            subset.add(nums[i]);
            ans.add(new ArrayList<>(subset));
            backtracking(nums,i+1,ans,subset);
            subset.remove(subset.size()-1);
        }
    }

}
~~~

