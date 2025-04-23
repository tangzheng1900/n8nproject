 以下是一个用 JavaScript 实现的冒泡排序算法：

```javascript
function bubbleSort(arr) {
    let n = arr.length;
    for (let i = 0; i < n - 1; i++) {
        for (let j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // 交换 arr[j] 和 arr[j + 1]
                let temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
    return arr;
}

// 示例用法:
const array = [64, 34, 25, 12, 22, 11, 90];
console.log(bubbleSort(array));
```

这段代码定义了一个名为 `bubbleSort` 的函数，它接收一个数组并返回排序后的数组。你可以直接运行它来测试排序效果！