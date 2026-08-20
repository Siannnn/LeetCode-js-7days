# Day 05 · 链表进阶

> **日期：** 2026-07-28  
> **学习目标：** 链表反转、合并、环检测与复杂指针  
> **默认语言：** JavaScript（TypeScript 只作类型辅助）  
> **相关知识页：** [[02-Wiki/专题总结/04-链表]]

---

## 一、今日 JS/TS 模板回顾

### 反转链表
```js
let prev = null;
let cur = head;

while (cur) {
  const next = cur.next;
  cur.next = prev;
  prev = cur;
  cur = next;
}

return prev;
```

### 快慢指针找环
```js
let slow = head;
let fast = head;

while (fast && fast.next) {
  slow = slow.next;
  fast = fast.next.next;
  if (slow === fast) break;
}
```

---

## 二、做题记录

### K 个一组翻转
- **是否独立做出：** 看题解
- **题型/模板：** 虚拟头结点+分组翻转
- **核心思路：** 
- **JavaScript 实现：**
```js
var reverseKGroup = function(head, k) {
    // 迭代法：虚拟头节点 + 分组翻转 + 组间重连
    if (!head || k === 1) return head;

    const dummy = new ListNode(0, head);
    let pre = dummy;  // 上一组的末尾（也是当前组的前驱）

    while (true) {
        // 1. 检查是否有 K 个节点
        let cur = pre;
        for (let i = 0; i < k; i++) {
            cur = cur.next;
            if (!cur) return dummy.next;  // 不足 K 个，结束
        }

        // 2. 翻转 K 个节点
        const groupHead = pre.next;   // 当前组的第一个节点（将成为最后一个）
        const nextGroup = cur.next;   // 下一组的头节点

        // 翻转 [groupHead, cur] 这一段
        let prev = null, curr = groupHead;
        for (let i = 0; i < k; i++) {
            const nxt = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nxt;
        }
        // 翻转后 prev 是新头，groupHead 是尾

        // 3. 组间重连
        pre.next = prev;              // 前驱指向新头
        groupHead.next = nextGroup;    // 当前组尾指向下一组

        pre = groupHead;  // pre 移到当前组尾（即下一组的前驱）
    }
};
```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** ✅ 熟练 / 🔄 需复习 / ❌ 未掌握
- **感悟/易错点：**

### LRU 缓存
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

### 1. 两两交换链表中的节点（Medium）
- **是否独立做出：**  看题解
- **题型/模板：** 虚拟头结点+交换
- **核心思路：** 遍历链表，交换俩个后新的循环移动pre
- **JavaScript 实现：**
```js
var swapPairs = function(head) {
    let dummy=new ListNode(0,head);
    let pre=dummy;

    while(pre.next&&pre.next.next){
        let p1=pre.next,p2=p1.next;
        pre.next=p2;
        p1.next=p2.next;
        p2.next=p1;

        pre=p1;
    }
    return dummy.next;
};
```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习 
- **感悟/易错点：** 循环需要先判断有俩个结点可以交换

### 2. K 个一组翻转链表（Hard）
- **是否独立做出：** 看题解
- **题型/模板：**
- **核心思路：** 虚拟头结点+单个组翻转+该链连接在整个链中
- **JavaScript 实现：**
```js
var reverseKGroup = function(head, k) {
    if (!head || k === 1) return head;
    let dummy=new ListNode(0,head);
    let pre=dummy;
    let curr;

    while(true){
        curr=pre;

        for(let i=0;i<k;i++){
            curr=curr.next;
            if (!curr) return dummy.next;  // 不足 K 个，结束
        }

        let nextGroup=curr.next;
        let groupHead=pre.next;
        let prev=null;
        curr=pre.next;

        for (let i = 0; i < k; i++) {
            let tmp=curr.next;
            curr.next=prev;
            prev=curr;
            curr=tmp;
        }
        pre.next=prev;
        groupHead.next=nextGroup;
        pre=groupHead;

    }
};
```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：**  ❌ 未掌握
- **感悟/易错点：** 虚拟头结点指向反转后每组第一个数；翻转前先找到每组头结点和下一个组头结点，在反转后将每组的头结点连接下一个组头结点；然后更新pre

### 3. 随机链表的复制（Medium）
- **是否独立做出：** 看题解
- **题型/模板：** 克隆节点
- **核心思路：** 遍历链表，插入克隆节点、对克隆节点进行random拷贝、拆分俩条链表
- **JavaScript 实现：**
```js
var copyRandomList = function(head) {
    // 三步法：插入克隆 -> 设置 random -> 拆分（O(1) 空间）
    if (!head) return null;

    // 第一步：在每个原节点后插入克隆节点
    let cur = head;
    while (cur) {
        const clone = new Node(cur.val);
        clone.next = cur.next;
        cur.next = clone;
        cur = clone.next;   // 跳到原链表的下一个节点
    }

    // 第二步：设置克隆节点的 random 指针
    cur = head;
    while (cur) {
        if (cur.random) {
            // 关键！原节点.random 的 next 就是对应的克隆节点
            cur.next.random = cur.random.next;
        }
        cur = cur.next.next;   // 一次跳两步，跳过克隆节点
    }

    // 第三步：拆分链表
    cur = head;
    const newHead = head.next;   // 保存克隆链表的头
    while (cur) {
        const clone = cur.next;
        cur.next = clone.next;   // 恢复原链表的 next
        if (clone.next) {
            clone.next = clone.next.next;  // 连接克隆链表的 next
        }
        cur = cur.next;   // cur 已经是原链表的下一个节点
    }

    return newHead;
};
```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** ❌ 未掌握
- **感悟/易错点：** 依旧开头先判空；对random、next的判断；

### 4. 排序链表（Medium）
- **是否独立做出：** 看题解
- **题型/模板：** 归并排序
- **核心思路：** 快慢指针找中点后将链表拆断；归并左右比较curr连接
- **JavaScript 实现：**
```js
var sortList = function(head) {
    if (!head || !head.next) return head;

    let slow=head,fast=head.next;
    while(fast&&fast.next){
        slow=slow.next;
        fast=fast.next.next;
    }

    let mid=slow.next;
    slow.next=null;

    let left=sortList(head);
    let right=sortList(mid);

    let dummy=new ListNode(0);
    let curr=dummy;
    while(left&&right){
        if(left.val<=right.val){
            curr.next=left;
            left=left.next;

        }else{
            curr.next=right;
            right=right.next;
        }

        curr=curr.next;
    }

    curr.next=left||right;
    return dummy.next;

};
```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习
- **感悟/易错点：** 开头判断链表空、开始快指针在慢指针前面一步可以确保最后慢指针落在左段链表末尾、循环连接链表时`(left&&right)` ，最后需要拼接剩余的链，即`curr.next=left||right` 

### 5. 合并 K 个升序链表（Hard）
- **是否独立做出：** 看题解
- **题型/模板：** 归并排序+分治
- **核心思路：** 每次分出左右俩边，进行归并排序
- **JavaScript 实现：**
```js
var mergeKLists = function(lists) {
    if(!lists||lists.length==0) return null;
	if (lists.length === 1) return lists[0];
    let mid=lists.length>>1;
    let left=mergeKLists(lists.slice(0,mid));
    let right=mergeKLists(lists.slice(mid));
    return mergeTwoList(left,right);
};

  
var mergeTwoList=function(l,r){
    let dummy=new ListNode(0);
    let curr=dummy;

    while(l&&r){
        if(l.val<=r.val){
            curr.next=l;
            l=l.next;
        }else{
            curr.next=r;
            r=r.next;
        }
        curr=curr.next;
    }
    curr.next=l||r;

    return dummy.next;

}
```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：**  ❌ 未掌握
- **感悟/易错点：**

### 6. LRU 缓存（Medium）
- **是否独立做出：** 看提示
- **题型/模板：** 哈希存储值
- **核心思路：**
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
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习
- **感悟/易错点：** 构造器this声明，而不是const+` this.cache.keys().next().value;` 
	- const、let是函数的私有变量；this.xxx是实例的公开属性——故原型方法只能通过this拿到属性

|      | `const/let cap` | `this.cap`          |
| ---- | --------------- | ------------------- |
| 住在哪  | 构造函数的**局部作用域**  | 创建出的**实例对象**上       |
| 谁看得见 | 只有构造器**内部**的代码  | 任何拿到实例的人（`obj.cap`） |
| 生命周期 | 构造器执行完就没了       | 跟实例一起存在             |

---

## 三、今日总结

**学到的新模板/技巧：**
- 链表反转：正常遍历更改两节点之间指向即可
- 交换节点：虚拟节点，循环pre
	- 多个交换的话：需要在整个链表中连接串起来
- 链表复制：深拷贝——节点克隆，二次遍历random赋值、三次剥离
- 链表排序：归并排序——分左右链表、迭代
	- 归并排序： `left=merge();right=merge(); merge(left,right)=>{...return dummy.next;}`

- 实例的this性质，实例构造器不应用const 、let声明。This是原型所可以访问到一个实例属性

**遇到的困难：**
- 经常忘记边界场景的判断`!head || !head.next` 之类
- 归并排序不熟悉
- 迭代不擅长

**需要加入 JS/TS 模板库的内容：**
-

**遗留问题（需复习）：**
-

**整体感受：** 😊 
