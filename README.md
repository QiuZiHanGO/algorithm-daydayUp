# algorithm-daydayUp
从2021年10月13日开始每天记录自己的算法成长历程

# 2021-10-13

## leetcode no1.两数之和（easy）

### 题目概述：双层循环+hash优化

![image-20211013110013287](/Users/qiuzihan/Library/Application Support/typora-user-images/image-20211013110013287.png)

### 方法一：暴力法

第一层循环遍历数组获取所对应的值，然后再循环去查找所对应的值

```js
var twoSum = function(nums, target) {
    let n = nums.length;
    for(let i=0;i<n-1;i++){
        for(let j=i+1;j<n;j++){
            if(nums[i]+nums[j]===target){
                return [i,j]
            }
        }
    }
};
```

### 方法二：hash存储+一次循环

通过一次遍历将所遇到的值存在hash表中，就可以在此后的遍历中轻松获取

```js
var twoSum = function(nums, target) {
    const map = new Map();// 创建一个hashmap来存储
    for(let i = 0; i < nums.length; i++) {
        let another = target - nums[i];
        if(map.has(another) && map.get(another) !== i) {
            return [i, map.get(another)]
        }
      	// 将所遇到的值存储在hashmap中
        map.set(nums[i], i);
    }
};
```

## leetcode no2.两数相加（mid）

### 题目概述：链表![image-20211013111111356](/Users/qiuzihan/Library/Application Support/typora-user-images/image-20211013111111356.png)

### 方法一：模拟

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
  	// 链表有一个头节点作为入口
    let head = null,tail = null;
    let carry = 0;
    while(l1||l2){
        const n1 = l1?l1.val:0;
        const n2 = l2?l2.val:0;
        const sum = n1+n2+carry;
        if(!head){
            head = tail = new ListNode(sum % 10);
        }else{
            tail.next = new ListNode(sum % 10);
            tail = tail.next;
        }
        carry = Math.floor(sum/10);
        if(l1){
            l1 = l1.next
        }
        if(l2){
            l2 = l2.next
        }
    }
    if(carry>0){
        tail.next = new ListNode(carry);
    };
    return head
};
```

## leetcode no3. 无重复字符的最长子串（mid）

### 题目概述：Set+暴力while循环+滑动窗口

![image-20211013202729006](/Users/qiuzihan/Library/Application Support/typora-user-images/image-20211013202729006.png)

### 方法一：暴力while循环

```js
//简单暴力法
//遍历字符串，每次以一个节点为起点去寻找不重复的最长子串，初始化Set
//遍历的过程中用while循环找出满足不重复的最大长度
var lengthOfLongestSubstring = function(s) {
    if(s.length===1){return 1}
    let ans = 0;
    for(let i = 0;i<s.length;i++){
        // 初始化Set
        let occ = new Set();
        // 添加第一项的值
        // String.prototype.charAt() / charAt() 方法从一个字符串中返回指定的字符 / str.charAt(index)
        occ.add(s.charAt(i));
        let rk = (i+1 < s.length) ? i+1 : 0;
        while( rk && rk < s.length && !occ.has(s.charAt(rk))){
            occ.add(s.charAt(rk));
            rk = rk + 1;
        }
    // 判断此次子串是不是最长的
    ans = Math.max(ans, rk - i)
    }
    return ans;
};
```

### 方法二：滑动窗口法

```js
// 滑动窗口法
// 基本思路与简单暴力法相似
// 区别在于不在for循环中每次初始化set，而是通过滑出后delete相应的值代替
var lengthOfLongestSubstring = function(s) {
    // 哈希集合，记录每个字符是否出现过
    const occ = new Set();
    const n = s.length;
  	// rk记录滑到的右边
    let rk = -1;
    let ans = 0;
    for(let i = 0;i < n; i++){
      	// 每次进入循环滑出上一个值
        if(i !== 0){
            occ.delete(s.charAt(i - 1));
        }
        while(rk + 1 < n && !occ.has(s.charAt(rk + 1))){
            occ.add(s.charAt(rk + 1));
            rk = rk + 1;
        }
    ans = Math.max(ans, rk - i + 1);
    }
    return ans;
};
```

## leetcode no4.寻找两个正序数组的中位数（hard）

### 题目概述：sort+concat

![image-20211013213448656](/Users/qiuzihan/Library/Application Support/typora-user-images/image-20211013213448656.png)

### 方法一：暴力法/合并数组后获取中位数

```js
var findMedianSortedArrays = function(nums1, nums2) {
    let nums = nums1.concat(nums2);
  	// 合并后再排序
    nums = nums.sort(function(a, b) {
        return a - b;
    });
    let n = nums.length;
  	// 获取中位数
    let result = n % 2 == 0
        ? (nums[n/2] + nums[n/2-1]) / 2
        : nums[Math.floor(n/2)];
    return result;
};
```



