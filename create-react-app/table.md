# Table

## 功能

增删改查

## fetch

1. 获取该页面的数据--->`requireList`
2. 如何写`fetch`

```javascript
this.setState({ loading: true })
fetch('http://localhost:8099', {
  method: 'GOT',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JOSN.Stringfy({
    ...data
  })
})
  .then(res => json())
  .then(res => {
    const { msg } = res
    if (msg === 'success') {
      console.log(msg)
      message.success('登录成功！')
    } else {
      console.log(msg)
      message.error('登录失败！')
    }
  })
  .catch(err => {
    console.log(err)
    message.error('网络请求异常！')
  })
  .finally(() => {
    this.setState({ loading: fales })
  })
```

3. 教你解决一个坑

- 后端是不是把一些少量的信息放在 headers 里传了？-哟
- 你是不是获取不到？-哟
- 你是不是还打印过 res,空的，这么拿得到 header 呢？-哟哟哟
  **哈哈哈哈，恭喜你，踩到狗屎运了，俺来教你**
  **那啥，可能是跨域问题，打印的 res 会显示`cros`**

```javascript
...
...
.then(res=>json())
.then(res=>{
    const token = res.headers.get('token')
    console.log(token)
    //那啥，我警告你不要去打印res,之后你啥也拿不到，可能是后端的安全的问题。。
})
```
