---
title: leetcode 寫題記錄
date: 2021-07-21 16:14:27
---

## 序
目前剛開始刷 leetcode，打算先把各種 `tag` 裡面 easy 的題目都刷一些然後再到 medium 刷，在這邊紀錄我在刷 leetcode 的心得。

## Hash table
想要降低時間複雜度的話，比起寫一個 `nested-loop` 時間複雜度為 O(n^2)，可以使用 `Hash table` 先將要使用到的值記下來，然後再對 Hash table 進行查找。這樣就能將時間複雜度降為 O(n)。

## Two pointer
在解 Two Sum II - Input array is sorted 這題的時候學到的技巧，對 array 的頭跟尾設定 `left` 以及 `right`，然後從兩側開始往中間夾 (移動 left 以及 right)，直到找出相符答案。

```javascript
// 設定頭尾指標
let l = 0, r = numbers.length-1;

while(true) {
    if (numbers[l] + numbers[r] === target)
        return [l+1, r+1];
    else if (numbers[l] + numbers[r] > target) 
        r --;
    else if (numbers[l] + numbers[r] < target)
        l ++;
}
```

## Find Difference
如果一個題目是要找兩個資料不同的地方的話，可以試著使用 xor 來加快運算時間以及減少記憶體使用量。像是 _leetcode 389. Find the Difference_ 這題要找兩個被打亂的字串相異的字元 ( t字串為s字串的打亂版再增加一個字元 )，就可以使用 xor 來找。

```javascript
var findTheDifference = function(s, t) {
    let res = 0, i;
    for(i = 0; i < s.length; i++) {
        res ^= s.charCodeAt(i) ^ t.charCodeAt(i);
    }
    res ^= t.charCodeAt(i);
    return String.fromCharCode(res);
};
```

## Binary Tree
目前寫到的題目通常來說可以分成使用 `recursive` 或是 `iterator` 來解，`recursive` 的話看是要用 前序, 中序, 還是後序來去搞，`iterator` 的話就先創一個 stack，然後將子節點都放進去來去處理。

```javascript
// 後序 (postorder)
const stack = [];
const res = []; // 負責存 output value
stack.push(root);
while(stack.length > 0) {
    const node = stack.pop();
    res.unshift(node.val);
    
    node.left ? stack.push(node.left) : '';
    node.right ? stack.push(node.right) : '';
}
```