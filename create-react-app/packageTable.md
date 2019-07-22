# 封装`Table`(一)

## 前言

多多记笔记，勤劳

## 什么是封装，为什么要封装

其实这都是我自己的理解，大家参考参考。首先，表单的增删改查非常重要，经常用到，但是代码其实我觉得挺乱的，不清晰。**为什么要封装？**=>代码太长了，放在一个文件里面，逻辑都看不清。**什么是封装**=>就比如这次的 Table 封装

```js
onSelectChange=(selectedRowKeys,selectedRows)=>{
    console.log(selectedRowKeys)
    console.log(selectedRows)
}
// 把这个函数抽到其他文件去
const rowSelection ={
    const {selectedRowKeys} = this.state
    onChange: this.onSelectChange
}
// 把这玩意抽到其他文件去，且能控制radio,还是checkbox,还有只能勾选一个还是多个（为后面增删改查做准备）
<Table  rowSelection={rowSelection} dataSource={this.state.list}
        columns={columns}/>
```

**即主要组件里就是有`<Table rowSelection={rowSelection} dataSource={this.state.list}columns={columns}/>`这一行代码了，贼清楚**

## 一个完整的`Table`组件必须具备的东西

1. loading
2. pageNum 　=>（这里有一个问题，按需加载）
3. isVisible(这就是一个弹框) => （有一个问题，不要用弹框添加，===> 或者说详情和修改一起在新组件进行，添加也在新组建进行）
4. 单选 Or 多选
5. `checkbox` Or `radio`

## 实现封装和`checkbox` Or `radio`

```js
// 取个好听的名字Etable
export default class ETable extends React.Component {
  state = {}

  // 选择框变更
  onSelectChange = (selectedRowKeys, selectedRows) => {
    console.log(selectedRowKeys)
    console.log(selectedRows)
    // 重点！！！
    this.props.updateSelectedItem(selectedRowKeys, selectedRows)
  }

  render() {
    const { selectedRowKeys } = this.props
    // 思想！！！
    const rowSelection = {
      type: 'radio',
      selectedRowKeys,
      onChange: this.onSelectChange
    }
    let row_selection = this.props.rowSelection
    // 当属性未false或者null时，说明没有单选或者复选列
    if (row_selection === false || row_selection === null) {
      row_selection = false
    } else if (row_selection == 'checkbox') {
      //设置类型未复选框
      rowSelection.type = 'checkbox'
    } else {
      //默认未单选
      row_selection = 'radio'
    }
    return (
      <Table
        className="card-wrap page-table"
        bordered
        // 重点！！！
        {...this.props}
        rowSelection={row_selection ? rowSelection : null}
      />
    )
  }
}
```

```js
return (
  <ETable
    //   重点！！！
    updateSelectedItem={Utils.updateSelectedItem.bind(this)}
    selectedRowKeys={selectedRowKeys}
    selectedRows={selectedRows}
    rowSelection={'checkbox'}
    dataSource={this.state.list}
    columns={columns}
    bordered={true}
    loading={this.state.loading}
  />
)
```

```js
// 调用组件外的函数
// 函数可以在任何环境下运行，要弄清楚它的运行环境（即对象）==> 知识点：this
export default {
  updateSelectedItem(selectedRowKeys, selectedRows) {
    this.setState({
      selectedRowKeys,
      selectedItem: selectedRows
    })
    console.log(selectedRowKeys)
    console.log(selectedRows)
  }
}
```

### 总结

1. 父子组件通信
2. `{...this.props}` 这算继承吗？ 我再看看文档昂。
3. 思想
4. `this`确实挺恶心的，烂代码
5. 组件调用外部函数，改变该组件的状态（这些函数一般是因为通用，才把它抽到外面）

## nb 玩意

```js
onChange: this.onSelectChange
onSelectChange = (selectedRowKeys, selectedRows) => {
  this.props.updateSelectedItem(selectedRowKeys, selectedRows)
}
```

**过渡到**

```js
onSelectChange = (b, selectedRowKeys) => {
  this.setState({ selectedRowKeys, b })
  console.log(selectedRowKeys)
  console.log(b)
}
```

## 最后

提醒自己：看文档很重要，加油！
