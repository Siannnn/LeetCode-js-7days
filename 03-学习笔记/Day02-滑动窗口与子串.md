# Day 02 · 滑动窗口与子串

> **日期：** 2026-07-25  
> **学习目标：** 滑动窗口、子串问题与字符频次维护  
> **默认语言：** JavaScript（TypeScript 只作类型辅助）  
> **相关知识页：** [[02-Wiki/专题总结/02-双指针与滑动窗口]] · [[02-Wiki/专题总结/01-哈希表]]

---

## 一、今日 JS/TS 模板回顾

### 滑动窗口：Map 维护窗口状态
```js
let left = 0;
const window = new Map();
let ans = 0;

for (let right = 0; right < s.length; right++) {
  const inChar = s[right];
  window.set(inChar, (window.get(inChar) ?? 0) + 1);

  while (/* 窗口不合法 */) {
    const outChar = s[left++];
    window.set(outChar, window.get(outChar) - 1);
    if (window.get(outChar) === 0) window.delete(outChar);
  }

  ans = Math.max(ans, right - left + 1);
}
```

### 定长字母频次数组
```js
const freq = Array(26).fill(0);
for (const ch of s) freq[ch.charCodeAt(0) - 97]++;
```

---

## 二、做题记录

### 前缀和
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

### 1. 无重复字符的最长子串（Medium）
- **是否独立做出：** 看题解
- **题型/模板：** 滑动窗口
- **核心思路：** 外层右指针循环，内层判断左指针。map存储指针所在下标，由于不删除所以判断要加索引值大小的判断
- **JavaScript 实现：**
```js
var lengthOfLongestSubstring = function(s) {

let mp=new Map();

let left=0,ans=0;

for(let right=0;right<s.length;right++){

if(mp.has(s[right])&& mp.get(s[right])>=left){

left=mp.get(s[right])+1;

}

mp.set(s[right],right);

ans=Math.max(ans,right-left+1);

  

}

return ans;

};
```
- **复杂度：** O(n) 
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习 
- **感悟/易错点：** map存储指针下标，未在窗口移动时进行删除，所以判断窗口移动时需要加下标值判断。

### 2. 找到字符串中所有字母异位词（Medium）
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

### 3. 和为 K 的子数组（Medium）
- **是否独立做出：** 看提示 
- **题型/模板：** 前缀和
- **核心思路：** map存储每个前缀和出现的次数
- **JavaScript 实现：**
```js
var subarraySum = function(nums, k) {

let map=new Map();

let pre=0,ans=0;

map.set(0,1);

for(let right=0;right<nums.length;right++){

pre+=nums[right];

if(map.has(pre-k)){

ans+=map.get(pre-k);

}

map.set(pre,(map.get(pre)??0)+1);

}

return ans;

};
```
- **复杂度：** O(_n_) 
- **JS 写法注意点：** 初始化：有一个前缀和为0的出现了一次：map.set(0,1)
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习 
- **感悟/易错点：** map初始化、map存储出现次数

### 5. 最小覆盖子串（Hard）
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

### 6. 最大子数组和（Medium）
- **是否独立做出：** 看提示 
- **题型/模板：** 动态规划简易版
- **核心思路：**维护“以当前位置结尾的最大子数组和”，如果前面的和是负贡献，就从当前元素重新开始
- **JavaScript 实现：**
```js
var maxSubArray = function(nums) {

let ans=-Infinity;

let curr=0;

for(const x of nums){

if(curr+x<x){

curr=0;

}

curr+=x;

ans=Math.max(ans,curr);

}

return ans;

};
```
- **复杂度：** O(_n_) 
- **JS 写法注意点：** 初始值为-Infinity
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习
- **感悟/易错点：** 比较前缀和+当前值和仅当前值的大小关系，判断当前值是否为子串第一个值

### 7. 合并区间（Medium）
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
