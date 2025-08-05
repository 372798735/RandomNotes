## js语法
### flat 数组扁平化
定义：flat()是javaScript数组的一个方法，用于将嵌入的数组“展平”(扁平化)，即将多维数组转为一维数组
基本语法：
```js
/**
 * @param {number} depth 可选，指定要提取嵌套数组的结构深度，默认值为1
*/
const newArray = arr.flat([depth])
```
```js
// 基本使用（默认深度为1）
const arr1 = [1, 2, [3, 4]];
console.log(arr1.flat()); // [1, 2, 3, 4]

const arr2 = [1, 2, [3, 4, [5, 6]]];
console.log(arr2.flat()); // [1, 2, 3, 4, [5, 6]]

// 指定展平深度
const arr3 = [1, 2, [3, 4, [5, 6]]];
console.log(arr3.flat(2)); // [1, 2, 3, 4, 5, 6]

const deeplyNested = [1, [2, [3, [4, [5]]]]];
console.log(deeplyNested.flat(3)); // [1, 2, 3, 4, [5]]

// 展平所有嵌套（使用  Infinity）
const arr4 = [1, [2, [3, [4, [5]]]]];
console.log(arr4.flat(Infinity)); // [1, 2, 3, 4, 5]

// 移除数组中的空项
const arr5 = [1, 2, , 4, 5];
console.log(arr5.flat()); // [1, 2, 4, 5]
```

### flatMap()
定义：flatMap()方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。它与 map 和 深度值1的 flat 几乎相同，但 flatMap 通常在合并成一种方法的效率稍微高一些。
```js
// 基本使用
const arr = [1, 2, 3, 4];

const result = arr.flatMap(x => [x, x * 2]);
// 等同于 arr.map(x => [x, x * 2]).flat()

console.log(result); // [1, 2, 2, 4, 3, 6, 4, 8]

// map()和flat()的比较
const arr = [1, 2, 3];

// 使用 map + flat
const mappedAndFlattened = arr.map(x => [x * 2]).flat();
console.log(mappedAndFlattened); // [2, 4, 6]

// 使用 flatMap
const flatMapped = arr.flatMap(x => [x * 2]);
console.log(flatMapped); // [2, 4, 6]

// 处理字符串
const sentences = ["Hello world", "Goodbye moon"];

const words = sentences.flatMap(sentence => sentence.split(' '));
console.log(words); // ["Hello", "world", "Goodbye", "moon"]

// 过滤元素
const numbers = [1, 2, 3, 4, 5];

// 使用 flatMap 实现过滤和映射
const result = numbers.flatMap(num => num % 2 === 0 ? [num * 2] : []);
console.log(result); // [4, 8] (只有偶数被保留并乘以2)
```
注意事项：
1. flatMap()只能展平一层深度，如果需要更深层次的展平，需要使用flat()方法并指定深度。
2. 如果回调函数返回的不是数组，结果会自动包装在数组中
3. flatMap()不会改变原数组，而是返回一个新的数组。
4. flatMap()是ES2019(ES10)中引入的方法，在旧版本浏览器中可能需要 polyfill。