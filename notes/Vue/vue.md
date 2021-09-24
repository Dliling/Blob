
#### 为什么说Vue是一个渐进式框架

通俗点说，就是想用啥就用啥，不强求。全家桶不必要都用。

#### Vue 和 React的异同点

相同点：都使用了虚拟DOM，组件化开发，都是单向数据流，都支持服务端渲染

不同点：React的JSX，Vue的template；数据变化，React手动setState,Vue由于响应式处理是自动的；React是单向绑定，Vue是双向绑定

#### MVC和MVVM分别是什么？

MVC：Controller将Model的数据展示在View上。Model和View不能直接通信。

​	Model：负责从数据库中取数据

​	View：负责展示数据

​	Controller：用户交互的地方，例如点击事件等

<img width="442" alt="mvc" src="https://user-images.githubusercontent.com/23520842/134677798-9f3106a8-1d4a-49c9-a61f-0795339e3634.png">

MVVM：实现了View和Model的自动同步。

​	Model：数据层

​	View：视图层

​	View-Model：将Model转化成View，通过数据绑定，即将后端传递的数据转化成所看到的页面；将View转化成Model，通过事件监听将所看到的页面转化成后端的数据。

<img width="450" alt="mvvm" src="https://user-images.githubusercontent.com/23520842/134677711-7ece332a-f84a-47ed-a8ce-c695f18bd099.png">


区别：MVVM比MVC精简很多，减少业务与界面的依赖，还解决了数据频繁更新的问题。

Vue不是严格符合MVVM，因为MVVM规定View和Model不能直接通信，但是```ref```可以做到。

#### Vue生命周期

![lifecycle](https://user-images.githubusercontent.com/23520842/134677612-da5ad939-a4d4-467b-84bb-0ebd30cf6c9c.png)


#### 修饰符

1. .lazy：改变输入框的值时value不会改变，当光标离开输入框时，v-model绑定的值value才会改变
2. .trim：首尾去除空格
3. .number：将值转成数字。先输入数字的话，只取前面数字部分，先输入字母的话，修饰符无效
4. .stop：阻止冒泡
5. .capture：事件默认是往外冒泡，这个修饰符时反过来，由外往内捕获
6. .self：只有点击事件绑定的本身才会触发
7. .once：事件只执行一次
8. .prevent：阻止默认事件，如a标签的跳转
9. .native：加在自定义组件的事件上，保证事件能执行
10. .passive：监听滚动时间时，会一直出发onscroll事件，使用这个修饰符，相当于给onscroll事件加了一个.lazy修饰符
11. .camel：确保绑定参数被识别为驼峰写法
12. .sync：父子组件传值，子组件想更新这个值，使用此修饰符可简写

#### 指令

1. v-slot：具名插槽时使用

   ```
   <template v-slot:header></template>
   ```

2. v-pre：跳过这个元素和它的子元素的编译过程。可以用来显示原始Mustache标签。跳过大量没有指令的节点会加快编译。

3. v-cloak：保持在元素上直到关联实例结束编译。和CSS规则如```[v-cloak]{display: none}```一起使用时，可隐藏未编译的Mustache标签直到实例准备完毕

#### 组件之间的传值方式

1. 父组件传值给子组件，子组件通过```props```进行接收
2. 子组件给父组件传值，子组件使用```$emit```+ 事件对父组件进行传值
3. 组件中可以使用```$parent``` 和 ```$children```获取到父组件实例和子组件实例，进而获取数据
4. 使用```$attrs```和```$listeners```，在对一些组件进行二次封装时方便传值
5. 使用```$refs```获取组件实例，进而获取数据
6. 使用Vuex进行状态管理
7. 使用```eventBus```进行跨组件触发事件，进而传递数据
8. 使用```provide```和```inject```,官方建议不要使用
9. 使用浏览器本地缓存，如localStorage
