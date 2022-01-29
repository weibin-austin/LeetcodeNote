# LeetcodeNote
This repository contains the leetcode note of Austin Sun


#### Facebook

##### 67. Add Binary

```java
class Solution {
    public String addBinary(String a, String b) {
        StringBuffer res = new StringBuffer();
        int aLength = a.length() - 1;
        int bLength = b.length() - 1;
        int carry = 0;
        
        while (aLength >= 0 || bLength >= 0) {
            int sum = carry;
            if(aLength >= 0) {
                sum += a.charAt(aLength) - '0';
                aLength--;
             }
            if(bLength >= 0) {
                sum += b.charAt(bLength) - '0';
                bLength--;
            }
            carry = sum > 1 ? 1 : 0;
            res.append(sum % 2);
        }
        if(carry != 0) res.append(carry % 2);
        return res.reverse().toString();
    }
}


/**
a: "1010"       i = 3
    |
       
b:   "11"       j = 1
      |
carry = 0

while (i >= 0 || j >= 0){
    sum = 0, 
    sum = 0, i = 2, 
    sum = 1, j = 0,
    sum不大于1, carry = 0
    res.append(sum % 2) -> res = "1"
    
    sum = carry = 0,
    sum = 0+1=1, i = 1,
    sum = 1+1=2, j = -1,
    sum大于1, carry = 1
    res.append(sum % 2) -> res = "10"
    
    sum = carry = 1;
    sum = 1+0=1, i = 0
    不执行j
    sum不大于1，carry = 0
    res.append(sum % 2) -> res = "101" 
    
    sum = carry = 0
    sum = 0+1=1, i = -1
    不执行j
    sum不大于1， carry = 0
    res.append(sum % 2) -> res = "1011"
}

if (carry != 0) res.append(carry)
res.reverse().toString();

*/
```

##### 415. Add Strings

```java
class Solution {
    public String addStrings(String num1, String num2) {
        StringBuffer res = new StringBuffer();
        int l1 = num1.length() - 1;
        int l2 = num2.length() - 1;
        int carry = 0;
        
        while(l1 >= 0 || l2 >= 0) {
            int sum = carry;
            if(l1 >= 0) sum += num1.charAt(l1--) - '0';
            if(l2 >= 0) sum += num2.charAt(l2--) - '0';
            carry = sum / 10;
            res.append(sum % 10);
        }
        if(carry != 0) res.append(carry);
        return res.reverse().toString();
    }
}
```

##### 560. Subarray Sum Equals K

BF法：双层循环，找到所有subarray的和吗，test if `SUM[i, j]` = k.

外层start: [0,n-1]， 内层end: [1,n]，内层计算[start, end

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int counter = 0; 
        for (int start = 0; start < nums.length; start++) {
            for(int end = start + 1; end <= nums.length; end++) {
                int sum = 0;
                for (int i = start; i < end; i++)
                    sum += nums[i];
                if(sum == k)
                    counter++;
            }
        }
        return counter;
    }
}
```

Map法，方法本质上是利用了哈希表，对应sum和频率。
从第一位开始的每个subarray，本质上是找sum[i,j] = sum[0, j] - sum[0, i]  == k(?)

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0,1);
        int sum = 0;
        int res = 0;
        for(int n: nums) {
            sum += n;
            if(map.containsKey(sum - k)) {
                res += map.get(sum - k);
            }
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return res;
    }
}
//===========================================================================================================
class Solution {
    public int subarraySum(int[] nums, int k) {
//array:     1, 2, 3,  4, 5,  6
//sum-array: 1, 3, 6, 10, 15, 21           
//    |------|
//    |-|
// 利用Map，解题思路类似于twoSum，判断Map中是否有对应的 （sum - target）
        
        Map<Integer, Integer> map = new HashMap<>(); // 存放当前的sum 和 当前满足条件结果的频率
        map.put(0, 1);          // 初始化， sum是0， 频率为1. 如果没有任何一个条件满足，不会get,map中的数据。
                               //如果没有对map初始化，使其频率为1，那么第一个数就等于k时读取的数据是0. 这样就会少算一位
        int curSum = 0;        
        int res = 0;
        for(int num : nums) {     
            curSum += num;                           // 当前的sum值
            if(map.containsKey(curSum - k)) {        // 判断当前的map里是否有对应的(sum - target)
                res += map.get(curSum - k);          // 如果有，就把这个对应的频率加到counter上
            }
            map.put(curSum, map.getOrDefault(curSum, 0) + 1); 
                                                     // 如果没有对应的，就把这一位的map的sum和频率初始化 或者+1
        }
        return res;
    }
}
//===========================================================================================================
class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        int sum = 0;
        int res = 0;
        for(int n: nums) {
            sum += n;
            if(sum == k){ // 对应上两个方法的初始化问题，在循环体外没有预先给map.put(0,1).说明没有考虑到当sum=k的情况
                res++;    // 所以需要在循环体内设置sum == k的情况，并更新res
            }
            
            if(map.containsKey(sum - k)) { // 这里判断sum-k的key存在于map中的情况
                res += map.get(sum - k);
            }
            
            map.put(sum, map.getOrDefault(sum, 0) + 1); // 每次判断结束都要将新的sum存放在map中
        }
        return res;
    }
}
```

##### 238. Product of Array Except Self

`answer[i]` *is equal to the product of all the elements of* `nums` *except* `nums[i]`.

记录当前数左边的全部乘积。存入数组。在用一个临时变量存储右边所有数的乘积。再把她们乘到一起。

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;    // n = 4
        
        int[] L = new int[n];   // left 长度4
        L[0] = 1;               // 最左边L[0]=1
        
        int[] R = new int[n];   // right 长度4
        R[n - 1] = 1;           // 最右边R[0]=1
        
        int[] ans = new int[n]; // 结果数组长度4
        ans[0] = 1;             // 第一位初始值1
        
        for (int i = 1; i < n; i++) {        // [1, 2, 3, 4]
            L[i] = nums[i - 1] * L[i - 1];   // [1, 1, 2, 6] 
        }                                    // 计算该位左边的所有乘积，从正数第二位开始
        
        for (int i = n - 2 ; i >= 0; i--) {  // [1, 2, 3, 4]
            R[i] = nums[i + 1] * R[i + 1];   // [24,12,4, 1]
        }                                    // 计算该位右边的所有乘积，从倒数第二位开始
        
        for (int i = 0; i < n; i++) {        // 遍历数组，将该位左右乘积累乘
            ans[i] = L[i] * R[i];
        }
        return ans;
    }
}
/**
==========================================================================================================

*/
class Solution {
    public int[] productExceptSelf(int[] nums) {   // nums = [1,2,3,4]
        int n = nums.length;          
        int res[] = new int[n];         
        res[0] = 1;                                // res:[1, _, _, _];
        
        for(int i = 1; i < n; i++) {            // 循环开始在第二位
            res[i] = res[i - 1] * nums[i - 1];  // 第二位等于上一位的结果*上一位的nums 
        }                                       // res[1] = res[0] * nums[0] = 1 * 1 = 1 -> res:[1, 1, _, _];
                                                // res[2] = res[1] * nums[1] = 1 * 2 = 2 -> res:[1, 1, 2, _];
                                                // res[3] = res[2] * nums[2] = 2 * 3 = 1 -> res:[1, 1, 2, 6];
        
        int right = 1;                        // right = 1, 用数值存放乘积
        for(int i = n - 1; i >= 0; i--) {     // 从最后一位开始，到第一位
            res[i] *= right;  // res[3] = res[3] * right = 6 * 1 = 6;   right = 1 * nums[3] = 1 * 4 = 4
            right *= nums[i]; // res[2] = res[2] * right = 2 * 4 = 8;   right = right * nums[2] = 4 * 3 = 12
        }                     // res[1] = res[1] * right = 1 * 12 = 12; right = right * nums[1] = 12 * 2 = 24
                              // res[0] = res[0] * right = 1 * 24 = 24; right = right * nums[0] = 24 * 1 = 24

        return res;
    }
}
```

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



##### 125. Valid Palindrome

判断是否为有效回文串：有效字符：数字+字母（大小写）

```java
class Solution {
    public boolean isPalindrome(String s) {
       
      if (s == null) {
		    return false;                 // 异常检测
	    }

	    int left = 0;                    // 定义双指针
      int right  = s.length() - 1;

	    while (left < right) {
	    	while (left < right && !isValid(s.charAt(left))) {
	    		left++;                  // 确定左边的有效字符：不越界 + 非法字符 -> 左指针右移    
	    }                            

	    while (left < right && !isValid(s.charAt(right))) {
	    		right--;                // 确定右边的有效字符：不越界 + 非法字符 -> 右指针左移
	    }

	    if (left < right && !isEqual(s.charAt(left), s.charAt(right))) {
			    return false;      // 上面两个循环找到的字符相比较是否不同，如果不同，直接return false；如果相同，指针靠拢一步
	    }

		    left++;             // 指针靠拢
		    right--;
	    }
	    return true;          // 循环结束条件：left < right
    }   
    
    private boolean isValid(char c) {    // 子方法，判断是否为有效字符
 	    return Character.isLetter(c) || Character.isDigit(c); 
    }
    
    private boolean isEqual(char a, char b) { //判断两字符是否相等 
        return Character.toLowerCase(a) == Character.toLowerCase(b); // 转换为统一的小写字符
    }
    
}

```

- 总结回文数的模型

  - 异常判断

  - ```java
    if(s == null) {
    	return false
    }
    ```

  - 定义双指针

  - ```java
    int left = 0;
    int right = s.length() - 1
    ```

  - 循环体

  - ```java
    while(left < right ) {
    		
    		//内部条件判断
    		while(left < right && 判断条件) {
    				
    		}
        // 内部循环的目的是为了移动双指针
        // 一般把双指针的移动放在内部循环之外
    		// left++;
    		// right--;
    }
    ```

    - **判断是否为回文数： 方法 (默写)**

    - ```java
      private boolean isPalindrome(String s, int left, int right) {
        while(left < right) {
          if(s.charAt(left) != s.charAt(right)) {
            return false;
          }
          left++;
          right--;
        }
        return true;
      }
      ```

      

VS-Code记笔记测试
