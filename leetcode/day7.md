# Day7

## 20.有效的括号

> 给定一个只包括 '('，')'，'{'，'}'，'['，']'  的字符串，判断字符串是否有效。

有效字符串需满足：

示例 1:

输入: "()"
输出: true
示例  2:

输入: "([)]"
输出: false

### 借鉴代码

```js
var isValid = function(s) {
  var map = {
    '{': '}',
    '[': ']',
    '(': ')'
  }
  var leftArr = []
  for (var ch of s) {
    if (ch in map) {
      leftArr.push(ch)
    } else {
      if (ch != map[leftArr.pop()]) return false
    }
  }
  return !leftArr.length //这一步很nb，希望这个数组为0，返回true
}
```
