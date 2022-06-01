---
title: 递归与回溯
date: 2022-05-26
---

## 39题：组合总和

> 解题思路：回溯算法，实现回溯算法有三大关键点
1. 一条路走到底，直到满足条件或者不符合条件
2. 回退一步
3. 另寻它路
> 关键点实现
1. for循环：作用在于另寻它路，可以利用它实现一个路径选择器，配合递归可以把所有路径都尝试一遍
2. 递归：可以实现一条走到底与回退一步，当把递归放到for循环内部时即可以把所有的路径都给走一遍，当递归从递归出口出来时，即实现了回退一步

```js
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
let combinationSum = function (candidates, target) {
  // 定义一个数组存储满足条件的值
  let res = []

  // 把它想象成一个树形图，进行深度优先遍历，遍历它的每一条枝杈，存储其枝杈节点
  let helper = (start, prevSum, prevArr) => {
    // 由于全是正整数 所以一旦和大于目标值了 直接结束本次递归即可。
    if (prevSum > target) {
      return
    }
    // 目标值达成
    if (prevSum === target) {
      res.push(prevArr)
      return
    }
    // 遍历所有树分叉，i = start不等于0是因为要减枝，删除重复项
    for (let i = start; i < candidates.length; i++) {
      // 这里还是继续从start本身开始 因为多个重复值是允许的
      let cur = candidates[i]
      let sum = prevSum + cur
      let arr = prevArr.concat(cur)
      helper(i, sum, arr)
    }
  }

  helper(0, 0, [])

  return res
}
```


## 40题：组合总和||
```js
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum2 = function(candidates, target) {
  // 定义一个变量存储结果
  let res = []
  // 对数组进行排序
  candidates.sort((a,b) => a-b)

  // 还是一样，想象成一个树形图，进行深度优先遍历
  const help = (startIndex, prevSum, prevArr) => {
      // 当树分叉节点之和大于目标时，结束本次递归
      if (prevSum > target) {
          return
      }
      // 当树分叉节点之和等于目标时，存储分叉节点数组
      if (prevSum === target) {
          res.push(prevArr)
          return
      }
      // 遍历同级树节点
      for (let i = startIndex; i < candidates.length; i++) {
         // 剔除重复结果，同级树节点，如果相邻节点值相等，取首次递归结果即可，后续跳过
          if (i> startIndex && candidates[i] === candidates[i-1]) {
              continue
          }
          // 接收当前值
          let cur = candidates[i];
          // 接收节点之和
          let sum = prevSum + cur;
          // 接收节点数组
          let arr = prevArr.concat(cur)
          // 进行递归
          help(i+1, sum, arr)
      }

  }

  help(0,0,[])
  return res
};
```

## 70题：组合

```js
/**
 * @param {number} n
 * @param {number} k
 * @return {number[][]}
 */
let combine = function (n, k) {
  // 接收最终结果
  let ret = []

  let helper = (start, prev) => {
    let len = prev.length
    if (len === k) {
      ret.push(prev)
      return
    }

    if (start > n) {
      return
    }

    // 还有 rest 个位置待填补
    let rest = k - prev.length
    for (let i = start; i <= n; i++) {
      // 减枝，举例：n=4，k=3 时，如果列表从 start=3 也就是 [3] 开始，那么该组合一定不存在，因为至少要 k=3 个数据，所以剪枝临界点为 n-i+1

      if (n - i + 1 < rest) {
        continue
      }
      helper(i + 1, prev.concat(i))
    }
  }
  helper(1, [])
  return ret
}
```

## 78题：子集

```js
var subsets = function(nums) {
    let result = []//存放结果
    let path = []//存放一个分支的结果
    function backtracking(startIndex) {//startIndex字符递归开始的位置
        result.push(path.slice())//path.slice()断开和path的引用关系
        for(let i = startIndex; i < nums.length; i++) {//从startIndex开始递归
            path.push(nums[i])//当前字符推入path
            backtracking(i + 1)//startIndex向后移动一个位置 继续递归
            path.pop()//回溯状态
        }
    }
    backtracking(0)
    return result
};
```

## 79题：子集||

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsetsWithDup = function(nums) {
    let result = []//存放结果

    // 对数组进行排序
    nums.sort((a,b) => a-b)

    let path = []//存放一个分支的结果
    function backtracking(startIndex) {//startIndex字符递归开始的位置
        result.push(path.slice())//path.slice()断开和path的引用关系
        for(let i = startIndex; i < nums.length; i++) {//从startIndex开始递归
        
            // 剔除重复结果，同级树节点，如果相邻节点值相等，取首次递归结果即可，后续跳过
            if (i> startIndex && nums[i] === nums[i-1]) {
                continue
            }
            path.push(nums[i])//当前字符推入path
            backtracking(i + 1)//startIndex向后移动一个位置 继续递归
            path.pop()//回溯状态
        }
    }
    backtracking(0)
    return result
};
```