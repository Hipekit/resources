```jsx
const element = <h1>hello,world!</h1>;
```

它被称为 JSX，是一个 JavaScript 的语法扩展。JSX不是React必需的，它只是React.createElement的语法糖。



### JSX标签类型

在JSX语法中，有两种标签类型：DOM类型的标签和React组件类型的标签。DOM类型的标签要以小写字母开头，React 组件类型的标签要以大写字母开头。JSX正是通过首字母的大小写来区分标签的类型。

```jsx
//DOM类型
const element = <h1>Hello,world</h1>;
      
//React组件类型
const element = <HelloWorld />;

//两者可以互相嵌套
const element = (
	<div>
  	<HelloWorld />
  </div>
);
```



### 在JSX中嵌入表达式

```jsx
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
	element,
    document.getElementById('root')
);
```

在 JSX 语法中，你可以在大括号内放置任何有效的 JavaScript表达式。例如，`2 + 2`，`user.firstName` 或 `formatName(user)` 都是有效的 JavaScript 表达式。

**注意：表达式只能是JavaScript表达式，不能是多行JavaScript语句。**



### JSX本身也是一个表达式

在编译之后，JSX 表达式会被转为普通 JavaScript 函数调用，并且对其取值后得到 JavaScript 对象。

也就是说，你可以在 `if` 语句和 `for` 循环的代码块中使用 JSX，将 JSX 赋值给变量，把 JSX 当作参数传入，以及从函数中返回 JSX：

```jsx
funcation getGreeting(user){
    if(user){
        return <h1>Hello, {formatName(user)}</h1>;
    }
    return <h1>Hello,Stranger.</h1>;
}
```



### JSX定义属性

DOM标签的部分属性名需要有所改变，class改成className，事件属姓名采用驼峰命名法。

```jsx
<div id="content" className='foo' onClick={() => console.log('Hello,world')} />
```

可以通过使用引号，来将属性值指定为字符串字面量：

```jsx
const element = <div tabIndex="0"></div>;
```

也可以使用大括号，来在属性值中插入一个 JavaScript 表达式：

```jsx
const element = <div tabIndex={user.userName}></div>
```



### 使用JSX创建子元素

没有子元素，使用`/>`直接闭合

```jsx
const element = <img src={user.avatarUrl} />;
```

创建子元素，使用`()`包起来

```jsx
const element = (
	<div>
    	<h1>Hello!</h1>
        <h2>Good to see you here.</h2>
    </div>
);
```



### JSX定义对象

以下两种方式完全等同：

* 直接创建

```jsx
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

* 通过`React.createElement`创建

```jsx
const element = React.createElement(
	'h1',
    {className : 'greeting'},
    'Hello, world!'
);
```



### 元素渲染

元素定义之后，可以通过`ReactDOM.render()`方法进行渲染

```jsx
const element = <h1>Hello, world</h1>;
ReactDOM.render(elementj, document.getElementById('root'));
```

