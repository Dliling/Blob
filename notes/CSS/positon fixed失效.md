https://developer.mozilla.org/zh-CN/docs/Web/CSS/position

**fixed**

元素会被移出正常文档流，并不为元素预留空间，而是通过指定元素相对于**屏幕视口**（viewport）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变。打印时，元素会出现在的每页的固定位置。`fixed` 属性会创建新的**层叠上下文**。**当元素祖先的 `transform`, `perspective` 或 `filter` 属性非 `none` 时，容器由视口改为该祖先。**

层叠上下文可以看下张老师的这篇文，很详细~[深入理解CSS中的层叠上下文和层叠顺序](https://www.zhangxinxu.com/wordpress/2016/01/understand-css-stacking-context-order-z-index/)

`position:fixed`多用来做吸顶效果，但是使用的时候要**注意祖先元素的CSS属性**。

若父元素使用了`filter`属性，可以将其用在`html`元素上，就不会影响了。因为`html`元素是相对屏幕视口的。

`html`和`body`两个元素的区别，可以看下[这篇文](http://phrogz.net/css/htmlvsbody.html)

