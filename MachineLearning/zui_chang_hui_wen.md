# 最长回文子串

## 题目

给你一个字符串 s，找到 s 中最长的回文子串。

## 用例

```
输入：s = "babad"
输出："bab"

输入：s = "abb"
输出："bb"
```

## 思路

遍历整个字符串，以一个字符为中心，向外扩展，比对两侧的字符是否一致，如果一致，则宽度扩增一点，直到不一致，或者到达字符串边缘

但是如果回文长度是偶数，没有中心，那么就没法实现，解决方法是在每个字符中间加个特殊字符`#`，这样得到的回文一定是奇数，只需要最后把`#`去掉即可

top即为中心点的编号位置，length即为宽度，那么最后的结果，即为top-length+1的位置的值，到top+length的位置的值

在左侧+1的原因是，length是++后判断是否符合要求，也就是说，边界的值是第一个不符合情况的值，所以需要+1

而右侧因为本来需要-1，又+1，则抵消了

## 答案

```
func longestPalindrome(s string) string {
    if len(s) < 2 {
        return s
    }

    var (
        max_length = 0
        max_top = 0
        top = 1
        tmp_str = "#" + strings.Join(strings.Split(s, ""), "#") + "#"
    )

    for top < len(tmp_str) {
        length := 1
        for {
            if top - length == 0 || top + length >= len(tmp_str) {
                break
            }
            if tmp_str[top-length] != tmp_str[top+length] {
                break
            }
            length ++
        }
        if length > max_length {
            max_top = top
            max_length = length
        }
        top++
    }
 
    return strings.Replace(tmp_str[max_top-max_length+1:max_top+max_length], "#", "", -1)
}
```