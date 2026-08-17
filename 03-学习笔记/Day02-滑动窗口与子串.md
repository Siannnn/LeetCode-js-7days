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
- **是否独立做出：** 看题解
- **题型/模板：** 滑动窗口
- **核心思路：** 用数组存储该字符串26个字母出现次数，窗口向前移动的时候，对左边移开的字符进行--，对右边新进的字符进行++，然后比较是否相等
- **JavaScript 实现：**
```js
var findAnagrams = function(s, p) {

    let ans=[];

    let sLen=s.length,pLen=p.length;

    if(sLen<pLen) return []

    const need = new Array(26).fill(0);

    const window = new Array(26).fill(0);

  

    const a='a'.charCodeAt(0);

    for(let i=0;i<pLen;i++){

        need[p.charCodeAt(i)-a]++;

        window[s.charCodeAt(i)-a]++;

    }

    const equal=()=>{

      return  window.every((v,i)=> v===need[i]);

    }

    if(equal()) ans.push(0);

    for(let i=pLen;i<sLen;i++){

        window[s.charCodeAt(i)-a]++;

        window[s.charCodeAt(i-pLen)-a]--;

        if(equal()) ans.push(i-pLen+1);

    }

  

    return ans;

};
```
- **复杂度：** O(_n_) / O(_1_)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习
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
- **是否独立做出：**  看题解
- **题型/模板：** 滑动窗口
- **核心思路：** 利用map存储字符出现次数。right循环，有该值则放入窗口，窗口中该字符达次数则valid++；valid达标后移动左指针，先进行length初始化、记录start值，对回缩的该值进行判断处理。最后对字符进行slice
- **JavaScript 实现：**
```js
var minWindow = function(s, t) {

    let left=0,right,valid=0,start;

    let need=new Map(),length=Infinity;

    let window=new Map();

    for(const x of t){

        need.set(x,(need.get(x)||0)+1);

    }

    for(right=0;right<s.length;right++){

        if(need.has(s[right])){

            window.set(s[right],(window.get(s[right])||0)+1);

            if(window.get(s[right])===need.get(s[right])){

                valid++;

            }

        }

        while(valid===need.size){

            if(right-left+1<length){

                length=right-left+1;

                start=left;

            }

  

            const d=s[left];

            left++;

            if(need.has(d)){

                if(window.get(d)===need.get(d))

                valid--;

                 window.set(d,window.get(d)-1);

            }

        }

    }

    return length===Infinity?"" :s.slice(start,start+length);

};
```
- **复杂度：** O(n) / O(_1_)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** ❌ 未掌握
- **感悟/易错点：** 注意length复制为infinity，注意赋值(window.get(x)||0 +1)

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
- **是否独立做出：** 看题解
- **题型/模板：** 排序+一次扫描
- **核心思路：** 将数组按起始点排序后，一次扫描，判断是直接进入ans还是更改已有的结束位置
- **JavaScript 实现：**
```js
var merge = function(intervals) {
    // 按区间起点升序排序（这是关键前提）
    intervals.sort((a, b) => a[0] - b[0]);
    const merged = [];

    for (const interval of intervals) {
        // 如果 merged 为空，或者当前区间与 merged 最后一个区间不重叠
        if (merged.length === 0 || interval[0] > merged[merged.length - 1][1]) {
            merged.push(interval);
        } else {
            // 重叠：合并区间--更新终点为较大值
            const last = merged[merged.length - 1];
            last[1] = Math.max(last[1], interval[1]);
        }
    }

    return merged;
};
```
- **复杂度：** O(n log n) / O(_n_)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习 
- **感悟/易错点：** 合并区间主要是比较后一个数组的起始点与前一个数组的末尾的关系即可。


- 双指针+排序
- 思路：循环里判断是否有重叠，有则更新end；无则将区间push进数组，然后更新start、end
- 易错点：记得判断数组长度

``` js
 let ans=[];

    if(intervals.length===0 ) return [];

    intervals.sort((a,b)=>a[0]-b[0]);

    let [start,end]=intervals[0];

    for(let i=1;i<intervals.length;i++){

        if(intervals[i][0]<=end){

            end=Math.max(intervals[i][1],end);

        }else{

            ans.push([start,end]);

            [start,end]=intervals[i];

        }

    }

    ans.push([start,end]);

    return ans;
```

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
