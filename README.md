# algorithm-daydayUp
从2021年10月13日开始每天记录自己的算法成长历程

# 2021-10-13

## leetcode no1.两数之和（easy）

### 题目概述：双层循环 + hash优化

> 给定一个整数数组 **nums** 和一个整数目标值 **target*，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。
>
> 你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。
>
> 你可以按任意顺序返回答案。
>
> **示例 1：**
>
> 输入：nums = [2,7,11,15], target = 9
> 输出：[0,1]
> 解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

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

### 方法二：hash存储 + 一次循环

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

### 题目概述：链表

> 给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。
>
> 请你将两个数相加，并以相同形式返回一个表示和的链表。
>
> 你可以假设除了数字 0 之外，这两个数都不会以 0 开头。
>
> **示例1:**
>
> ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/02/addtwonumber1.jpg)
>
> ```
> 输入：l1 = [2,4,3], l2 = [5,6,4]
> 输出：[7,0,8]
> 解释：342 + 465 = 807.
> ```
>
> 

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

### 题目概述：Set + 暴力while循环 + 滑动窗口

> 给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。
>
> 示例 1:
>
> 输入: s = "abcabcbb"
> 输出: 3 
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

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

### 题目概述：sort + concat

> 给定两个大小分别为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的 中位数 。
>
> 示例 1：
>
> 输入：nums1 = [1,3], nums2 = [2]
> 输出：2.00000
> 解释：合并数组 = [1,2,3] ，中位数 2

### 方法一：暴力法 / 合并数组后获取中位数

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

# 2021-10-14

## leetcode no7.整数反转（easy）

### 题目概述：获取整数每一位的整的方法

> 给你一个 32 位的有符号整数 x ，返回将 x 中的数字部分反转后的结果。
>
> 如果反转后整数超过 32 位的有符号整数的范围 [−231,  231 − 1] ，就返回 0。
>
> 假设环境不允许存储 64 位整数（有符号或无符号）。
>
> **示例 1：**
>
> ```
> 输入：x = -123
> 输出：-321
> ```
>
> **示例 2：**
>
> ```
> 输入：x = 120
> 输出：21
> ```

### 方法一：利用取余获取每一个位数上的值

```js
var reverse = function(x) {
    let max = Math.pow(2,31) - 1,min = -max - 1;
    let result = 0;
    // 当x的位数被取完的时候 x === 0
    while(x !== 0) {
      result = result * 10 + x % 10;
      // 通过向下取整去除掉已经取到的那一位
      // 不能用Math.floor(x / 10), 因为存在负数情况，如Math.floor(-1.1) === -2
      x = parseInt(x / 10);
    }
  	if(result > max || result < min) { return 0 };
    return result;
};
```

## leetcode no5.最长回文子串（mid）

### 题目概述：中心扩散法 / 遍历所有可能 + 动态规划

> 给你一个字符串 s，找到 s 中最长的回文子串。
>
> 示例 1：
>
> 输入：s = "babad"
> 输出："bab"
> 解释："aba" 同样是符合题意的答案。

### 方法一：中心扩散法

```js
// 中心扩散法
// 遍历整个字符串，把每个节点假象为回文子串的节点，并分两种情况，一是奇数回文子串，二是偶数回文子串
// 因为函数check跳出while循环的时候，m与n是刚好不满足情况的时候，所以满足情况的是s.slice(m+1, n),length = n - m -1;
// str.slice(beginIndex[, endIndex]),str.slice(1, 4) 提取第二个字符到第四个字符
var longestPalindrome = function(s) {
    if(s.length === 1 || s.length === 0) {return s};
    let result = s[0];
    for(let i = 0; i < s.length; i++) {
        // 回文子串长度为奇数
        check(i, i);
        // 回文子串长度为偶数
        check(i, i + 1); 
    }
    function check(m, n) {
        while(s[m] === s[n] && m >= 0 && n <= s.length - 1) {
            m --;
            n ++;
        };
        result = n - m - 1 > result.length ? s.slice(m + 1, n) : result;
    }
    return result;
};
```

### 方法二：动态规划

```js
// dp[i][j]存储i到j是否为回文
// 先从第一列开始填写dp[i][j]是否为回文，且需要j >= i
// 若dp[i][j]为回文需要满足s[i] === s[j] 且 dp[i + 1][j - 1] === true
// 最后通过遍历dp找到dp[i][j] === true的最大回文子串
var longestPalindrome = function (s) {
  	// 用dp[i][j]表示i到j是否是回文
    // 先初始化为true
  	const dp = new Array(s.length).fill(0).map(item => new Array(s.length).fill(true));
  	// 遍历一半的二维数组，因为dp[i][j]中需要j >= i
  	for(let i = 1; i < s.length; i++) {
      	for(let j = 0; i + j < s.length; j++) {
          	// i到j为回文则需要i+1到j-1为回文
						dp[j][j + i] = (s[j] === s[j + i]) && dp[j + 1][i + j - 1];
      	};
    };
  	// 接下来再遍历一遍，依次求dp[i]的最大回文子串
  	let maxSubStr = '';
  	for(let i = 0; i < s.length; i++) {
    		let maxIndex = dp[i].lastIndexOf(true);
      	// 如果以i为启点的最大回文子串长度 > maxstr，则替换maxstr
      	if(maxIndex - i + 1 > maxSubStr.length) {
          	maxSubStr = s.substring(i, maxIndex + 1);
        };
    };
  	return maxSubStr;
}
```

### leetcode no8.字符串转换整数(atoi)（mid）

### 题目概述：

> 请你来实现一个 myAtoi(string s) 函数，使其能将字符串转换成一个 32 位有符号整数（类似 C/C++ 中的 atoi 函数）。
>
> 函数 myAtoi(string s) 的算法如下：
>
> 读入字符串并丢弃无用的前导空格
> 检查下一个字符（假设还未到字符末尾）为正还是负号，读取该字符（如果有）。 确定最终结果是负数还是正数。 如果两者都不存在，则假定结果为正。
> 读入下一个字符，直到到达下一个非数字字符或到达输入的结尾。字符串的其余部分将被忽略。
> 将前面步骤读入的这些数字转换为整数（即，"123" -> 123， "0032" -> 32）。如果没有读入数字，则整数为 0 。必要时更改符号（从步骤 2 开始）。
> 如果整数数超过 32 位有符号整数范围 [−231,  231 − 1] ，需要截断这个整数，使其保持在这个范围内。具体来说，小于 −231 的整数应该被固定为 −231 ，大于 231 − 1 的整数应该被固定为 231 − 1 。
> 返回整数作为最终结果。
> 注意：
>
> 本题中的空白字符只包括空格字符 ' ' 。
> 除前导空格或数字后的其余字符串外，请勿忽略 任何其他字符。
>
> 示例 1：
>
> 输入：s = "-91283472332"
> 输出：-2147483648
> 解释：
> 第 1 步："-91283472332"（当前没有读入字符，因为没有前导空格）
>          ^
> 第 2 步："-91283472332"（读入 '-' 字符，所以结果应该是负数）
>           ^
> 第 3 步："-91283472332"（读入 "91283472332"）
>                      ^
> 解析得到整数 -91283472332 。
> 由于 -91283472332 小于范围 [-231, 231 - 1] 的下界，最终结果被截断为 -231 = -2147483648 。
>
> 示例 2：
>
> 输入：s = "4193 with words"
> 输出：4193
> 解释：
> 第 1 步："4193 with words"（当前没有读入字符，因为没有前导空格）
>          ^
> 第 2 步："4193 with words"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
>          ^
> 第 3 步："4193 with words"（读入 "4193"；由于下一个字符不是一个数字，所以读入停止）
>              ^
> 解析得到整数 4193 。
> 由于 "4193" 在范围 [-231, 231 - 1] 内，最终结果为 4193 。

### 方法一：取巧parseInt()

```js
// parseInt(string, radix) 解析一个字符串并返回指定基数的十进制整数， radix 是2-36之间的整数，表示被解析字符串的基数。
var myAtoi = function(str) {
    const number = parseInt(str, 10);
    if(isNaN(number)) {
        return 0;
    } else if (number < Math.pow(-2, 31) || number > Math.pow(2, 31) - 1) {
        return number < Math.pow(-2, 31) ? Math.pow(-2, 31) : Math.pow(2, 31) - 1;
    } else {
        return number;
    }
};
```

### 方法二：自动机

