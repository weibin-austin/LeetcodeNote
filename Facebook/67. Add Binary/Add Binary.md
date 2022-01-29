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