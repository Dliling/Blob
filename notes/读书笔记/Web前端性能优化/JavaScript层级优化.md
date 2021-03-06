#### 5.1 Javascript的运行机制

##### 5.1.1 什么是线程

​	任何一个程序，无论是用何种语言的代码，当它在运行的时候，在任务管理器会发现有一个进程在运行着。

​	进程在开启的时候，系统会给他分配一点资源，如内存等，而线程就是在进程的基础上，从进程那里再分派一点资源来运行。线程是进程的一个实体，它只占用了一点运行时的资源，但它同样共享着其所属进程所拥有的全部资源。相比于进程，线程可以更高效地利用CPU资源。

​	进程与进程之间的切换是很耗时的。进程能做的事情线程其实都能做，而且只需要很少的运行资源。线程之间的切换也比进程快的多，大大提高了系统的性能。

​	其实，开的线程实际上还是一个进程。实际上，线程是把CPU的工作时间分成几个片段，一个片段执行一个任务。由于它们之间切换效率较高，在用户看来就是同时工作。

##### 5.1.2 JavaScript执行机制与其他执行机制的异同

​	大多数后台语言的处理方式，当有一个任务或请求时，可以从线程池中取一条线程去运行，当线程处理完请求或操作逻辑后重新放回线程池中。当并发量非常高时，线程就会不够用。

​	JavaScript实际上只有一个线程。以Node为例，JavaScript的处理实际上非常像一位分配任务的领导，当接收到一个任务或请求时，JavaScript引擎会将该任务放入任务队列中，JavaScript的同步主线程仅仅做了一个标识，用来接收及分配任务或请求。这个过程不需要做任何的处理，类似于倾听和记录，这样就可以非常高效地处理任务。在该任务中，任务A在某个时间点开始执行，任务B在另一个时间点开始执行，任务与任务之间不会相互等待阻塞，任务其实几乎是同时进行的，这样就大大加快了程序的执行效率。

​	任务队列与线程最大的区别在于，队列只要内存足够，可以一直堆叠任务。这样会造成很大的内存开销，但如今硬件成本越来越低，增加一点内存就能极大的提高用户体验。

#### 5.2 定时器是一把双刃剑

​	每开一个定时器，就是往任务队列中加一条任务。实际上，定时器本身是属于JavaScript运行的机制，正常情况下并不会对JavaScript性能造成影响。

##### 5.2.1 用好定时器

​	大多数定时器影响性能是因为使用不恰当，特别是当一些定时器内部的处理逻辑是关于DOM结构，或者一些动画实现使浏览器过于频繁地重排、重绘是。

​	如监听鼠标的滑动事件添加动画时，鼠标的一个滑动动作，瞬间会制造上百个定时器。若还有DOM操作引起重排或重绘，则会导致浏览器非常卡顿，甚至崩溃。

```
document.querySelector('.btn').addEventListener('mousemove', function() {
	setInterval(function() {
		// do something
	}, 100)
});
```

##### 5.2.2 如何及时清除定时器

​	不用的内容及时清除，避免重复生产。

```
document.querySelector('.btn').addEventListener('mousemove', function() {
	clearInterval(timer);
	timer = setInterval(function() {
		// do something
	}, 100)
});
```

##### 5.2.3 合理使用CSS3动画

#### 5.3 事件的绑定

##### 5.3.1 多利用事件委托机制

​	若给每一个`li`标签绑定事件，当数量较多时，会开启多个监听，影响页面性能。可以委托给父元素或某一级的祖先元素，由它们的祖先分发、下派时间。

##### 5.3.2 避免重复的事件监听

​	在重新监听事件时，先将原来的监听`remove`掉。

##### 5.3.3 事件冒泡机制

​	从里向外冒泡，与从外向里捕获相比，冒泡中父层的元素永远只有一个，而里层的元素不止一个，从遍历递归的时间上，冒泡更短一些。

#### 5.4 一些优秀的JavaScript层级思想

##### 5.4.1 `jQuery`的`ready`与原生`window.onload`的比较

​	JavaScript加载完就执行，不会等页面渲染完成，若JavaScript在DOM树创建之前就执行了，可能会出现节点未获取到等问题。传统的原生是讲代码写入`window.onload`的回调函数中，等待整个页面加载后执行。但当页面很大时，等页面加载完执行JS需要等待很久，这时页面出现假死状态，用户操作得不到响应。且`window.onload`不能定义多个，否则只有最后一个生效。

​	jQuery的`ready`事件则只加载运行JS需要的DOM结构，JS执行不需要等整体页面加载完成。

##### 5.4.2 MVVM框架的组件的生命周期

##### 5.4.3 变量缓存与私有化

​	在JavaScript中，对象查找属性的过程比较昂贵，特别是在一些本身就大的对象中，如`window`。若多次使用时，可将其赋值给变量进行缓存。

​	全局变量易造成变量污染。好的思路是将变量私有化，只局限在某个作用域内。
