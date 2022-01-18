### 一、Hello React
1. 为什么用jsx不用js?
- 标签嵌套下，如h1嵌套span，js写虚拟dom时过于麻烦,jsx比较流畅，jsx经过babel编辑过后，生成的就是js代码。
``` javascript
// js创建虚拟dom
const VDOM = React.createElement("h1", { id: "title" }, React.createElement('span',{},'hello,React'));
```
``` javascript
// jsx创建虚拟dom
const VDOM = (
  <h1 id="title">
    <span>hello,React</span>
  </h1>
); /*此处不能写引号，因为不是字符串*/
```
2. 关于虚拟DOM
- 虚拟DOM本质是Object类型的对象（一般对象）
``` javascript
console.log(VDOM) // > object props、ref、type、id...
console.log(typeOf VDOM) // object
console.log(VDOM instanceof Object) // true
```
- 虚拟DOM挂载的API较少，真实DOM挂载的API较多（通过debugger断点可以看到),虚拟DOM是React内部在使用，无需真实DOM上那么多属性
``` javascript
// 真实DOM
const TDOM = document.getElementById("test");
debugger // 查看TDOM accessKey、align、baseURI等等几十个
console.log(TDOM); 
```
- 虚拟DOM最终会被React转化为真实DOM，渲染到页面上
### 二、JSX语法规则
- jsx全称javaScript XML（XML早期用于存储和传输数据（像微信服务器和开发者服务器之间还是用XML，后来有了json方式存储）
``` javascript
// XML
<student>
    <name>张三</name>
    <age>18</age>
</student>
```
- react定义的一种类似于XML的js扩展语法：js+XML
``` javascript
<script type="text/babel">
  const id = "vdom";
  const data = "hello,React";
  /**
   * jsx语法规则：
   * 1.定义虚拟DOM时，不要写引号，因为不是写字符串
   * 2.标签中混入js表达式时，要使用花括号{}
   * 3.样式的类名指定时，不要用class，用className
   * 4.内联样式需要写双花括号{{}}，外层花括号代表里面要写js表达式，里层花括号代表写的是一个对象key:value,类似于font-size属性，需要驼峰命名fontSize
   * 5.jsx（虚拟DOM)不能有多个根标签，只能有一个，外层最好用div包裹，标签必须闭合
   * 6.标签首字母，（1.如果小写字母开头，jsx最终编译成html，会找同名元素，找不到就报错。（2.大写字母开头，react就会渲染对应组件，组件未定义情况下，就报错。
  */
  const VDOM = (
    <h1 id={id}>
      <span style={{color:'white'}}>{data}</span>
    </h1>
    // <input type="text"> x 只能有一个根标签
  );
  //2. 渲染虚拟dom到页面
  ReactDOM.render(VDOM, document.getElementById("test"));
</script>
```
只能有一个根标签 √
``` javascript
<script type="text/babel">
  const id = "vdom";
  const data = "hello,React";
  const VDOM = (
    <div>
      <h1 id={id}>
        <span style={{color:'white'}}>{data}</span>
      </h1>
      <input type="text" />
    </div>
  );
  ReactDOM.render(VDOM, document.getElementById("test"));
</script>
```

- 本质是React.createElement(component,props,...children)方法的语法糖
- 作用：用来简化创建虚拟DOM
- 标签名任意：HTML标签或其他标签
react在new Weather时，通过实例调用了render方法，想拿到return的值，就要执行<h1 onClick={demo()}>今天天气很{this.state.isHot ? "炎热" : "凉爽"}</h1>，执行到onClick赋值语句时，就要将demo()函数调用的返回值交给onClick作为回调，demo()的返回值是undifend,也就是把undifend交给onClick作为回调，这是错误的做法，是因为在demo后加()，导致函数调用。等到点击时，就调用了undifend，react处理了这一过程，如果是undifend,jiu