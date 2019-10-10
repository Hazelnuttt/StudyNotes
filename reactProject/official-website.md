---
title: 项目总结
categories: 官网
data: 2019-10-7
---

# axios

## 原理

[axios 原理](https://blog.csdn.net/mmjinglin/article/details/82761126)

## 用法

- [使用固定的封装好的方法](https://github.com/axios/axios/blob/master/README.md#request-method-aliases) get/post/delete/put...
- [使用自定义配置创建一个 axios 实例](https://github.com/axios/axios/blob/master/README.md#creating-an-instance)
- [使用拦截器（err/loading）](https://github.com/axios/axios/blob/master/README.md#interceptors)

```js
class Axios {
  private axiosInstance: AxiosInstance;

  constructor(props: any = {}) {
    this.axiosInstance = axios.create({
      baseURL: props.baseURL || BASE,
      timeout: props.timeout || 5000,
      headers: {
        token: props.token || localStorage.getItem('user_token') || ''
      }
    });

    // 拦截器
    this.axiosInstance.interceptors.response.use(
      response => {
        return response;
      },
      error => {
        message.error('网络请求异常！');
        console.log(error);
        return Promise.reject(error);
      }
    );
  }

  // 更新 header 中的 token
  public updateToken = (token: string) => {
    this.axiosInstance.defaults.headers.common['token'] = token;
  };

  public get = (url: string, config?: AxiosRequestConfig): Promise<any> => {
   ...
  };

  public post =...

  public put =...

  public delete =...

const axiosInstance = new Axios();

export default axiosInstance;
export const get = axiosInstance.get;
export const post = axiosInstance.post;
export const put = axiosInstance.put;
export const del = axiosInstance.delete;
```

## 优点

该封装包括了：1. 更新 header 中的 token 2. 拦截器 3. 指定的配置(this.axiosInstance)与可用的实例方法(get、post...)合并。
使得调用时代码极简，开发时专注于处理 res.data，逻辑清晰。

# react

## 原理

它创造了虚拟 dom 并且将它们储存起来，每当状态发生变化的时候就会创造新的虚拟节点和以前的进行对比，让变化的部分进行渲染。

## 知识点

### react 的 diff 算法

1. 组件更新的时候，react 会创建一个新的虚拟 dom 树，和之前的存储的 dom 树进行对比，这个比较的过程用到了 diff 算法。（初始化用不到）
2. 它假设：相同的节点具有类似的结构，而不同的节点具有不同的结构。
3. 列表的 diff 算法：列表具有相同的结构，对列表节点进行增删改查的时候，单个节点的整体操作远比一个个对比/替换好，所以创建列表的时候需要设置 key 值。
   ![diff 算法](https://github.com/Hazelnuttt/StudyNotes/blob/master/reactProject/docs/diff.JPG)

### React 生命周期

- ![lifetime](https://github.com/Hazelnuttt/StudyNotes/blob/master/reactProject/docs/lifetime.png)

### shouldComponentUpdate && PureComponent && immutable.js

- `shouldComponetUpdate`在首次渲染或`forceUpdate()`不会调用该方法
- 当`props`或`state`发生变化时，`shoudComponetUpdate()`会在渲染之前调用，默认返回 true
  ![Newlifetime](https://github.com/Hazelnuttt/StudyNotes/blob/master/reactProject/docs/Newlifetime.JPG)
- `React.PureComponent`以浅层对比(是否是对象，是否不为空，属性数量是否相等，属性的值是否一样，不比较实际的值)`props`和`state`的方式来实现函数
- `PureComponent`里用到了`shouldComponetUpdate()`和浅层对比
- `immutable.js`是持久性数据库，它的数据一旦创建，不会再变化，经过操作的数据永远会返回一个新的对象。
- `immutable.js`原理`Persistent Data Structure`(持久化数据结构)+`Structural Sharing`(结构共享)，使用旧数据避免`Deep Copy`,如果对象树中一个 root 改变，只修改这个 root、和它影响的父节点，其他节点进行共享
  ![Newlifetime](https://github.com/Hazelnuttt/StudyNotes/blob/master/reactProject/docs/Immutable.JPG)

### 为什么 setState 不设计成同步的（这个问题应该和 redux 异步 有共通性吧？）

1. 保持内部一致性和状态的安全。（不是很明白）
2. 性能优化

   - 异步获取数据后，统一渲染页面
   - react 会依据不同的调用源，给不同的 setState 调用分配不同的优先级（调用源：事件处理、网络请求、动画）

### 父子组件通信

[父子通信](http://hazelnuttt.com/2019/07/15/parent-child/#more)

### 父组件调用子组件的方法

- Refs 提供了一种方式，允许我们访问 DOM 节点或在 render 方法中创建的 React 元素。
- 在典型的 React 数据流中，props 是父组件与子组件交互的唯一方式。要修改一个子组件，你需要使用新的 props 来重新渲染它。但是，在某些情况下，你需要在典型数据流之外强制修改子组件。
- Example:

```js
class Editor extends React.PureComponent {
  state = {}
  componentDidMount() {
    this.refs.modify.fetchProfile1() // MyEditor 里的方法
  }

  render() {
    return (
      <>
        <MyEditor ref="modify" />
        <Button type="primary" size="large" onClick={() => this.refs.modify.handleModify1()}>
          修改
        </Button>
      </>
    )
  }
}

export default Editor
```

# react-router

## 用法

- router 分割(App|Admin)

```js
renderAdminRoute = () => {
      return <AdminRouter />;
  };

  render() {
    return (
        <Router>
          <Switch>
            <Route
              path="/admin"
              render={props => this.renderAdminRoute()}
            ></Route>
            <Route path="/" render={props => <AppRouter />}></Route>
          </Switch>
        </Router>
    );
  }
}
```

- WithRouter
  将 react-router 的 history、location、match 三个对象传入 props 对象上
  可用来获取路由上的一些信息

```js
this.props.match.params.id
```

## 优点

- 重构路由，把路由按照一定逻辑或作用分割，代码清晰

# redux

## 知识点

### redux 数据的生命周期

1. 调用 `store.dispatch(action)`
   > 触发 action
2. Redux Store 调用 reducer 函数
   > store 会把两个参数传入 reducer ：当前的 state 和 action
3. 根 reducer 把多个 子 reducer 输出合并成一个单一的 state 树
   > 是否有多个 reducer，视情况。所有 state 都以一个对象树的形式合并存储是一定的。
4. Redux stone 保存了根 reducer 返回的完整的 state 树

### 处理 Action

在为 action 开发一些 reducer 的时候，有一个 reducer 组合 的概念。比如：一个网站的首页内容的 fetch,通过 combineReducer() 把 子 reducer 合并成一个大的 reducer；一个网站所有的某些特定的内容的 post,同上，处理成一个 reducer。

### 同步 && 异步

#### 同步

在基础教程中，是同步操作。每当 dispatch action 时，state 会被立即更新

#### 异步

- 如何把 同步 acion 和 网络请求结合 => redux-thunk（等一系列 middleware）
  - 核心：middleware --> 创建函数返回 --> 1.action 对象 2.函数
  - action 创建函数 返回函数时，这个函数被 redux-thunk 执行。它不需要保持纯净，它可以包括：1.异步 API 请求 2.dispatch acion
- connect 是异步的

## 用法

文件结构

```
|- modes
  |-- BannerMode
  |-- LoginMode
  |-- ProductMode
  |-- ProfileMode
```

Example:

```js
export default class BannerMode extends BaseMode {
  fetchBannerList = (): Promise<IBanner[]> => {
    return get('/rotation_charts').then((res: Resp<IBanner[]>) => {
      this.updateBannerListState(res.data);
      console.log(res.data);
      return res.data;
    });
  };

  // action
  updateBannerListState = (banners: IBanner[]) => {
    this.dispatch({
      type: UPDATE_BANNERS,
      payload: banners
    } as IActionType<IBanner[]>);
  };
}

interface IBannerListState {
  bannerList: IBanner[];
}

const INIT_STATE: IBannerListState = {
  bannerList: []
};

// 子reducer
export const BannerReducer = (
  state: IBannerListState = INIT_STATE,
  action: IActionType
) => {
  switch (action.type) {
    case UPDATE_BANNERS:
      return {
        ...state,
        bannerList: action.payload
      };
    default:
      return state;
  }
};
```

```js
//使用 combineReducers
export default function createReducer(injectedReducers = {}) {
  return combineReducers({
    ...injectedReducers,
    profile: ProfileReducer,
    login: LoginReducer,
    banner: BannerReducer,
    product: ProductReducer
  })
}
```

## 优点

BannerModes 等于一个容器组件，一个逻辑对应一个 reducer 组合,BannerModes 把属于自己的 action、reducer 集中，最后合并。modes 文件夹下逻辑清晰，对应到开发工具 composeWithDevTools，便于测试。

# TypeScript

## 知识点

### 类与类

1. 类继承（只能一个）
2. 类多继承（Mixins）

### 接口与类

1. 类继承接口（多）
2. 接口继承类（同上 类与类）

### 接口与接口

1. 多

# 总结

1. 一个项目的封装、重构是不可能少的

- 封装

  - axios
  - login

- 重构

  - 组件提取
  - redux/mode
  - router [App/Admin]
  - menuConfig

2. redux 写得好真的有用，composeWithDevTools 上面看的很舒服，很清楚

# QS

- 如果 fetch、put、等一些与 state 有关的一些方法 全部放到 mode 里面，意味着 state 都要挂到 state 树上，那就没有`this.setState()`？但是一些 isVisible 这些小 state 也要挂吗？
- redux-chunk 什么时候要用
- 下面这个写法好吗？

```js
// 映射Redux全局的state到组件的props上
const mapStateToProps = (state) => ({

});
// 映射dispatch到props上
const mapDispatchToProps = (dispatch) => {
  return {
    togglePlayListDispatch(data) {
      dispatch(changeShowPlayList(data));
    },
    changeCurrentIndexDispatch(data) {
      dispatch(changeCurrentIndex(data));
    },
    changeModeDispatch(data) {
      dispatch(changePlayMode(data));
    },
    changePlayListDispatch(data) {
      dispatch(changePlayList(data));
    }
  }
```
