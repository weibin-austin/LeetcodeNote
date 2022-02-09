##### 18. 4Sum

Given an array `nums` of `n` integers, return *an array of all the **unique** quadruplets* `[nums[a], nums[b], nums[c], nums[d]]` such that:

- `0 <= a, b, c, d < n`
- `a`, `b`, `c`, and `d` are **distinct**.
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

You may return the answer in **any order**.

思路：双指针 定二 + 动二
注意：双层for循环，循环体第一步先进行重复检测
方法和 **3Sum** 方法类似 

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> res = new ArrayList<>();
        if(nums == null || nums.length < 4) {
            return res;
        }
        Arrays.sort(nums);
        for(int i = 0; i < nums.length - 3; i++) {     
            if(i > 0 && nums[i - 1] == nums[i]) {
                continue;
            }  
            for(int j = i + 1; j < nums.length - 2; j++) {   
                if( j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
                int left = j + 1, right = nums.length - 1;                
                while(left < right){
                    int curr = nums[i] + nums[j] + nums[left] + nums[right];
                    if(curr == target){
                        List<Integer> t = helper(nums, i, j, left, right);
                        res.add(new ArrayList<Integer>(t));
                        left++; right--;
                        while(left < right && nums[left] == nums[left - 1]){
                            left++;
                        }
                        while(left < right && nums[right] == nums[right + 1]){
                            right--;
                        }
                    }else if(curr > target){
                        right--;
                    }else{
                        left++;
                    }
                }
            }
        }
        return res;
    }
    
    private List<Integer> helper(int[] nums, int i, int j, int k, int l) {
        List<Integer> t = new ArrayList<>();
        t.add(nums[i]);
        t.add(nums[j]);
        t.add(nums[k]);
        t.add(nums[l]);
        return t;
    }
}
```

