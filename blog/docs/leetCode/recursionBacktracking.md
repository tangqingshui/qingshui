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
