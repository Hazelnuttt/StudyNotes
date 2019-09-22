# Day3

### 报数序列是一个整数序列，按照其中的整数的顺序进行报数，得到下一个数。其前五项如下：

1.      1
2.      11
3.      21
4.      1211
5.      111221

### code

```js
var countAndSay = function(n) {
  if (n === 1) return '1'
  var str = countAndSay(n - 1)
  var count = 0
  var ans = ''
  var temp = str[0]
  for (var i = 0; i < str.length; i++) {
    if (str[i] === temp) {
      count++
    } else {
      ans += '' + count + temp
      temp = str[i]
      count = 1
    }
    if (i === str.length - 1) {
      ans += '' + count + temp
    }
  }
  return ans
}
```
