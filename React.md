# <center><font color='red'>React</font></center>

##一、创建`React`项目
```
npx create-react-app my-app
cd my-app
npm start
```
>第一行的 `npx` 不是拼写错误 —— 它是 npm 5.2+ 附带的 package 运行工具



##二、引入`less`

###1.安装
```
npm install less less-loader --save-dev
```

###2.暴露配置文件
**暴露`webpack`配置文件**

使用 `create-react-app` 创建的项目，默认情况下是看不到 `webpack` 相关的配置文件，我们需要给它暴露出来，使用下面命令即可
```
npm run eject
```
>运行之后，我们发现多了一个config文件夹


###3.修改配置文件
**修改`webpack`配置文件**

修改：
```
const cssRegex = /\.css$/;
const cssModuleRegex = /\.module\.css$/;
```
为:
```
const cssRegex = /\.(css|less)$/;
const cssModuleRegex = /\.module\.(css|less)$/;
```

`use`数组配置增加`less-loader`：
```
{
    loader: require.resolve('less-loader'),
},
```


##三、路由
**安装`react-router-dom`**
```
npm install -save react-router-dom
```

###1.`router`选择
`React-router`和`React-router-dom`的选择
* **`React-router`**
`React-router`提供了一些router的核心api，包括`Router`, `Route`, `Switch`等，但是它没有提供dom操作进行跳转的api。
* **`React-router-dom`**
`React-router-dom`提供了`BrowserRouter`, `Route`, `Link`等api,我们可以通过dom的事件控制路由。例如点击一个按钮进行跳转，大多数情况下我们是这种情况，所以在开发过程中，我们更多是使用`React-router-dom`。

###2.`router`用法
`React-router-dom`的核心用法
1. `HashRouter`和`BrowserRouter`
**前者在你有服务器处理动态请求的时候使用，后者在静态网站的时候使用。通常我们选择使用`BrowserRouter`**
它们两个是路由的基本，就像盖房子必须有地基一样，我们需要将它们包裹在最外层，我们只要选择其一就可以了。现在讲它们的不同：

    * **`HashRouter`**
如果你使用过react-router2或3或者vue-router，你经常会发现一个现象就是url中会有个#，例如localhost:3000/#，`HashRouter`就会出现这种情况，它是通过hash值来对路由进行控制。如果你使用`HashRouter`，你的路由就会默认有这个#。

    * **`BrowserRouter`**
很多情况下我们则不是这种情况，我们不需要这个#，因为它看起来很怪，这时我们就需要用到`BrowserRouter`。

2. `Route`
`Route`是路由的一个原材料，它是控制路径对应显示的组件。我们经常用的是`exact`、`path`以及`component`属性
`exact`控制匹配到/路径时不会再继续向下匹配，`path`标识路由的路径，`component`表示路径对应显示的组件。后面我们将结合`NavLink`完成一个很基本的路由使用。同时我们可以设置例如`/second/:id`的方式来控制页面的显示，这需要配合`Link`或者`NavLink`配合使用。下面我们会提到

路径|location.pathname|exact|是否匹配
---|:--:|---:|---
/one|/one/two|true|否
/one|/one/two|false|是

路径|location.pathname|strict|是否匹配
---|:--:|---:|---
/one/|/one|true|否
/one/|/one/|true|是
/one/|/one/two|true|是

<Route>提供了三种渲染内容的方法：

>* `Route component`：在地址匹配的时候React的组件才会被渲染，route props也会随着一起被渲染；
>* `Route render`：这种方式对于内联渲染和包装组件却不引起意料之外的重新挂载特别方便；
>* `Route children`：与render属性的工作方式基本一样，除了它是不管地址匹配与否都会被调用；



3. **`Link`和`NavLink`的选择**
两者都是可以控制路由跳转的，不同点是`NavLink`的api更多，更加满足你的需求。

* **`Link`**
主要api是`to`，`to`可以接受string或者一个object，来控制url。

* **`NavLink`**
它可以为当前选中的路由设置类名、样式以及回调函数等
`exact`用于严格匹配，匹配到/则不会继续向下匹配，`to`则是控制跳转的路径，`activeClassName`是选中状态的类名，我们可以为其添加样式。我们在`/second`后面添加`1234`来想路由中传递信息，这结合了上面`Route`中的`/second/:id`

>使用`Link`或者`NavLink`后，添加行内样式`textDecoration:'none'`消除路由选项的下划线

4. `match`
`match`是在使用`router`之后被放入`props`中的一个属性，在`class`创建的组件中我们需要通过`this.props.match`来获取`match`之中的信息。

5. `Switch`
`Switch`常常会用来包裹`Route`，它里面不能放其他元素，用来只显示一个路由。


###3.路由跳转
1.`DOM`跳转
在需要跳转的页面导入`import {Link} from 'react-router-dom'`,在需要跳转的地方使用`Link`标签的`to`属性进行跳转，路由配置文件中导出的那个类名相当于`router-view`标签，在需要展示的地方引入

* ①在路由配置文件中配置路由
    ```
    <Route path="/second" component={Second}/>
    <Route path="/third" component={Third}/>
    <Route path="/child01" component={Child01}/>
    ```
* ②在需要跳转的页面引入`import {Link} from 'react-router-dom'`
* ③使用`Link`标签进行跳转
    ```
    import React form 'react'
    import {Link} from 'react-router-dom'

    class First extends React.Component{
        render(){
            return(
                <div>
                    <Link to="/child01">this is first</Link>
                </div>
            )
        }
    }
    export default First;
    ```
* ④在需要展示的区域进行展示
    `App.js:`
    ```
    import React,{ Component } form 'react'
    important '/App.css'
    //导入路由配置文件中导出的类
    import RouterIndex form './router/index'

    class App extends Component{
        render(){
            return(
                <div>
                    <RouterIndex />
                </div>
            )
        }
    }
    export default App;
    ```
2.`js`跳转
* 使用`this.props.history.push('/child02')`
    ```
    import React form 'react'
    import {Link} from 'react-router-dom'

    class First extends React.Component{
        constructor(props){
            super(props);
            this.state={}
        }
        btnFn(){
            this.props.history.push('./child02')
        }
        render(){
            return(
                <div>
                    <Link to="/child01">this is first</Link>
                    <button onClick={this.btnFn.bind(this)}>点我啊</button>
                </div>
            )
        }
    }
    export default First;
    ```

###4.路由跳转
* ①`params`参数
    * a:在路由配置中以`/:`的方式拼接参数标识
        ```
        <Route path="/child01/:id" component={Child01}/>
        <Route path="/child02/:name" component={Child02}/>
        ```
    * b:在路径后面将参数拼接上(`/参数`)
        ```
        btnFn(){
            this.props.history.push('./child02/张三')
        }
        render(){
            return(
                <div>
                    <Link to="/child01/007">this is first</Link>
                    <button onClick={this.btnFn.bind(this)}>点我啊</button>
                </div>
            )
        }
        ```
    * c:在被跳转页使用`this.props.match.params.xxx`接收参数

* ②`query`传参
    ```
    btnFn2(){
        this.props.history.push({pathname:'/child03',query:{name:'jack',age:18}})
    }
    render(){
        return(
            <div>
                <!-- props传参 -->
                <Link to="/child01/007">this is first</Link>

                <button onClick={this.btnFn.bind(this)}>点我啊1</button>
                
                <!-- query传参 -->
                <button onClick={this.btnFn2.bind(this)}>点我啊2</button>
            </div>
        )
    }
    ```
    * a:在`router`文件中配置为正常配置`<Route path="/child03/:name" component={Child03}/>`
    * b:在跳转时，路径为一个对象`{}`，其中`pathname`为路径，`query`为一个对象，对象里是携带的参数
    * c:使用`this.props.location.query`接收参数

* ③`state`传参
    使用`this.props.location.state`接收参数
    ```
    <!-- state传参 -->
    <Link to={{pathname:'/child04',state:{name:'香蕉',price:20}}}>state传参</Link>
    ```
### 5.按需加载
> 一个动态导入加载组件的高阶组件--React Loadable.
> `npm install React Loadable`

```
// import Home from './page/home/home'
// import Page_test1 from './page/page_test1/page_test1'
// import Page_test2 from './page/page_test2/page_test2'
// import Page_test3 from './page/page_test3/page_test3'

import React from 'react'
import Loadable from 'react-loadable'

function LoadingComponent({ error, pastDelay }) {
    if (error) {
      return <div>Error!</div>;
    } else if (pastDelay) {
      return <div>Loading...</div>;
    } else {
      return null;
    }
  }

// delay 默认是 200ms，可以向 Loadable 传递第 3 个参数自定义 delay。
const Home = Loadable({
    loader:()=>import('./page/home/home'),
    loading:LoadingComponent,
    delay: 300
})
const Page_test1 = Loadable({
    loader:()=>import('./page/page_test1/page_test1'),
    loading:LoadingComponent,
    delay: 300
})
const Page_test2 = Loadable({
    loader:()=>import('./page/page_test2/page_test2'),
    loading:LoadingComponent,
    delay: 300
})

const Page_test3 = Loadable({
    loader:()=>import('./page/page_test3/page_test3'),
    loading:LoadingComponent,
    delay: 300
})


const router = [
    {
        path:'/',
        component:Home,
        name:'home'
    },
    {
        path:'/home',
        component:Home,
        name:'home'
    },
    {
        path:'/page_test1',
        component:Page_test1,
        name:'page_test1'
    },
    {
        path:'/page_test2',
        component:Page_test2,
        name:'page_test2'
    },
    {
        path:'/page_test3',
        component:Page_test3,
        name:'page_test3'
    }
]

export default router;
```



##四、发布到tomcat
1. `package.json`里面增加`"homepage":"."`
2. 使用`npm run build`将项目打包
3. 把打包后的`build`文件夹更名为`package.json`中`name`参数名，放到tomcat中webapps中
4. 开启服务器访问'localhost:8080/项目名'即可以看到你的项目内容

##五、吸顶功能
```
position{
    position:sticky;
    top:0;
}
```
>sticky属性特点是在该dom对象在可是范围内,其定位属性无效，但是一旦离开其定位位置，sticky属性将会改变为Fixed定位方式。这样能够很好地处理吸顶操作


##六、父子组件
1.父 => 子（传值）
```
<!-- 父组件： -->
a = 1

<!-- 子组件： -->
this.props.a = 1
```

2.子 => 父（传值）
```
<!-- 父组件： -->
fun={this.fun.bind(this)}
fun(event){
    值为：event
}

<!-- 子组件： -->
this.props.fun(val)
```

3.父 => 子（传方法）
```
<!-- 父组件： -->
fun={this.fun()}

<!-- 子组件： -->
this.props.fun()
```

4.子 => 父（传方法）
```
<!-- 父组件： -->
this.refs.ref值.fun()

<!-- 子组件： -->
this.fun()
```