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