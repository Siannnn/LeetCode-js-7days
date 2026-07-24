# Day 01 · 哈希与双指针

> **日期：** 2026-07-24  
> **学习目标：** 哈希表的 Map/Set 使用技巧与双指针经典模式  
> **默认语言：** JavaScript（TypeScript 只作类型辅助）  
> **相关知识页：** [[02-Wiki/专题总结/01-哈希表]] · [[02-Wiki/专题总结/02-双指针与滑动窗口]]

---

## 一、今日 JS/TS 模板回顾

### 哈希表：值 → 下标映射
```js
const seen = new Map();

for (let i = 0; i < nums.length; i++) {
  const need = target - nums[i];
  if (seen.has(need)) return [seen.get(need), i];
  seen.set(nums[i], i);
}
```

### 双指针（对撞）
```js
let left = 0;
let right = nums.length - 1;

while (left < right) {
  const sum = nums[left] + nums[right];
  if (sum === target) return [left, right];
  if (sum < target) left++;
  else right--;
}
```

### 双指针（快慢）
```js
let slow = 0;

for (let fast = 0; fast < nums.length; fast++) {
  if (nums[fast] !== 0) {
    [nums[slow], nums[fast]] = [nums[fast], nums[slow]];
    slow++;
  }
}
```

---

## 二、做题记录

### 1. 两数之和（Easy）
- **是否独立做出：** 独立
- **题型/模板：** 哈希表 map用法
- **核心思路：** map判断是否含有目标值-当前值、map的key为数组值 值为索引下标
- **JavaScript 实现：**
```js
var twoSum = function(nums, target) {

const mp=new Map();

for(let i=0;i<nums.length;i++){

const x=nums[i];

if(mp.has(target-x)) return [mp.get(target-x),i]

mp.set(x,i);

}

return []

};
```
- **复杂度：** O(n) / O(__)
- **JS 写法注意点：** mp.get()获得的是键对应的🈯️、mp.set()
- **从 Python 题解转写时的差异：**
- **掌握程度：** ✅ 熟练
- **感悟/易错点：**

### 2. 字母异位词分组（Medium）
- **是否独立做出：** 看提示 
- **题型/模板：** 哈希
- **核心思路：** 将每个字符串sort后置入map。字符串先split后sort
- **JavaScript 实现：**
```js
var groupAnagrams = function(strs) {

const mp=new Map();

for(const x of strs){

let m=x.split('').sort().join("#");

if(!mp.has(m)) mp.set(m,[]);

mp.get(m).push(x);

}

return [...mp.values()];

};
```
- **复杂度：** O(n) 
- **JS 写法注意点：** 刚开始map无值时，需初始化为[]：mp.set(m,[])
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习
- **感悟/易错点：** 最后输出一个数组，每个值是一个同字母的数组，所以需要展开`[...mp.values()]

### 3. 最长连续序列（Medium）
- **是否独立做出：**  看题解
- **题型/模板：**  set 
- **核心思路：** 每个元素作为起始值看，是起始值则接着循环
- **JavaScript 实现：**
```js

var longestConsecutive = function(nums) {
    let set=new Set(nums);
    let ans=0;
    for(const x of set){
        if(!set.has(x-1)) {
            let curr=x;
            let len=1;
            while(set.has(++curr)){
                len++;
            }
            ans=Math.max(len,ans);
        }
        

    }
    return ans;
};
```
- **复杂度：** O(n^2) 
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：**  🔄 需复习 
- **感悟/易错点：** 数组需是连续的值

### 和为k的子数组
- **是否独立做出：**  提示
- **题型/模板：** 前缀和+哈希
- **核心思路：** 找是否有当前前缀和-k的其他前缀和。mp的键是前缀和，值是该前缀和数量。
- **JavaScript 实现：**
```js
var subarraySum = function(nums, k) {
    let prefix=0;
    let ans=0;
    let mp=new Map();
    mp.set(0,1);
    for(const x of nums){
        prefix+=x;
        
        if(mp.has(prefix-k)){
            ans+=mp.get(prefix-k);
        }
        mp.set(prefix,(mp.get(prefix)??0)+1);
    }
    return ans;
};
```
 - **复杂度：** O(n) 
- **JS 写法注意点：** mp.get()可能为undefined需要??0
- **从 Python 题解转写时的差异：**
- **掌握程度：**  🔄 需复习 
- **感悟/易错点：** 数组可能有负数，所以前缀和可能会有相同的值，所以map值需是前缀和数量
### 4. 移动零（Easy）
- **是否独立做出：** 独立 / 看提示 / 看题解
- **题型/模板：**
- **核心思路：**
- **JavaScript 实现：**
```js

```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** ✅ 熟练 / 🔄 需复习 / ❌ 未掌握
- **感悟/易错点：**

### 5. 盛最多水的容器（Medium）
- **是否独立做出：** 独立 / 看提示 / 看题解
- **题型/模板：**
- **核心思路：**
- **JavaScript 实现：**
```js

```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** ✅ 熟练 / 🔄 需复习 / ❌ 未掌握
- **感悟/易错点：**

### 6. 三数之和（Medium）
- **是否独立做出：** 独立 / 看提示 / 看题解
- **题型/模板：**
- **核心思路：**
- **JavaScript 实现：**
```js

```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** ✅ 熟练 / 🔄 需复习 / ❌ 未掌握
- **感悟/易错点：**

### 7. 接雨水（Hard）
- **是否独立做出：** 独立 / 看提示 / 看题解
- **题型/模板：**
- **核心思路：**
- **JavaScript 实现：**
```js

```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** ✅ 熟练 / 🔄 需复习 / ❌ 未掌握
- **感悟/易错点：**

---

## 三、今日总结

**学到的新模板/技巧：**
-

**遇到的困难：**
-

**需要加入 JS/TS 模板库的内容：**
-

**遗留问题（需复习）：**
-

**整体感受：** 😊 😐 😢
