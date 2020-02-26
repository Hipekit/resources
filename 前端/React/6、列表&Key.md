### 列表

定义列表

```jsx
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

* `numbers`定义了一个数组
* `number.map`转换数据格式
* key用于给列表元素唯一标识
* `<ul>{listItems}</ul>`渲染列表



### Key

key 帮助 React 识别哪些元素改变了，比如被添加或删除。因此你应当给数组中的每一个元素赋予一个确定的标识。

```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```

* 一个元素的 key 最好是这个元素在列表中拥有的一个独一无二的字符串。
  - 如果列表项目的顺序可能会变化，我们不建议使用索引来用作 key 值，因为这样做会导致性能变差，还可能引起组件状态的问题。
* 如果你选择不指定显式的 key 值，那么 React 将默认使用索引用作为列表项目的 key 值。

* key只是兄弟节点之间必须唯一