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