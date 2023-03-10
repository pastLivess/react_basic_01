# day1

## React的介绍

​	声明式 -> 组件化 -> 跨平台

![image-20230214201744859](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230214201744859.png)

### 声明式编程

 维护数据 -> 渲染至页面

![image-20230214202020405](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230214202020405.png)

### 组件化开发

![image-20230214202059086](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230214202059086.png)

### 跨平台

![image-20230214202408111](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230214202408111.png)

## react开发依赖

![image-20230214203155801](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230214203155801.png)

![image-20230214212356266](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230214212356266.png)

~~~~jsx
// 三个地址 通过mdn引入   crossorigin是用于解决跨域的
<script src="https://unpkg.com/react@18/umd/react.production.min.js" crossorigin是用于解决跨域的></script>
    <script
      src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"
      crossorigin
    ></script>
// babel是用于将jsx转为js的
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
~~~~

## react实现hello world

~~~~jsx
// 1. 容器 名为root和app都可以
<div id="root"></div>

//type="text/babel" 指定用babel将jsx转为js
    <script type="text/babel">
      //2. react18以前的API ReactDOM.render() 
      // 参数一 渲染哪个元素 参数二 将元素放到哪个容器
      // ReactDOM.render(<h2>hello world</h2>, document.querySelector('#root'))

      //3. 18以后为createRoot() 分为两步
      const root = ReactDOM.createRoot(document.querySelector('#root'))
      root.render(<h2>hello world</h2>)
~~~~

## react实现hello react

![image-20230214213044513](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230214213044513.png)

![image-20230214213052621](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230214213052621.png)

~~~~jsx
    <div id="root"></div>
    <script type="text/babel">
      const root = ReactDOM.createRoot(document.querySelector('#root'))

      let message = 'hello world'
      
      function changeMessage() {
        //1. 修改数据
        message = 'hello react'
        //2. 重新渲染界面 两种方式 第一种直接调用root.render重新渲染
        rootRender()
      }

      rootRender()

      //3. 封装渲染函数
      function rootRender() {
        // 1.jsx语法中统一绑定变量使用 单括号
        // 2.jsx语法中绑定事件 onXxxx
        root.render(
          <div>
            <h2>{message}</h2>
            <button onClick={changeMessage}>修改文本</button>
          </div>
        )
      }
    </script>
~~~~

![image-20230308014220505](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230308014220505.png)

## react中使用类组化与事件绑定(重点)

![image-20230214214410961](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230214214410961.png)

~~~~jsx
   class Root extends React.Component {
        // 组件数据
        constructor() {
          super()
          this.state = {
            message: 'hello world',
            name: 'state',
            age: 19,
          }
          // 这样做也可以,之后所有changeMessage方法调用不需要绑定this了
          // this.changeMessage = this.changeMessage.bind(this)
        }
        // 组件方法(实例方法)
        changeMessage() {
          // 内部做了两件事 1.将state中message值改掉 2.自动重新执行render函数
          this.setState({
            message: 'hello react',
          })
        }
        // 渲染组件 绑定的事件需要通过this访问
        render() {
          return (
            <div>
              <h2>{this.state.message}</h2>
              <button onClick={this.changeMessage.bind(this)}>修改文本</button>
            </div>
          )
        }
      }

      // react渲染的方法
      const root = ReactDOM.createRoot(document.querySelector('#root'))
      root.render(<Root />)
~~~~

### 案例二:电影列表多种方式实现

~~~~jsx
 class Root extends React.Component {
        constructor() {
          super()
          this.state = {
            movies: ['变形金刚', '终结者', '异形', '星际穿越'],
          }
        }
        render() {
          // 第一种做法 for循环
          /*  const liEls = []
          for (let i = 0; i < this.state.movies.length; i++) {
            const movies = this.state.movies[i]
            const liEl = <li>{movies}</li>
            liEls.push(liEl)
          } */
            
          // 第二种做法 map循环
          // const liEls = this.state.movies.map((item) => <li>{item}</li>)
          return (
            <div>
              <h2>电影列表</h2>
              <ul>
                  // 直接用jsx放在这里即可 jsx中可以写表达式
                {this.state.movies.map((item) => (
                  <li>{item}</li>
                ))}
              </ul>
            </div>
          )
        }
      }

      const root = ReactDOM.createRoot(document.querySelector('#root'))
      root.render(<Root />)
~~~~

### 案例三:计数器案例

~~~~jsx
  class Root extends React.Component {
        constructor() {
          super()
          this.state = {
            counter: 0,
          }
          this.increment = this.increment.bind(this)
          this.decrement = this.decrement.bind(this)
        }
        increment() {
          this.setState({
            counter: this.state.counter + 1,
          })
        }
        decrement() {
          this.setState({
            counter: this.state.counter - 1,
          })
        }
        render() {
          return (
            <div>
              <h2>当前计数:{this.state.counter}</h2>
              <button onClick={this.increment}>+</button>
              <button onClick={this.decrement}>-</button>
            </div>
          )
        }
      }
      const root = ReactDOM.createRoot(document.querySelector('#root'))
      root.render(<Root />)
~~~~

# jsx语法

### React为何选择jsx

![image-20230310162942593](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230310162942593.png)

![image-20230310163253195](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230310163253195.png)

React初期做了取舍 但是为了让代码高内聚 低耦合 让代码联系更佳紧密 而不是像vue一样绑来绑去  React选择jsx

![image-20230310163423710](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230310163423710.png)

### jsx书写规范

~~~~jsx


    <script type="text/babel">
      class Root extends React.Component {
        constructor() {
          super()
          this.state = {
            message: 'hello world',
          }
        }

        // jsx结构必须有一个根元素 与vue2一致
        // 返回的结构推荐写一个()包裹
        // jsx中标签可以是单标签 且不能省略 /
        render() {
          return (
            <div>
              <h2>{this.state.message}</h2>
            </div>
          )
        }
      }

      const root = ReactDOM.createRoot(document.querySelector('#root'))
      root.render(<Root />)
    </script>


~~~~

### jsx注释

~~~~jsx
render() {
          return (
            <div>
              {/* jsx注释的写法  */}
              <h2>{this.state.message}</h2>
            </div>
          )
        }
      }
~~~~

### jsx中插入变量注意事项

~~~~jsx
 class Root extends React.Component {
        // 组件数据
        constructor() {
          super()
          this.state = {
            message: 'hello world',
            age: 18,
            arr: [1, 2, 3],
            a: undefined,
            b: true,
            c: null,
            obj: { name: 'obj' },
          }
        }

        render() {
          const { message, age, arr, a, b, c, obj } = this.state
          return (
            <div>
              {/* jsx中变量插入标签体的注意事项  字符串/数字/数组可以正常显示 */}
              <h2>{message}</h2>
              <h2>{age}</h2>
              <h2>{arr}</h2>
              {/* undefined/null/布尔值 会被jsx解析为空 */}
              <h2>{a}</h2>
              <h2>{b}</h2>
              <h2>{c}</h2>
              {/* Object类型不能直接被展示 */}
              <h2>{obj}</h2>
            </div>
          )
        }
      }

~~~~

### jsx插入不同表达式

![image-20230310170401193](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230310170401193.png)

~~~~jsx
  class App extends React.Component {
        constructor() {
          super()
          this.state = {
            firstName: '张',
            lastName: '三',
            age: 15,
            arr: ['唱', '跳', 'rap'],
          }
          this.arrFn = this.arrFn.bind(this)
        }
        arrFn() {
          return this.state.arr.map((item) => <li key={item}>{item}</li>)
        }
        render() {
          const { firstName, lastName } = this.state
          // jsx中没有类似vue的计算属性 直接使用原生js操作即可
          const fullName = firstName + lastName
          return (
            <div>
                  {/* 计算表达式 */}
              <h2>{fullName}</h2>
                  {/* 三元运算符 */}
              <h2>{this.state.age > 18 ? '成年' : '未成年'}</h2>
                  {/* 调用函数返回结果 */}
              <ul>{this.arrFn()}</ul>
            </div>
          )
        }
      }
      const root = ReactDOM.createRoot(document.querySelector('#root'))
      root.render(<App />)
~~~~

### jsx绑定基本属性

~~~~jsx
<script type="text/babel">
      class App extends React.Component {
        constructor() {
          super()
          this.state = {
            message: 'hello react',
            title: '哈哈哈',
            imgUrl: 'https://sponsors.vuejs.org/images/vuemastery.avif',
            a: 'https://cn.vuejs.org/',
          }
        }
        render() {
          const { message, title, imgUrl, a } = this.state
          return (
            <div>
              {/* jsx绑定全局属性 或这些简单的属性 */}
              <h2 title={title}>{message}</h2>
              <img src={imgUrl} alt="" />
              <a href={a}>vue</a>
            </div>
          )
        }
      }
      const root = ReactDOM.createRoot(document.querySelector('#root'))
      root.render(<App />)
    </script>
~~~~

### jsx绑定class和style

~~~~jsx
<script type="text/babel">
      class App extends React.Component {
        constructor() {
          super()
          this.state = {
            message: 'hello world',
            isActive: true,
            style: {
              color: 'blue',
              fontSize: '50px',
            },
          }
        }
        render() {
          const { message, isActive, style } = this.state

          // jsx中动态添加class推荐写法
          // 写法一 字符串拼接
          // const className = `abc cba ${isActive ? 'active' : ''}`
          // 写法二 将所有class放到数组中
          const classList = ['abc', 'cba']
          if (isActive) classList.push('active')
          // 写法三 要使用到第三方库(classnames) 后面脚手架再讲
          return (
            <div>
              {/* 由于js中class是关键字 所以jsx中绑定class 使用className属性 */}
              <h2 className={classList.join(' ')}>{message}</h2>
              {/* 绑定style 对象写法 */}
              <h3 style={style}>{message}</h3>
            </div>
          )
        }
      }
      const root = ReactDOM.createRoot(document.querySelector('#root'))
      root.render(<App />)
    </script>
~~~~



# 今日API总结

1.ReactDOM.createRoot(传入一个真实DOM节点) ->ReactDOM.createRoot(document.querySelector('#root')) 

​	用于创建一个React根,之后渲染的内容会包含在该根中

2.root.render函数

​	参数:要渲染的根组件

例: root.render(<h2>hello</h2>)

3.jsx插值语法 {}  vue插值语法 {{}} mustanche

4.创建类组件 class 组件 extends React.Component {}

5.jsx绑定标签体内容 字符串/数组/数字 均可   null/undefined/布尔值 不会显示 Object类型不能绑定

6.jsx绑定属性 -> 基本属性使用jsx插值即可 <img  src={}>

7.jsx绑定class -> 字符串拼接 class=`a ${isActive ? 'active' :''} `

​	数组写法 -> const arr = ['a']  if(isActive) arr.push(active)

8.jsx绑定style -> 对象写法  const obj ={color:red} <h2 style={obj}>你好</h2>

# 作业总结

## 一. 完成课堂所有的代码







## 二. React开发的三个依赖包是什么？分别有什么作用？

react.development 是react的核心代码

react-dom.development 是react中对DOM操作的部分

babel.min.js babel是工具用于将jsx语法转为js语法





## 三. React如何封装组件，组件里面包含哪些内容？



~~~~jsx
class 组件 extends React.Component {

	constructor(){

		super()  // 调用父类中方法和属性 用于继承
		// state用于存放数据
		this.state = {

			message:'hello'

	 	}

	}
    // 函数在模板中被调用时 this指向是window 这里是类 所以是undefined 需要我们手动改this为当前这个类
    changeMessage(){
        // setState方法用于修改state中的数据
        this.setState({
            message:"你好"
        })
    }
    // render函数用于返回jsx中代码 名字不能换 render函数中的this是当前类
    render(){
        const {message} = this.state
        return (
            <div>
                <h2>{message}</h2>
                {/* jsx中绑定事件通过onXxxx  this.changeMessage访问当前的函数调用bind将this改为当前类 */}
                <button onClick={this.changeMessage.bind(this)}>改变message</button>
            </div>)
    }

 }
~~~~





## 四. 在进行函数绑定时，如何进行this关键字的绑定？



~~~~jsx
使用bind函数 bind函数会返回一个修改this后的函数
~~~~

### 手写bind

~~~~js
function foo(a, b, c) {
        console.log(this, a, b, c)
      }

      Function.prototype.myBind = function (thisArg, ...args) {
        return (...arg) => {
          if (thisArg === 'undefined' || thisArg === 'null') thisArg = window
          thisArg = Object(thisArg)
          thisArg.fn = this
          thisArg.fn(...args, ...arg)
          // console.log(thisArg, args)
          delete thisArg.fn
        }
      }
      foo.myBind('1', 1)(2, 3)
~~~~





## 五. React如何进行列表数据的展示？回顾数组的常见高阶函数。

~~~~Js
jsx插值语法中 可以写表达式 我们可以使用map等函数进行遍历 map会返回一个数组中包含着加工好的数组

 class Root extends React.Component {
        constructor() {
          super()
          this.state = {
            movies: ['变形金刚', '终结者', '异形', '星际穿越'],
          }
        }
        render() {

          return (
            <div>
              <h2>电影列表</h2>
              <ul>
                  // 直接用jsx放在这里即可 jsx中可以写表达式
                {this.state.movies.map((item) => (
                  <li>{item}</li>
                ))}
              </ul>
            </div>
          )
        }
      }

      const root = ReactDOM.createRoot(document.querySelector('#root'))
      root.render(<Root />)
~~~~

### 常用高阶函数

~~~~js
// forEach 将数组中每个元素进行遍历 跳出循环使用抛出异常的方式
      const arr = [1, 2, 3]
      arr.forEach((item) => {
        if (item === 2) {
          return
        }
        console.log('forEach', item)
      })

      // map方法 返回一个数组 当结果为true 数组中包含加工好的每个元素
      let result = arr.map((item) => item * 2)
      console.log('map', result)

      // filter 返回一个数组 当结果为true 数组中包含满足条件的元素
      result = arr.filter((item) => item === 2)
      console.log('filter', result)

      // reduce 求和函数
      result = arr.reduce((prev, item) => prev + item)
      console.log('reduce', result)

      // find/findIndex函数 当结果为true 返回那个找到的元素/索引
      result = arr.find((item) => item === 2)
      console.log('find', result)
      result = arr.findIndex((item) => item === 2)
      console.log('findIndex', result)

      // every 检查数组中元素是否满足要求 全部满足返回true 全部不满足返回false
      result = arr.every((item) => item > 1)
      console.log('every', result)

      // some 检查数组中元素是否有一个满足要求 满足返回true 不满足返回false
      result = arr.some((item) => item > 1)
      console.log('some', result)

      // sort 排序函数 a-b 升序 b-a 降序
      result = arr.sort((a, b) => b - a)
      console.log('sort', result)
~~~~



## 六. JSX如何绑定数据？如何绑定内容、属性，有哪些规则？

1. jsx通过jsx插值语法绑定值 
2. 标签体内容可以绑定的数据类型为 number/string/array  
3. 标签体绑定后不会显示在页面的数据类型为 null/undefined/true(false)
4. 标签体不能绑定的数据类型 Object
5. 属性绑定时也是过jsx插值语法
6. jsx中绑定class 通过className属性 因为class是js中的关键字
7. 绑定class时 可以使用字符串写法cosnt str='a b c ' 或数组const arr=['a','b','c']  **<h2 className={arr}><h2/>**
8. 绑定style时 可以通过对象写法 const obj ={color:red} **<h2 style={obj }><h2/>**



## 七. JSX绑定事件，this绑定有哪些规则？

### this绑定规则

~~~~js
默认绑定
function fn ()[] fn()

显式绑定 call/apply/bind
const obj={fn}
fn.call(obj)

// 隐式绑定
obj.fn()

// new绑定 this指向当前类
class Person{} new Person()
~~~~

