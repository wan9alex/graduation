



### 公司

#### 与后端沟通配合

前后端都要充分了解项目的需求

后端只提供能力（如数据库能力、消息能力、应用协同能力等等）

前端组织业务（如组织Auth、组织数据POST/GET/DELETE）

接口设计（出接口文档），前后端沟通设计接口，前端需要后台返回什么样的数据（格式），后台需要前端传递什么参数（哪些参数是必须的，哪些参数是可选的，采用get还是post，哪些数据需要前端先进行校验，哪些需要双方都校验）。共同制定出整个程序所有的接口说明，形成文档。前后台按照约定好的接口进行开发

#### 项目遇到了的问题及解决方案

短时间的大访问量：	网站服务器	同网站，不同项目部署，/独立域名 避免对网站造成影响
高并发问题不停刷新：	数据库	页面静态化
带宽 200k的页面 并发1w次 ：	秒杀页缓存cdn 租借临时带宽，反向代理服务器，nginx ，甚至用户浏览器。（cookie）
不能提前下单：	服务器url动态化，＋随机数

下单之后的被抢的问题：	库加 乐观锁





#### 项目开发流程

![img](https://mmbiz.qpic.cn/mmbiz_png/iappkPENyMQic0yZRtgb303icrOSKkhut3FF4033ga1tZdDeu2R2wTsubibzwGSXfamtFW0YEKJGQcWhUBq6ZkTLNQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

PM：产品经理，也是一个项目的推动者，即兼职项目经理的角色。

UE：交互设计师，负责页面布局、交互的设计，不负责视图的细节。

UI：视觉设计师，交互确定之后，设计页面样式。注意，很多情况下，UE 和 UI 是一个人。

RD：后端开发人员。

CRD：客户端开发人员，安卓和 ios 都是。

FE：前端开发人员。

QA：测试人员。

OP：服务器运维人员，一般负责审批上线单

------



### 原理和思想

#### diff算法

React用 三大策略 将O(n^3)复杂度 转化为 **O(n)复杂度**

策略一（**tree diff**）：
 Web UI中DOM节点跨层级的移动操作特别少，可以忽略不计。

策略二（**component diff**）：
 拥有相同类的两个组件 生成相似的树形结构，
 拥有不同类的两个组件 生成不同的树形结构。

策略三（**element diff**）：
 对于同一层级的一组子节点，通过唯一id区分

#### 虚拟DOM

虚拟 dom 相当于在 js 和真实 dom 中间加了一个缓存，利用 dom diff 算法避免了没有必要的 dom 操作，从而提高性能。

用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异把 2 所记录的差异应用到步骤 1 所构建的真正的 DOM 树上，视图就更新了。

#### 状态管理

##### redux的理解

是什么：是一个应用数据流框架，集中式管理

怎么用：view 调用 store 的 dispatch 接收 action 传入 store，reducer 进行 state 操作，view 通过 store 提供的 getState 获取最新的数据

缺点： state必须由父组件传过来，子组件相关数据更新，父组件会强渲

##### flux 思想

单向数据流

1. 用户访问 View
2. View 发出用户的 Action
3. Dispatcher 收到 Action，要求 Store 进行相应的更新
4. Store 更新后，发出一个"change"事件
5. View 收到"change"事件后，更新页面

##### vuex的理解

原理：vuex 仅仅是作为 vue 的一个插件而存在，不像 Redux,MobX 等库可以应用于所有框架，vuex 只能使用在 vue 上，很大的程度是因为其高度依赖于 vue 的 computed 依赖检测系统以及其插件系统，

vuex 整体思想诞生于 flux,可其的实现方式完完全全的使用了 vue 自身的响应式设计，依赖监听、依赖收集都属于 vue 对对象 Property set get 方法的代理劫持。最后一句话结束 vuex 工作原理，vuex 中的 store 本质就是没有 template 的隐藏着的 vue 组件；

使用：在 main.js 引入 store，注入。新建了一个目录 store，….. export 

场景：单页应用中，组件之间的状态。音乐播放、登录状态、加入购物车



#### MVVM思想

MVVM 是 Model-View-ViewModel 的缩写。mvvm 是一种设计思想。Model 层代表数据模型，也可以在 Model 中定义数据修改和操作的业务逻辑；View 代表 UI 组件，它负责将数据模型转化成 UI 展现出来，ViewModel 是一个同步 View 和 Model 的对象。

在 MVVM 架构下，View 和 Model 之间并没有直接的联系，而是通过 ViewModel 进行交互，Model 和 ViewModel 之间的交互是双向的， 因此 View 数据的变化会同步到 Model 中，而 Model 数据的变化也会立即反应到 View 上。

ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，而 View 和 Model 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作 DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理

#### mvc

mvc 和 mvvm 其实区别并不大。都是一种设计思想。主要就是 mvc 中 Controller 演变成 mvvm 中的 viewModel。mvvm 主要解决了 mvc 中大量的 DOM 操作使页面渲染性能降低，加载速度变慢，影响用户体验。和当 Model 频繁发生变化，开发者需要主动更新到 View



#### 双向绑定原理

vue.js 是采用数据劫持结合发布者-订阅者模式的方式，通过 Object.defineProperty()来劫持各个属性的 setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。

第一步：需要 observe 的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter 和 getter 这样的话，给这个对象的某个值赋值，就会触发 setter，那么就能监听到了数据变化

第二步：compile 解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图

第三步：Watcher 订阅者是 Observer 和 Compile 之间通信的桥梁，主要做的事情是:

- 在自身实例化时往属性订阅器(dep)里面添加自己
- 自身必须有一个 update()方法
- 待属性变动 dep.notice()通知时，能调用自身的 update() 方法，并触发 Compile 中绑定的回调，则功成身退。

第四步：MVVM 作为数据绑定的入口，整合 Observer、Compile 和 Watcher 三者，通过 Observer 来监听自己的 model 数据变化，通过 Compile 来解析编译模板指令，最终利用 Watcher 搭起 Observer 和 Compile 之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据 model 变更的双向绑定效果

#### 路由的实现原理

hash模式：在浏览器中符号“#”，用window.location.hash读取

特点：hash虽然在URL中，但不被包括在HTTP请求中；用来指导浏览器动作，对服务端安全无用，hash不会重加载页面。仅 hash 符号之前的内容会被包含在请求中，如 http://www.xxx.com，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 404 错误。

history模式：history采用HTML5的新特性；且提供了两个新方法：pushState（），replaceState（）可以对浏览器历史记录栈进行修改，以及popState事件的监听到状态变更。

特点：前端的 URL 必须和实际向后端发起请求的 URL 一致，如 http://www.xxx.com/items/id。后端如果缺少对 /items/id 的路由处理，将返回 404 错误。你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。

#### 框架处理事件原理

React 的事件是合成事件，React 在组件加载(mount)和更新(update)时，将事件通过 addEventListener  统一注册到 document 上，然后会有一个事件池存储了所有的事件，当事件触发的时候，通过 dispatchEvent 进行事件分发

#### 高阶组件(higher order component)

高阶组件是一个以组件为参数并返回一个新组件的函数。HOC 运行你重用代码、逻辑和引导抽象。最常见的可能是 Redux 的 connect 函数。除了简单分享工具库和简单的组合，HOC 最好的方式是共享 React 组件之间的行为。如果你发现你在不同的地方写了大量代码来做同一件事时，就应该考虑将代码重构为可重用的 HOC。

#### axios底层原理

axios其实就是在ajax的基础上加了promise，具体如下：

```
const myAxios = {
        get: function(url) {
            return new Promise((resolve, reject) => {
                var xhr = new XMLHttpRequest();
                xhr.open('GET', url, true);
                xhr.onreadystatechange = function() {
                    if (xhr.readyState == 4 && xhr.status == 200) {
                        resolve(xhr.responseText)
                    }
                };
                xhr.send();
            })
        }
```









------



### 区别差异不同

#### 业务组件与技术组件

- 即 UI 组件和容器组件。
- UI 组件负责 UI 的呈现，容器组件负责管理数据和逻辑。
- 两者通过 React-Redux 提供 connect 方法联系起来。

#### gulp和webpack的区别

gulp可以进行js，html，css，img的压缩打包，是自动化构建工具，可以将多个js文件或是css压缩成一个文件，并且可以压缩为一行，以此来减少文件体积，加快请求速度和减少请求次数；并且gulp有task定义处理事务，从而构建整体流程，它是基于流的自动化构建工具。

Webpack是前端构建工具，实现了模块化开发和文件处理。他的思想就是“万物皆为模块”，它能够将各个模块进行按需加载，不会导致加载了无用或冗余的代码。所以他还有个名字叫前端模块化打包工具。

#### get和post区别

- GET和POST本质上没有区别

- GET/POST都是TCP链接。GET和POST能做的事情是一样一样的。你要给GET加上request body，给POST带上url参数，技术上是完全行的通的。 

- 对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；

  而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）
