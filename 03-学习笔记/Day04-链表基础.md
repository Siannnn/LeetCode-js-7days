# Day 04 · 链表基础

> **日期：** 2026-07-27  
> **学习目标：** 虚拟头节点、快慢指针与基础链表操作  
> **默认语言：** JavaScript（TypeScript 只作类型辅助）  
> **相关知识页：** [[02-Wiki/专题总结/04-链表]]

---

## 一、今日 JS/TS 模板回顾

### TypeScript 辅助定义
```ts
class ListNode {
  val: number;
  next: ListNode | null;

  constructor(val = 0, next: ListNode | null = null) {
    this.val = val;
    this.next = next;
  }
}
```

### 虚拟头节点
```js
const dummy = new ListNode(0, head);
let cur = dummy;

while (cur.next) {
  cur = cur.next;
}

return dummy.next;
```

---

## 二、做题记录

### 链表翻转（三指针）
- **是否独立做出：**  看题解
- **题型/模板：**
- **核心思路：** 三指针，pre、curr、tmp，curr的下一个指向pre，然后更新curr和pre的位置
- **JavaScript 实现：**
```js
var reverseList = function(head) {
    let pre=null,curr=head;

    while(curr){
        const tmp=curr.next;
        curr.next=pre;
        pre=curr;
        curr=tmp;
    }

   return pre;
};
```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习 
- **感悟/易错点：**

### 1. 相交链表（Easy）
- **是否独立做出：** 看题解
- **题型/模板：** 双指针等路程法
- **核心思路：** 一方跑完自己的行程，再去另一个的起点；另一方同理。两者行程相同故会在相交点相会。
- **JavaScript 实现：**
```js
var getIntersectionNode = function(headA, headB) {
    let pA=headA,pB=headB;
    while(pA!==pB){
        pA=pA?pA.next:headB;
        pB=pB?pB.next:headA;
    }
    return pB;
};
```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** ✅ 熟练 / 🔄 需复习 / ❌ 未掌握
- **感悟/易错点：**

- 哈希集合法
``` js
var getIntersectionNode = function(headA, headB) {
    // 哈希集合法
    const visited = new Set();
    let cur = headA;
    while (cur) {
        visited.add(cur);
        cur = cur.next;
    }

    cur = headB;
    while (cur) {
        // Set 中对象按引用判等，正好对应"节点引用相同"的题意
        if (visited.has(cur)) return cur;
        cur = cur.next;
    }
    return null;
};
```
### 3. 回文链表（Easy）
- **是否独立做出：** 看题解
- **题型/模板：**
- **核心思路：** 快慢指针找中点，然后翻转后一半后进行比较；递归判断，递归至链表结尾，回溯判断
- **JavaScript 实现：**
```js
 let slow=head,fast=head;
    if (!head || !head.next) return true;
    while(fast&&fast.next ){
        slow=slow.next;
        fast=fast.next.next;
    }
   let curr=slow,prev=null;
    while(curr){
        let temp=curr.next;
        curr.next=prev;
        prev=curr;
        curr=temp;
    }

    while(prev){
        if(prev.val!=head.val) return false;
        prev=prev.next;
        head=head.next;
    }
   return true;
```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：** 
- **从 Python 题解转写时的差异：**
- **掌握程度：** ✅ 熟练 / 🔄 需复习 / ❌ 未掌握
- **感悟/易错点：** 注意判断边界，比如!head||!head.next 提前return掉

- 递归法
``` js
let front=head;

    const dfs=(node)=>{
        if(!node) return true;
        if(!dfs(node.next)) return false;
        if(node.val!==front.val) return false;
        front=front.next;
        return true;
    }
    return dfs(head);
```
- 

### 4. 环形链表（Easy）
- **是否独立做出：** 看题解
- **题型/模板：** Floyd 判圈算
- **核心思路：** 快慢指针，如果有环形，两者必会相遇
- **JavaScript 实现：**
```js
var hasCycle = function(head) {
    if(!head||!head.next) return false;
    let slow=head,fast=head;

    while(fast&&fast.next){
        fast=fast.next.next;
        slow=slow.next;
        if(fast===slow) return true;
    }

    return false;

};
```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习 
- **感悟/易错点：** 边界判断——`!head ||!head.next `  、循环需要判断`fast&&fast.next` 。即在使用x.next时需写明x非undefined

- 哈希集合写法
```js
var hasCycle = function(head) {
    // 哈希集合法：Set 对节点对象按引用判等，第二次访问即有环
    const visited = new Set();
    let cur = head;
    while (cur) {
        if (visited.has(cur)) return true;
        visited.add(cur);
        cur = cur.next;
    }
    return false;
};
```
### 5. 环形链表 II（Medium）
- **是否独立做出：** 看题解
- **题型/模板：** 快慢指针
- **核心思路：** 从相遇点继续走到环入口的距离=从起点到环入口的距离
- **JavaScript 实现：**
```js
var detectCycle = function(head) {
    let slow=head,fast=head;
    if(!head||!head.next) return null;

    while(fast&&fast.next){
        slow=slow.next;
        fast=fast.next.next;
        if(slow==fast){
            slow=head;
            while(slow!=fast){
                slow=slow.next;
                fast=fast.next;
            }
            return slow;
        }
    }
    return null;  
};

```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** 🔄 需复习
- **感悟/易错点：**

### 7. 两数相加（Medium）
- **是否独立做出：** 看题解
- **题型/模板：** 
- **核心思路：** 逆序相加会有进位需要注意。
- **JavaScript 实现：**
```js
var addTwoNumbers = function(l1, l2) {
    // 模拟竖式加法
    const dummy = new ListNode(0);   // 虚拟头节点
    let cur = dummy;
    let carry = 0;                   // 进位

    while (l1 || l2 || carry) {
        // 取当前位的值，链表为空时取 0
        const val1 = l1 ? l1.val : 0;
        const val2 = l2 ? l2.val : 0;

        const total = val1 + val2 + carry;
        carry = Math.floor(total / 10);        // 新的进位（对应 Python 的 //）
        cur.next = new ListNode(total % 10);   // 当前位的值

        cur = cur.next;
        if (l1) l1 = l1.next;
        if (l2) l2 = l2.next;
    }

    return dummy.next;
};
```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：**
- **从 Python 题解转写时的差异：**
- **掌握程度：** ✅ 熟练 / 🔄 需复习 / ❌ 未掌握
- **感悟/易错点：** 在取结点的`val` 时记得先判断是否有该结点`l1?l1.val:0` 

- 还有个递归法！！！

### 8. 删除链表的倒数第 N 个节点（Medium）
- **是否独立做出：** 看提示 
- **题型/模板：** 快慢指针
- **核心思路：** 快指针先走N步
- **JavaScript 实现：**
```js
var removeNthFromEnd = function(head, n) {
    // 快慢指针 + 虚拟头节点
    // LeetCode 的 JS ListNode 构造器支持第二个 next 参数；
    // 保险起见也可拆成两行：const dummy = new ListNode(0); dummy.next = head;
    const dummy = new ListNode(0, head);
    let fast = dummy, slow = dummy;   // 分开声明，不能写 let fast = slow = dummy

    // 快指针先走 n+1 步
    for (let i = 0; i < n + 1; i++) {
        fast = fast.next;
    }

    // 快慢指针同步前进
    while (fast) {
        fast = fast.next;
        slow = slow.next;
    }

    // 此时 slow 指向待删节点的前驱
    slow.next = slow.next.next;
    return dummy.next;
};
```
- **复杂度：** O(__) / O(__)
- **JS 写法注意点：** 注意需要虚拟头结点，因为可以删除头结点，所以需要一个虚拟头结点作为前置
- **从 Python 题解转写时的差异：**
- **掌握程度：**  🔄 需复习
- **感悟/易错点：** 注意fast指针移位置n步，要确保fast指向null时，slow指向所要删除结点的前置结点上

---

## 三、今日总结

**学到的新模板/技巧：**
- 双指针：
	- 相交链表：俩指针跑的路程一样会相聚——所以一方跑完自己链路后去另一个指针的起点开始，两者会在相交点相遇
	- 回文链表：两倍的快慢指针找到中点后，后一半反转后进行比较
	- 环形链表：快慢指针有环必会相遇。找环的起始位置——从相遇点到起点=头结点到起点
- 双指针速度调节，看下是否有需要中点、是否需要特定N个的位置差之类判断。

**遇到的困难：**
-

**需要加入 JS/TS 模板库的内容：**
- 虚拟头结点——在头结点可能会有变动时需要
- 在有`.next.next` 时记得前置判断`x&&x.next` 
- 对`val` 同理，`x?x.val:0` 


**遗留问题（需复习）：**
-

**整体感受：** 😊 
