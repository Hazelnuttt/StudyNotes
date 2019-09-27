# Day5

### 给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素最多出现两次，返回移除后数组的新长度。不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

示例  1:

给定 nums = [1,1,1,2,2,3],

函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3 。

你不需要考虑数组中超出新长度后面的元素。

### code

#### 先说说我的 code

类似于我 day4 写的

> 已删

#### 借鉴的代码

```JS
var removeDuplicates = function (nums) {
  let i = 0
  while (i < nums.length) {
    if (nums[i] === nums[i - 2]) {
      nums.splice(i, 1)
    } else {
      i++
    }
  }
  return nums
};
```

### 语法

```js
var s = ['a', 'b', 'c']
console.log(s.splice(2, 1))
console.log(s) //打印['a','b']
```

### Review

鉴于我提交失败的代码太长，已经删除，review 的时候我都没耐心看。。。
