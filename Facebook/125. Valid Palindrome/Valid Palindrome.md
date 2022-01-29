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