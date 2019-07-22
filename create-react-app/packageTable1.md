# 封装`Table`（二）

## 前言

上次讲到在 antd 的基础上，封装`Table`的 radio 和 checkbox，今天就讲用 Modal 实现增删改查。

## Modal 里的 Form

```js
<Modal
  title={this.state.title} //获得该状态的title
  visible={this.state.isVisible} //获得是否显示
  onOk={() => {
    this.handleSubmit() //提交信息
    // 重点！！！
    this.RouteForm.props.form.resetFields() //antd 的this.props.form.resetFields
    this.setState({
      isVisible: false,
      routeinfo: ''
    })
  }} //里面是一个方法
  onCancel={() => {
    this.RouteForm.props.form.resetFields()
    this.setState({
      isVisible: false,
      routeinfo: ''
    })
  }}
  width={600}
  {...footer} // 为什么要...
>
  <RouteForm
    routeinfo={this.state.routeinfo}
    type={this.state.type}
    // 重点！！！
    wrappedComponentRef={inst => (this.RouteForm = inst)}
  />
</Modal>
```

## handleSubmit && handleOprate && 只能选一个

1. 传入 type

```js
<Button type="add" onClick={() => this.handleOperate('add')}>
  增加
</Button>
// 其余类似
```

2. handleOprate,拿到 item=>该渲染的数据,拿到 selectedRowKeys=>该提交的 id,让弹框出现

```js
handleOperate = type => {
  let { selectedRowKeys } = this.state
  let item = this.state.selectedItem //这样就有了选中的所有信息
  if (type == 'delete') {
    Modal.confirm({
      title: '确定要删除此用户吗？',
      onOk: () => {
        axios
          .ajaxpost({
            url: '/routeinfo/delete',
            data: {
              params: { id: 3 }
            }
          })
          .then(res => {
            const { msg } = res
            message.success('删除成功！')
          })
      }
    })
  } else if (type == 'update' || type == 'detail') {
    // 重点！！！
    if (this.state.selectedRowKeys.length > 1) {
      Modal.info({
        title: '提示',
        content: '请选择一个。。。'
      })
    } else {
      this.setState({
        title: type == 'edit' ? '编辑信息' : '查看详情',
        isVisible: true,
        routeinfo: item,
        selectedRowKeys,
        type
      })
    }
  } else if (type == 'add') {
    this.setState({
      title: '创建路线',
      isVisible: true,
      routeinfo: '',
      type
    })
  }
}
```

3. 拿到弹框内的`Form`提交信息

```js
handleSubmit = () => {
  let type = this.state.type
  // 重点！！！
  let data = this.RouteForm.props.form.getFieldsValue()
  const { selectedRowKeys } = this.state
  console.log(data)

  if (type == 'add') {
    axios
      .ajaxpost({
        url: '/routeinfo/add',
        data: { data }
      })
      .then(res => {
        const { msg } = res
        console.log(data)
        console.log(msg)
        message.success('添加成功！')
      })
  } else {
    axios
      .ajaxpost({
        url: '/routeinfo/update',
        data: { data, id: selectedRowKeys }
      })
      .then(res => {
        const { msg } = res
        console.log(data)
        console.log(msg)
        message.success('修改成功！')
      })
  }
}
```

## 总结

1. wrappedComponentRef

```js
 wrappedComponentRef={inst => (this.RouteForm = inst)}
...
...
this.RouteForm.props.form.getFieldValue()
this.RouteForm.props.form.resetFields()
```

2. `{( ) => this.handleOperate('add')}`
3. 我本来是`this.RouteForm.props.form.handleSubmit()`,想把 handleSubmit 放在 Form 里的，这样就可以把**提交**放在要提交的 Form 里了。**Form**在另外的文件。但是因为`const selectedRowKeys = this.props.selectedRowKeys`拿不到 selectedRowKeys。。。。。。=>props 可能不能添加，再去看文档。。。。。。
