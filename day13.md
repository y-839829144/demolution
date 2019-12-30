## 如何使用内置组件

大部分依赖库只需要在依赖管理里面导入对应的依赖名称,注入到组件中即可

```js
export default {
	components:{ ...app.getEntry('package_elementui').components}
	data(){
		return{}
	}
}

//注意:
vbench是 vbench_admin的子集,vbench_admin包含一些只能在商家管理中心使用的组件
在管理界面 无需声明依赖 即可直接使用vbench_admin组件
使用的方法: app.getVbench().components
app.getDcompany()这样的写法,其实等价于 app.getEntry('dcompany')
```

## Vbench

Vbench架构

* 在线拼接组件实现任意应用
* 实现设计和开发的融合

Vbench Vue组件的组合平台,支持通过布局组合Vue组件实现功能和界面

webpack(构建工具)

当使用webpack或 Browserify类似的构建工具时,Vue源码会根据 process.env.NODE_ENV 决定是否启用生产环境模式,默认情况为开发环境模式.在webpack与Browserify中都有方法来覆盖此变量,以启用Vue的生产环境模式,同时在构建过程中警告语句也会被 压缩工具去除,所有这些在 vue-cli模板中都预先配置好了.

## Dcompany 

定义: Dcompany 是相对较老的组件库,里面除了compoents, 



## *Mixins

DwProxyShareMixin: 小程序宿主对象 - 分享部分 APP



buildStore(app)

列表服务



kind: global function 全局函数

* buildStore(app)

  * ~getters

  * ~actions

    * .mixinUpdateQueryData = > promise

      更新 查询参数

    * .mixinApiFactory() => promise

      修改查询参数

    * .mixinLoad() = > promise

      加载列表页

    * actions.updateCurrentPage() = > promise

      更新分页

    * actions.mixinUpdateFilter() => promise

      更新参数



## filters

filters   object   常用过滤器对象,包括qiniu|dateformat



## 是否启用生产环境模式

(默认情况为开发环境模式)

在 webpack4+中, 可以使用mode选项:



```
	module.exports = {

​		mode:'production'

}
```

在webpack3及其更低版本中,需要使用DefinePlugin:

```js
var webpack = require('webpack')

module.exports = {
    //...
    plugins:[
        new webpack.DefinePlugin({
            'process.env.NODE_ENV':JSON.stringify('production')
        })
    ]
}
```

 Browserify

* 在运行打包命令时将NODE_ENV 设置为' production' .这等于告诉vueify避免引入热重载和开发相关的代码

* 对打包后的文件进行一次全局的envify转换,这使得压缩工具能清楚掉Vue源码中所有用环境变量条件包裹起来的警告语句.

  ```js
  NODE_ENV=production browserify -g envify -e main.js | uglifyjs
   -c  -m > build.js
  ```

* 或者在Gulp中使用envify:

  ```js
  // 使用 envify 的自定义模块来定制环境变量
  var envify = require('envify/custom')
  
  browserify(browserifyOptions)
  	.transform(vueify)
  	.transofrm(
  	// 必填项,以处理 node_modules里的文件
      { global : true},
      	envify({NODE_ENV:'production'})
  	)
  	.bundle()
  
  ```

  ## export 与 export default区别

  export 与 export default 均可用于导出常量,函数,文件,模块等,你可以在其他文件或模板中通过import+(常量 | 函数 | 文件 |模块)名的方式,将其

  导入

* 但在一个文件或者模块中,export   import可以有多个,export default仅有一个

  ```js
  // p1.js
  export const str = 'hello world!'
  
  export function f(a){
      return a+1
  }
  
  //p2.js
  import {str,f} from 'p1'
  
  // p1.js
  export default const str = 'hello world!'
  // p2.js
  import str from 'p1'
  
  
  ```

  ## require

  ```js
  
  require('./a')();// a 模块是一个函数,立即执行a模块函数
  var data = require('./a').data; //a模块导出的是一个对象
  var a = require('./a')[0]; //a模块导出的是一个数组
  ```

  ## *prop 类型

  ```js
  //通过prop向子组件传递数据
  //prop是你在组件上注册的一些自定义特性
  //当一个值传递给一个prop特性的时候,它就变成了那个组件实例的一个属性
  
   Vue.component/*全局定义组件*/('blog-post',{
       props:['title'],
       template:'<h3>{{title}}</h3>'
   })
  
  
  // HTML
  <blog-post title='hello world!'></blog-post>
  <blog-post title='hello JavaScript!'></blog-post>
  <blog-post title='hello Vue!'></blog-post>
  
  
  new Vue({
      el:'#blog-post-demo',
      data:{
          posts:[
              {id:1,title:'hello world!'},
              {id:2,title:'hello word!'},
              {id:3,title:'hello Vue!'}
          ]
      }
  })
  //HTML
  <blog-post
  	v-for = 'post in posts'
  	v-bind:key = "post.id",
      v-bind:title = "post.title"
  ></blog-post>
  
  
  
  当构建一个 <blog-post>组件时,你的模板最终会包含的东西远不止一个title
  
  
  Vue.components('blog-post',{
      props:['post'],
      template:
      <div class = "blog-post">
      	<h3>{{post.title}}</h3>
          <div v-html = "post.content"></div>
       </div>
                 
  })
  new Vue = ({
      el:'#blog-post',
      data:{
          posts:[
              {id:1,title:'',content:''},
              {id:2,title:'',content:''},
              {id:3,title:'',content:''},
              {id:4,title:'',content:''},
          ]
      }
  })
  ```

   

  ```js
  //组件的局部注册
  
  var ComponentA = {/*...*/}
  var ComponentB = {/*...*/}
  
  new Vue({
      el: '#app',
      components:{
          'component-a':componentsA,
          'component-b':componentsB
      }
  })
  
  //对于components 对象中的每个属性来说,其属性名就是自定义元素的名字,其属性值就是这个组件的选项对象
  
  
  //注意局部注册的组件在其子组件中不可用
  //设:componentA在componentB中可用
  
  var ComponentA = {/*...*/}
  var ComponentB = {
      components:{
          'components-a':ComponentA
      }
  }
  ```

  ## ref 与$refs

  ```js
  //ref 被用来给元素或子组件注册引用信息.引用信息将会注册在父组件的$refs 对象上,如果在普通的DOM元素上使用,引用指向的就是DOM元素；如果用在子组件上,引用就指向组件实例:
  
  //当 v-for 用于元素或组件的时候,引用信息将是包含DOM节点或组件实例的数据
  //关于ref注册时间的重要说明:因为ref本身是作为渲染结果被创建的,在初始渲染的时候 你不能访问他们-他们还不存在 $refs 也不是响应式,因此你不应该视图用它在模板中做数据绑定
  
  
  
  <base-input ref = "usernameInput"></base-input>
  
  this.$refs.usernameInput
  
  
  <input ref = "input">
      
  methods:{
      //用来从父级组件聚焦输入框
      focus:function(){
          this.$refs.input.focus()
      }
  }
  //这样就允许父级组件通过下面的代码聚焦<base-input> 里的输入框:
  this.$refs.usernameInput.focus()
  ```

## 插槽

```js
<navigation-link url = "/profile">
    your profile
</navigation-link>

<a 
	v-bind:href="url"
	class = "nav-link"
>
       <slot></slot>
	</a>
//当组件渲染的时候,<slot></slot>将会被替换为"you profile" 插槽内可以包含任何模板代码

<navigation-link url = "/profile">
    <span class = "fa fa-user"></span>
	you profile
</navigation-link>

<navigation-link url="/profile">
    <fon-awesome-icon name ="user"></font-awesome-icon>
	you profile
    </navigation-link>

```



## Vue的内置指令

```js
1. v-bind
v-bind主要用于属性绑定,Vue官方提供雷一个简写方式:bind
v-bind主要用于属性绑定,比方你的class属性,style属性,value属性,href属性等等
<img v-bind:src="imageSrc"
//内联字符串拼接
<img:src="'/path/to/images'+fileName"

//绑定HTML Class对象语法
<div v-bind:class="{active:isActive}"></div>

<div class = "static"
	v-bind:class="{active: isActive,'text-danger':hasError}">
 </div>
data:{
    isActive:true,
    hasError:false
}
结果为:
<div class = "static active"></div>

//用在组件上
Vue.component('my-compoent',{
    template:'<p class = "foo bar">Hi</p>'
})

<my-component class = "barz boo "></my-component>

```



## 购物车页面

currentTarget 和 currentTab

