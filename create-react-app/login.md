# 多思考，少做简单的体力活

## CSS 布局

```css
.login-card {
  min-height: 100vh;
  width: 100%;
  display: flex;
  /* 长宽都占满    flex布局 */
  /* align-items: center; //上下 */
  /* justify-conrent: center; //左右 */
  /* 垂直居中 */
  background-image: url(../../img/bg.JPG);
  background-size: 100% 100%;
  /* 宽、高 */
  background-repeat: no-repeat;
  /* 不重叠 */
}
```

## axios

- `fetch` 要自己设置`header:Content-Type:application/json`
- `axios` 默认`json`

## 永远的迷/坑（无奈）

```js
 axios
          .post(LOGIN_URL, values)
          // .then(res => res.json())
          .then(response => {
              ...
```

- `axios` 的`res`的`body`自己套了一个 data？

* `let res = response.data`

## 细节成就你

```js
if (response.status == 200) {
              if (res.code == 0) {
                  ...
```

加上`status`和`code`
