[toc]

### 算法篇
#### 深度优先遍历和广度优先遍历，如何实现？
**深度优先遍历DFS**
先访问根节点，然后遍历左子树，再遍历右子树。当节点v的所有边都已被探寻过，搜索将回溯到发现节点v的那条边的起始节点。这一过程一直进行到已发现从根节点可达的所有节点为止。若还存在未被发现的节点，则选择其中一个作为源节点并重复以上过程，整个进程反复进行直到所有节点都被访问为止。利用堆栈（先进后出）的先进后出。

1. 访问根节点v
2. 依次从v的未被访问的邻接点出发，进行深度优先遍历，直到v的所有连接点都被访问过
3. 若此时尚有顶点未被访问，则从一个未被访问的顶点触发，重新进行深度优先遍历，直到所有顶点均被访问过为止。

以DOM树遍历查找为例
```js
let deepTraversal1 = (node, nodeList = []) {
    if (node !== null) {
        nodeList.push(node);
        let children = node.children;
        for (let i = 0; i < children.length; i++) {
            deepTraversal1(children[i], nodeList);
        }
    }
    return nodeList;
}
let deepTraversal2 = (node) => {
    let nodes = [];
    if (node !== null) {
        nodes.push(node);
        let children = node.children;
        fot (let i = 0; i < children.length; i++) {
            nodes = nodes.concat(deepTraversal2(children[i]));
        }
    }
    return nodes;
}
// 非递归
let deepTraversal3 = (node) => {
    let stack = [];
    let nodes = [];
    if (node) {
        stack.push(node);
        while(stack.length) {
            let item = stack.pop();
            let children = item.children;
            nodes.push(item);
            for (let i = children.length - 1; i >= 0; i--) {
                stack.push(children[i]);
            }
        }
    }
    return nodes;
}
```


**广度优先遍历BFS**
从根节点出发，沿着树的宽度遍历节点，如果所有节点均被访问过，则算法终止。如果此时还有节点未被访问过，则需要另选一个未曾被访问过的节点作为新的起始点，直到所有节点都被访问过。一般用队列（先进先出）来辅助实现。
```js
let widthTraversal = (node) => {
    let nodes = [];
    let stact = [];
    if (node) {
        stack.push(node);
        while (stack.length) {
            let item = stack.shift();
            let children = item.children;
            nodes.push(item);
            for (let i = 0; i < children.length; i++) {
                stack.push(children[i]);
            }
        }
    }
    return nodes;
}
```