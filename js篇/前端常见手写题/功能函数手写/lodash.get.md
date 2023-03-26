`lodash.get`是一个JavaScript函数，用于从对象中获取嵌套的属性值。它接受三个参数：要获取值的对象、嵌套属性的路径以及可选的默认值。如果给定的路径无法找到属性，则返回默认值（如果提供）或undefined。

```js
function _get(obj, path, defaultValue = "undefined") {
  //先将path处理成统一格式
  let newPath = [];
  if (Array.isArray(path)) {
    newPath = path;
  }
  else {
    // 字符串类型 obj[a] obj.a  这里把'[' 替换成'.' ']' 替换成''
    newPath = path.replace(/\[/g, '.').replace(/\]/g, '').split('.');//最后转成数组
    console.log(newPath);
  }
  //obj 替换成 obj.a 逐步调用
  return newPath.reduce((o, k) => {
    return (o || {})[k];
  }, obj) || defaultValue
};
!(() => {
  var object = { 'a': [{ 'b': { 'c': 3 } }] };

  console.log(_get(object, 'a[0].b.c'));
  // => 3

  console.log(_get(object, ['a', '0', 'b', 'c']));;
  // => 3

  console.log(_get(object, 'a.b.c', 'default'));;
// => 'default'
})()
```

