以下是用 JavaScript 实现的二叉树深度优先遍历（DFS）算法，包括前序遍历、中序遍历和后序遍历的示例：

```javascript
// 定义二叉树节点
class TreeNode {
    constructor(value) {
        this.value = value;
        this.left = null;
        this.right = null;
    }
}

// 前序遍历
function preOrderTraversal(root) {
    if (root) {
        console.log(root.value); // 访问根节点
        preOrderTraversal(root.left); // 递归遍历左子树
        preOrderTraversal(root.right); // 递归遍历右子树
    }
}

// 中序遍历
function inOrderTraversal(root) {
    if (root) {
        inOrderTraversal(root.left); // 递归遍历左子树
        console.log(root.value); // 访问根节点
        inOrderTraversal(root.right); // 递归遍历右子树
    }
}

// 后序遍历
function postOrderTraversal(root) {
    if (root) {
        postOrderTraversal(root.left); // 递归遍历左子树
        postOrderTraversal(root.right); // 递归遍历右子树
        console.log(root.value); // 访问根节点
    }
}

// 示例用法
const root = new TreeNode(1);
root.left = new TreeNode(2);
root.right = new TreeNode(3);
root.left.left = new TreeNode(4);
root.left.right = new TreeNode(5);

// 深度优先遍历示例
console.log("前序遍历:");
preOrderTraversal(root);

console.log("中序遍历:");
inOrderTraversal(root);

console.log("后序遍历:");
postOrderTraversal(root);
```

在这个示例中，我们首先定义了一个 `TreeNode` 类来表示二叉树的节点。然后实现了三种遍历算法：前序遍历、中序遍历和后序遍历。最后，我们创建了一个简单的二叉树并调用这些遍历函数，输出遍历结果。您可以直接运行这段代码来查看效果！