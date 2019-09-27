# Day4

## 26.删除排序数组中的重复项

> 给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

示例  1:

给定数组 nums = [1,1,2],

函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。

你不需要考虑数组中超出新长度后面的元素。

### My code

```js
var removeDuplicates = function(nums) {
  var len = nums.length
  var temp = nums[0]
  var count = 0
  var j = 0
  for (i = 0; i < len - 1; i++) {
    if (nums[i] == nums[i + 1]) {
      count++
    } else {
      j = j + 1
      nums[j] = nums[i + 1]
    }
  }
  len = len - count
  return len
}
```

### Review

- 解 1：

```js
var removeDuplicates = function(nums) {
  var i = 1
  while (i < nums.length) {
    if (nums[i] == nums[i - 1]) {
      nums.splice(i, 1)
    } else {
      i++
    }
  }
  return nums.length
}
```

- 解 2：（mycode improve)(优解)

```js
var removeDuplicates = function(nums) {
  var len = 1
  for (i = 1; i < nums.length; i++) {
    if (nums[i] != nums[i - 1]) nums[len++] = nums[i]
  }
  return len
}
```

- 解 3：（鉴于双指针）

```js
var removeDuplicates = function(nums) {
  var j = 0
  for (i = 0; i < nums.length; i++) {
    if (nums[i] != nums[j]) {
      j++
      nums[j] = nums[i]
    }
  }
  return j + 1
}
```
