# Day2

### 给定一个  haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从 0 开始)。如果不存在，则返回   -1。

### code

```js
var strStr = function(haystack, needle) {
  const len = needle.length
  const length = haystack.length
  for (i = 0; i <= length - len + 2; i++) {
    if (haystack.slice(i, i + len) == needle) {
      return i
    }
    if (i > length - len) return -1
  }
}
```
