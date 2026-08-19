# Day 03 · 数组与矩阵

> **日期：** 2026-07-26  
> **学习目标：** 普通数组、矩阵遍历、前缀和与原地修改  
> **默认语言：** JavaScript（TypeScript 只作类型辅助）  
> **相关知识页：** [[02-Wiki/专题总结/03-数组与矩阵]]

---

## 一、今日 JS/TS 模板回顾

### 一维数组初始化
```js
const arr = Array(n).fill(0);
```

### 二维数组初始化（避免共享行）
```js
const grid = Array.from({ length: m }, () => Array(n).fill(0));
```
`Array.from` 把“类数组/可迭代对象”转成真数组。`{ length: m }` 是一个只有 length 属性的类数组对象，转换后得到 `[undefined, undefined, ..., undefined]`（m 个）。

第二个参数是**映射函数**：数组的每一项都会经过它处理，用返回值替换原来的 undefined。
这是一个箭头函数，**对每个位置都调用一次**，每次返回一个全新的、填满 0 的长度为 n 的数组。
### 矩阵遍历
```js
for (let i = 0; i < matrix.length; i++) {
  for (let j = 0; j < matrix[0].length; j++) {
    // matrix[i][j]
  }
}
```

---

## 二、做题记录

### 三次翻转法
- **是否独立做出：** 看题解
- **题型/模板：**
- **核心思路：**
- **JavaScript 实现：**
```js
原始数组：     [1, 2, 3, 4, 5, 6, 7]   k=3
整体翻转：     [7, 6, 5, 4, 3, 2, 1]    ← 后面的到了前面
翻转前 k 个：   [5, 6, 7, 4, 3, 2, 1]    ← 恢复前 k 个的顺序
翻转后 n-k 个： [5, 6, 7, 1, 2, 3, 4]    ← 恢复后 n-k 个的顺序
```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：**  🔄 需复习
- **感悟/易错点：**

### 前缀积+后缀积
- **是否独立做出：** 看题解
- **题型/模板：**
- **核心思路：**
- **JavaScript 实现：**
```js
var productExceptSelf = function(nums) {
    const n = nums.length;
    const res = new Array(n).fill(1);

    // 第一次遍历：左边乘积
    // res[i] = nums[0] * nums[1] * ... * nums[i-1]
    let leftProd = 1;
    for (let i = 0; i < n; i++) {
        res[i] = leftProd;
        leftProd *= nums[i];
    }

    // 第二次遍历：右边乘积
    // 将右边乘积乘到 res[i] 上
    let rightProd = 1;
    for (let i = n - 1; i >= 0; i--) {
        res[i] *= rightProd;
        rightProd *= nums[i];
    }

    return res;
};
```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** ✅ 熟练 / 🔄 需复习 / ❌ 未掌握
- **感悟/易错点：**

### 1. 轮转数组（Medium）
- **是否独立做出：** 看题解
- **题型/模板：** 三次轮转法 
- **核心思路：**  
- **JavaScript 实现：**
```js
var rotate = function(nums, k) {
     k%=nums.length;
    if(k==0) return nums;

    const reverse=(l,r)=>{
        while(l<r){
            [nums[l],nums[r]]=[nums[r],nums[l]];
            l++;
            r--;
        }
    }
    reverse(0,nums.length-1);
    reverse(0,k-1);
    reverse(k,nums.length-1);
    return  nums;
}
```
- **复杂度：** O(_n_) / O(_1_)
- **JS 写法注意点：** 
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习
- **感悟/易错点：**  if(k= =0) return nums; 提前返回 

### 2. 除自身以外数组的乘积（Medium）
- **是否独立做出：** 看题解
- **题型/模板：** 前缀积+后缀积
- **核心思路：**  前缀：res[i]=last; last*=nums[i];
- **JavaScript 实现：**
```js
var productExceptSelf = function(nums) {
    const n = nums.length;
    const res = new Array(n).fill(1);

    // 第一次遍历：左边乘积
    // res[i] = nums[0] * nums[1] * ... * nums[i-1]
    let leftProd = 1;
    for (let i = 0; i < n; i++) {
        res[i] = leftProd;
        leftProd *= nums[i];
    }

    // 第二次遍历：右边乘积
    // 将右边乘积乘到 res[i] 上
    let rightProd = 1;
    for (let i = n - 1; i >= 0; i--) {
        res[i] *= rightProd;
        rightProd *= nums[i];
    }

    return res;
};
```
- **复杂度：** O(_n_) / O(_1_)
- **JS 写法注意点：** 若是用俩个数组存储前后缀积，记得清空last的变量。
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习 
- **感悟/易错点：** 结果要先fill(1)初始化

### 3. 缺失的第一个正数（Hard）
- **是否独立做出：**  看题解
- **题型/模板：** 原地哈希
- **核心思路：** 遍历数组，让每个数x，存在x-1的下标里。while循环确保i下标已存储i+1这个数或者该位置没有对应的数了。
- **JavaScript 实现：**
```js
var firstMissingPositive = function(nums) {

    let n=nums.length;
    for(let i=0;i<nums.length;i++){
        while(nums[i]>=1 && nums[i]<=n && nums[nums[i]-1]!==nums[i]){
            let curr=nums[i]-1;
            [nums[i],nums[curr]]=[nums[curr],nums[i]];
        }
    }

    for(let i=0;i<n;i++){
        if(nums[i]!==i+1) return i+1;
    }
    return n+1;

};
```
- **复杂度：** O(_n_) / O(_1_)
- **JS 写法注意点：** for循环中使用while而不是if，因为第一次将nums[i]交换至正确位置后，i索引处的交换过来的值可能不正确，while循环确保i下标下存储对应的值或不再可以进行交换。
	- while循环里写明nums[i]<=n，有等于号，因为下面取下标是num[i]-1，=N可以确保能取到所有下标处
	- nums[nums[i]-1]!= =nums[i] ——阻止遇到重复的值后对i下标处再次进行交换。
- **从 Python 题解转写时的差异：**
- **掌握程度：** ❌ 未掌握
- **感悟/易错点：**

### 4. 旋转图像（Medium）
- **是否独立做出：**  看提示
- **题型/模板：**
- **核心思路：**  旋转90°即：沿主对角线对称，再每行翻转
- **JavaScript 实现：**
```js
var rotate = function(matrix) {
    for(let i=0;i<matrix.length;i++){
        for(let j=0;j<i;j++){
            [matrix[i][j],matrix[j][i]]=[matrix[j][i],matrix[i][j]];
        }
    }
    for(let i=0;i<matrix.length;i++){
        matrix[i].reverse();
    }
    return matrix;
};
```
- **复杂度：** O(_n²_) / O(_1_)
- **JS 写法注意点：** 注意对角线翻转需要j<i ，避免重复
- **从 Python 题解转写时的差异：**
- **掌握程度：**  🔄 需复习 
- **感悟/易错点：**

---

## 三、今日总结

**学到的新模板/技巧：**
- 数组初始化
	- 一维 `Array(n).fill(0)` 
	- 二维 `Array.from({ length: m }, () => Array(n).fill(0)) `
- 三次翻转法：对`[l,r]` 间进行翻转——数组移动k位可以进行三次翻转实现
- 前缀积+后缀积——实现某一位置的其他元素积
- 对称+转置——旋转图像

**遇到的困难：**
-

**需要加入 JS/TS 模板库的内容：**
-

**遗留问题（需复习）：**
-

**整体感受：** 😊 
