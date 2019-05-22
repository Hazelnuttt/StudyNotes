# Table

## 功能

增删改查

## 用`record`指明某条数据

它主要是针对一条数据进行操作

**我刚刚发现我一开始的写法不知道在干嘛。。**

#### 如何使 react 的索引 key --> 指向数据库的 id

```javascript
//在requireList()里
this.setState({
  list: res.list.map((item, index) => {
    item.key = item.index
    return item
  })
})
//这个index是索引的意思，但是对我传数据一点没用。今天看到说，key是方便react修改虚拟dom的，可以提高性能
//这个item 就是一条条数据
```

```javascript
//在requireList()里
this.setState({
  list: res.list.map(item => {
    item.key = item.id
    return item
  })
})
//这个item 就是一条条数据
```

**之后貌似不用一直依赖于这个 record 了==>`item.key`就可以对数据操作了**

## 用`selectedRows`和`selectedKeys`指明多条数据

它主要针对多条数据的操作，会涉及到 radio/checkbox 的样式，如果不是批量编辑，就需要限制 checkbox 打钩的个数

### 必须知道的东西

**`key`&`selectedRowkeys`&`rowKey`&`selectedRows`**

- ![key](https://github.com/Hazelnuttt/StudyNotes/blob/master/create-react-app/docs/key.png)
- ![selectedRowkeys](https://github.com/Hazelnuttt/StudyNotes/blob/master/create-react-app/docs/selectedRowkeys.png)
- ![rowKey](https://github.com/Hazelnuttt/StudyNotes/blob/master/create-react-app/docs/rowKey.png)

#### `key`和`rowKey`是一个东西

#### `selectedRowkeys`是选中的这条数据的 key 值

#### `selectedRows`是选中的这条数据的所有字段的值（就是这条数据）

#### 选择数据的有单选框和多选框两种

```javascript
onSelectChange = (selectedRowkeys, selectedRows) => {
  this.setState({
    selectedRowkeys,
    selectedItem: selectedRows[0] //注意：这里本人走了很长时间的坑，按照我的理解是：selectedRows 会有好多条数据，只能是第一条，数据才能被后续代入
  })
}
const rowSelection = {
  type: 'radio', //单选框，可选择多选框
  selectedRowKeys,
  onChange: this.onSelectChange
}
<Table rowSelection={rowSelection} xxx />
```

#### 点睛之笔

```javascript
handleOperator = type => {
    const { selectedItem, selectedRowKeys } = this.state
    let item = this.state.selectedItem
    console.log(selectedItem)
    console.log(selectedRowKeys)

    ····
    ····

    ····
     else if (type == 'edit' || type == 'detail') {
      this.setState({
        title: type == 'edit' ? '编辑用户' : '查看详情',
        isVisible: true,
        terinfo: item,
        type
      })
```

## 翻页

**这里主要是一个分页，向后端获取第 2..3..的数据**
**注意点：这个 `onChange` 方法可以向 `requirelist` 一样变成一个通用的获取资源的方法，主要用在 `edit`/`delete`，因为删除或编辑的时候，需要仍然停留在该页，获取*该页*的数据**

```javascript
onChange = pageNumber => {
    this.setState({
      pageNumber,
      loading: true
    })
    fetch(`http://198.13.50.147:8099/api/endpoint?page=${pageNumber}`, {
      ·····
    }
    ·····

<Pagination
  className="pagination"
  defaultCurrent={1}
  total={this.state.total}
  onChange={this.onChange}
/>
```
