---
title: js常见算法题
date: 2017-08-16 19:58:53
categories: javaScript
tags: [js算法]
---
#### 判断一个单词是否是回文

{% codeblock lang:objc %}
var str = 'helloolleh';
function checkPalindrom(str) {
    return str == str.split('').reverse().join('');
}
console.log(checkPalindrom(str));//true
{% endcodeblock %}
<!--more-->
#### 去掉一组整型数组重复的值

{% codeblock lang:objc %}
 let arr = [1, 13, 24, 11, 11, 14, 1, 2];
{% endcodeblock %}
##### 第一种方法：Object方法。主要考察个人对Object的使用，利用key来进行筛选。

{% codeblock lang:objc %}
let unique = function (arr) {
    let hashTable = {};
    let data = [];
    for (let i = 0, l = arr.length; i < l; i++) {
        if (!hashTable[arr[i]]) {
            hashTable[arr[i]] = true;
            data.push(arr[i]);
        }
    }
    return data;
}   
console.log(unique(arr));//[1,13,24,11,14,2]
{% endcodeblock %}
##### 第二种方法：Set方法

{% codeblock lang:objc %}
let newArr = Array.from(new Set(arr));
console.log(newArr);//[1,13,24,11,14,2]
{% endcodeblock %}
##### 第三种方法：indexOf方法

{% codeblock lang:objc %}
let unique02 = function(arr){
    let newArr = [arr[0]];
    for(let i = 1;i < arr.length;i ++){
        if(newArr.indexOf(arr[i]) == -1){
            newArr.push(arr[i]);
        }
    }
    return newArr;
}
console.log(unique02(arr));//[1,13,24,11,14,2]
{% endcodeblock %}
#### 统计一个字符串出现最多的字母

{% codeblock lang:objc %}
let str = 'afjghdfraaaasdenas';
function findMaxDuplicateChar(str) {
    if (str.length == 1) {
        return str;
    }
    let charObj = {};
    for (let i = 0; i < str.length; i++) {
        if (!charObj[str.charAt(i)]) {
            charObj[str.charAt(i)] = 1;
        } else {
            charObj[str.charAt(i)] += 1;
        }
    }
    let maxChar = '',
        maxValue = 1;
    for (var k in charObj) {
        if (charObj[k] >= maxValue) {
            maxChar = k;
            maxValue = charObj[k];
        }
    }
    return maxChar;
}
console.log(findMaxDuplicateChar(str));//a
{% endcodeblock %}
#### 排序算法

{% codeblock lang:objc %}
let arr = ['a','d','c','b'];
{% endcodeblock %}

##### 第一种：冒泡排序


{% codeblock lang:objc %}
function bubbleSort(arr){
    if (arr.length <= 1) {
        return arr;
    }
    for(let i = 0;i < arr.length-1;i ++){
        for(let j = i+1;j < arr.length;j ++){
            if(arr[i] > arr[j]){
                let temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
    }
    return arr;
}
console.log(bubbleSort(arr));//['a','b','c','d']
{% endcodeblock %}
##### 第二种：快速排序

{% codeblock lang:objc %}
function quickSort(arr) {
    if (arr.length <= 1) {
        return arr;
    }
    let leftArr = [];
    let rightArr = [];
    let q = arr[0];
    for (let i = 1; i < arr.length; i++) {
        if (arr[i] > q) {
            rightArr.push(arr[i]);
        } else {
            leftArr.push(arr[i]);
        }
    }
    return [].concat(quickSort(leftArr), [q], quickSort(rightArr));
}
console.log(quickSort(arr));//['a','b','c','d']
{% endcodeblock %}


#### 随机生成指定长度的字符串

{% codeblock lang:objc %}
//参数n为要生成的字符串长度
function randomString(n) {
    let str = 'abcdefghijklmnopqrstuvwxyz9876543210';
    let tmp = '',
        i = 0,
        l = str.length;
    for (i = 0; i < n; i++) {
        tmp += str.charAt(Math.floor(Math.random() * l));
    }
    return tmp;
}
{% endcodeblock %}
#### 找出正数组的最大差值


{% codeblock lang:objc %}
var arr = [10,5,11,7,8,9];

{% endcodeblock %}
很明显我们知道，最大差值肯定是一个数组中最大值与最小值的差。
{% codeblock lang:objc %}
function getMaxProfit(arr) {
    var minPrice = arr[0];
    var maxProfit = 0;
    
    for (var i = 0; i < arr.length; i++) {
        var currentPrice = arr[i];
        minPrice = Math.min(minPrice, currentPrice);
        var potentialProfit = currentPrice - minPrice;
        maxProfit = Math.max(maxProfit, potentialProfit);
    }
    return maxProfit;
}
console.log(getMaxProfit(arr));//6
{% endcodeblock %}