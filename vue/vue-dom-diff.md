# vue-dom

## 一、Mount 挂载

1.  根据虚拟节点创建新的节点，`createDOMElementFromVnode()`

    无非就是 vnode 上的东西挂出来 type,props,text,children
    通过方法

    (1) document.creatElement(vnode.type)

    (2) document.createTextNode(vnode.text)

    (3) 更新属性烦一点， `updateProperties()` ，domElement.style[oldAttrName]

    (4) children 递归

2.  把真实的 dom 节点挂载到 container 上

        container.appendChild(newDomElement)

3.  vue 的虚拟节点就是`h()`方法，返回的一个有`type` `props` `key` `children`属性的对象

## 二、patch 更新 DOM

三部分

- type
- text
- children

1. 如果 type 不一样，直接新节点替换老节点
2. 只要 text 是 undefined ,就说么它是一个光的节点
3. 比较儿子前，老的虚拟节点、新的虚拟节点给 domElement,然后要更新老的真实 dom 的属性
   - 老的有儿子，新的没有儿子，把新儿子直接更新到老的上面
   - 新的有儿子，老的没有儿子，把老的儿子直接删了
   - 新旧都儿子，怎么不计较呢？双指针
     - 头和头
     - 尾和尾
     - 头和尾
     - 尾和头
     - 老的完了，新的还没
     - 新的完了，老的还没
