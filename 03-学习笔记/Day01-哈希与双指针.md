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
- **是否独立做出：** 看题解
- **题型/模板：** 快慢指针
- **核心思路：** 快指针外层循环，遇到非零则交换快慢，然后slow++
- **JavaScript 实现：**
```js
let slow=0;
for(let fast=0;fast<nums.length;fast++){
	if(nums[fast]!==0){
		[nums[slow],nums[fast]]=[nums[fast],nums[slow]];
		slow++；
	}
}
```
- **复杂度：** O(n__) / O(1__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：**  ❌ 未掌握
- **感悟/易错点：** 不知道在循环里怎么描述，应该是快指针非零则交换，为零的话则快指针前进，慢指针停留在为0的位置。

### 5. 盛最多水的容器（Medium）
- **是否独立做出：** 看题解
- **题型/模板：** 双指针
- **核心思路：** 双指针在首尾，比较左右指针指向大小，小的指针进行移动
- **JavaScript 实现：**
```js
let left=0,right=height.length-1;

    let ans=0;

    while(left<right){

        let h=Math.min(height[left],height[right]);

        let area=h*(right-left);

        if(height[left]<height[right]) left++;

        else right--;

        ans=Math.max(ans,area);

    }

  

    return ans;
```
- **复杂度：** O(_n_) / O(_1_)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习 
- **感悟/易错点：**

### 6. 三数之和（Medium）
- **是否独立做出：** 看题解
- **题型/模板：** 去重+双指针
- **核心思路：** 先排序， 固定第一个数，左指针：i+1，右指针：length-1 。第一个数外层循环，里面左右指针循环，同时对左右指针进行去重
- **JavaScript 实现：**
```js
var threeSum = function(nums) {

    let left,right=nums.length-1;

    nums.sort((a,b)=>a-b);

    let ans=[];

    for(let i=0;i<nums.length-2;i++){

        if(nums[i]>0) break;

        if(i>0&& nums[i]===nums[i-1]) continue;

        left=i+1;

        right=nums.length-1;

        while(left<right){

            let sum=nums[i]+nums[right]+nums[left];

            if(sum===0){

                ans.push([nums[i],nums[left],nums[right]]);

                left++;

                right--;

                while(left<right && nums[left]===nums[left-1]) left++;

                while(left<right && nums[right]===nums[right+1]) right--;

            }else if(sum<0)left++;

            else right--;

        }

    }

  

    return ans;

};
```
- **复杂度：** O(_n^2_) / O(_1_)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** ❌ 未掌握
- **感悟/易错点：** 外层循环内对第一个数去重（即比较当前数与上一个数的大小是否相同，相同则跳过）、由于排序可以通过第一个数>0进行break；left< right 循环中，sum=0要else if链接。找到数后要收缩左右指针，然后还得去重。
	- 去重即比较当前数与上一个数的大小关系，然后进行指针挪动

### 7. 接雨水（Hard）
- **是否独立做出：** 看题解
- **题型/模板：** 双指针
- **核心思路：** 能接的雨水只与离自己近的那个最高点相关。所以左右指针，得到当前左右的最大值，再根据这俩个最大值进行回缩
- **JavaScript 实现：**
```js
var trap = function(height) {

    let left=0,right=height.length-1;

    let leftMax=0,rightMax=0,ans=0;

    while(left<right){

        leftMax=Math.max(leftMax,height[left]);

        rightMax=Math.max(rightMax,height[right]);

        if(leftMax<rightMax){

            ans+=leftMax-height[left];

            left++;

        }

        else{

            ans+=rightMax-height[right];

            right--;

        }

    }

    return ans;

};
```
- **复杂度：** O(_n_) / O(_1_)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：**  ❌ 未掌握
- **感悟/易错点：**


- 单调栈写法
- stack存储下标，下标是高度的递减。即栈顶是最先进去的，最高的；栈低是最近进去的可pop的，高度最低的
``` js
const stack=[];

   let ans=0;

   for(let i=0;i<height.length;i++){

        while(stack.length && height[i]>height[stack[stack.length-1]]){

            const bottom=stack.pop();

            if(!stack.length) break;

            const left=stack[stack.length-1];

            const wid=i-left-1;

            const h=Math.min(height[left],height[i])-height[bottom];

            const area=h*wid;

            ans+=area;

        }

        stack.push(i);

   }

   return ans;

```
- 注意点：
	- while循环中，只有比凹槽大的会进入循环，pop出最低点后，拿到左侧最低点，比较左右哪个最低后计算出水的高度。然后该最低点的水平面即算结束了，一层一层进行计算的。
- 易错： pop出凹槽之后需要判断栈的长度是否要break
- **复杂度：** O(_n_) / O(_n_)

### 8. LRU缓存（medium）
- **是否独立做出：**  看题解
- **题型/模板：** 哈希表+双向链表
- **核心思路：** lru：最近最少使用。双向链表的尾结点即最近最少使用。可使用map特性：get时判断是否有该key，然后进行移动至头结点；put时也判断是否已有，是否超缓存大小。
		如果手写map的特性、双向链表，cache存储key和对应node，需实现添加、删除结点，移动至头部，和删除尾结点。
- **JavaScript 实现：**
```js
class LRUCache {
    /** 基于 JS Map（保持插入顺序）实现 */
    constructor(capacity) {
        this.capacity = capacity;
        this.cache = new Map();
    }

    get(key) {
        if (!this.cache.has(key)) return -1;
        const value = this.cache.get(key);
        // 删除后重新插入，使其成为最近使用
        this.cache.delete(key);
        this.cache.set(key, value);
        return value;
    }

    put(key, value) {
        // 已存在则先删除，保证重新插入后位于末尾（最近使用）
        if (this.cache.has(key)) {
            this.cache.delete(key);
        }
        this.cache.set(key, value);
        if (this.cache.size > this.capacity) {
            // 移除最久未使用的（Map 的第一个键）
            const oldestKey = this.cache.keys().next().value;
            this.cache.delete(oldestKey);
        }
    }
}
```
- **复杂度：** O(_1_) / O(_1_)
- **JS 写法注意点：** put时没有该key需要new Node()；
- **从 Python 题解转写时的差异：**
- **掌握程度：** ❌ 未掌握
- **感悟/易错点：** 双向链表理清处某些结点之间的关系，指针指向。
---

## 三、今日总结

**学到的新模板/技巧：**
-
哈希与双指针：
	哈希表一般可用于判断是否有某值
		map可存下标
		也可以存储某值出现几次
	双指针分快慢和对撞
		快慢可以快指针循环：内部判断控制慢指针变化
		对撞就是left< right之后， 根据条件进行收缩


**遇到的困难：**
-
- if else缺失找不到问题
- 不知道快慢指针谁循环，谁变化
- 不知道运用啥方法
	- 移动零这种变化两两位置的可以快慢指针
	- 二数之和可以哈希表；三数之和可以对撞指针
- 不知道剪枝
	- 可以提前break、continue的，判断stack大小
	- 排序后可去重+剪枝

**需要加入 JS/TS 模板库的内容：**
-

**遗留问题（需复习）：**
-

**整体感受：** 😐 
