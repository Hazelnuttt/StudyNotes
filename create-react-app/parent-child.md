---
title: 父子通信
date: 2019-7-15
tags: react
categories:
  - 笔记
---

# 前言

记忆力你怎么这么秀？复习复习复习 @Hazelnut

##

1. class&function
   class 里必须是`this.props.name`
   ![1](https://github.com/Hazelnuttt/StudyNotes/blob/master/create-react-app/docs/1.png)
   ![2](https://github.com/Hazelnuttt/StudyNotes/blob/master/create-react-app/docs/2.png)
   function component 里是`props.name`
   ![3](https://github.com/Hazelnuttt/StudyNotes/blob/master/create-react-app/docs/3.png)
2. 父组件向子组件传参，子组件拿到父组件的 state,子组件里 props 渲染出来

**这时候父组件里**

```js
constructor(props){
super(props)
}
```

**可有可无，都没什么关系。**
![4](https://github.com/Hazelnuttt/StudyNotes/blob/master/create-react-app/docs/4.png)

3. 子组件传参数给父组件——标题看似是子传父，其实是父传子。
   子组件要想把改变的参数传给父组件，其实是“状态提升”，在父组件里写 state,子组件里拿 props, 其实就等同于父传子。**唯一特别的就是，往往把“本来在子组件改变状态的方法”移到父组件去，这样父组件，子组件都能通过这个方法拿到 state。**
   ![6](https://github.com/Hazelnuttt/StudyNotes/blob/master/create-react-app/docs/6.png)
   ![5](https://github.com/Hazelnuttt/StudyNotes/blob/master/create-react-app/docs/5.png)
