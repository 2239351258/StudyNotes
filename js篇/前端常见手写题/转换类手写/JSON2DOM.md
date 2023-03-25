### 要求

实现类似react的render函数

```json
{
  tag: 'DIV',
  attrs: {
    id: 'app'
  },
  children: [
    {
      tag: 'SPAN',
      children: [
        { tag: 'A', children: ['111'] }
      ]
    },
    {
      tag: 'SPAN',
      children: [
        { tag: 'A', children: [222] },
        { tag: 'A', children: ['333'] }
      ]
    }
  ]
}
```

将上述虚拟dom转化为真实dom

```html
<div id="app">
  <span>
    <a>111</a>
  </span>
  <span>
    <a>222</a>
    <a>333</a>
  </span>
</div>
```

### 实现

```js
function _render(vnode) {
  // 如果是数字类型的转化为字符串
  if (typeof vnode === 'number') {
    vnode=String(vnode)
  }
  // 字符串类型就直接创建文本节点
  if (typeof vnode === 'string') {
    return document.createTextNode(vnode)
  }
  // 普通DOM
  const dom = document.createElement(vnode.tag)
  if (vnode.attrs) {
    // 若有属性，则遍历添加属性
    for (let [key, val] of Object.entries(vnode.attrs)) {
      dom.setAttribute(key,val)
    }
  }
  // 递归子数组
  vnode.children.forEach((child) => dom.appendChild(_render(child)))
  return dom
}
// test
!(() => {
  let vnode = {
    tag: 'DIV',
    attrs: {
      id: 'app'
    },
    children: [
      {
        tag: 'SPAN',
        children: [
          { tag: 'A', children: ['111'] }
        ]
      },
      {
        tag: 'SPAN',
        children: [
          { tag: 'A', children: [222] },
          { tag: 'A', children: ['333'] }
        ]
      }
    ]
  }
  document.getElementsByTagName('body')[0].appendChild(_render(vnode))
})()
```

