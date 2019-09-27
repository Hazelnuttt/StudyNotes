# Day6

## 14.最长公共前缀

> 编写一个函数来查找字符串数组中的最长公共前缀。如果不存在公共前缀，返回空字符串 ""。示例  1:

输入: ["flower","flow","flight"]
输出: "fl"
示例  2:

输入: ["dog","racecar","car"]
输出: ""

### My code

```js
var longestCommonPrefix = function(strs) {
  var temp = 0
  if (!strs.length) return ''
  if (strs.length == 1) return strs[0]
  //位数
  while (temp < strs[0].length) {
    //个数
    for (j = 1; j < strs.length; j++) {
      if (strs[j].slice(0, temp + 1) != strs[j - 1].slice(0, temp + 1))
        return strs[0].slice(0, temp)
    }
    temp++
  }
  return strs[0].slice(0, temp)
}
```

### Review

```js
var longestCommonPrefix = function(strs) {
  var re = ''
  if (!strs.length) return re
  for (j = 0; j < strs[0].length; j++) {
    for (i = 1; i < strs.length; i++) {
      if (strs[i][j] != strs[0][j]) return re
    }
    re += strs[0][j]
  }
  return re
}
```

#### mycode improve

> 提高了 16ms

```js
var longestCommonPrefix = function(strs) {
  var temp = 0
  var re = ''
  if (!strs.length) return re
  if (strs.length == 1) return strs[0]
  //位数
  while (temp < strs[0].length) {
    //个数
    for (j = 1; j < strs.length; j++) {
      if (strs[j].slice(0, temp + 1) != strs[0].slice(0, temp + 1)) return re
    }
    temp++
    re = strs[0].slice(0, temp)
  }
  return re
}
```
