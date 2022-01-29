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