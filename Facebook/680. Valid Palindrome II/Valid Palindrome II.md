##### 680. Valid Palindrome II

Given a string `s`, return `true` *if the* `s` *can be palindrome after deleting **at most one** character from it*.

```java
Input: s = "aba"
Output: true
```

```java
Input: s = "abca"
Output: true
Explanation: You could delete the character 'c'.
```

```java
class Solution {
    public boolean validPalindrome(String s) {   // 回文数，双指针
        
        if (s == null) {
            return false;                        // 异常判断
        }
        
        int left = 0;                            // 定义双指针
        int right = s.length() - 1;
        
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) { 
                break;                           // 双指针相向移动，遇到不同的字符 break
            }
            left++;
            right--;
        }
        
        if (left > right) {                       // 双指针运动交叉，说明回文数的字符全部遍历了，说明本身就是一个回文数， 不需要删除一个字符。其本身就是一个回文数
            return true;
        }
        
        return iP(s, left + 1, right) || iP(s, left, right - 1);  //调用iP函数，iP函数用来判断是否为回文数
    }
    
    private boolean iP(String s, int left, int right) {
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```
