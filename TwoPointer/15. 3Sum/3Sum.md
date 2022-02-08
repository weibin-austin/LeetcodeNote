Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

思路：三指针 -> 定一个 + 双指针

注意： 左指针相同数字，需要去除重复组合。两个检测都是在指针移动之后验证是否左指针重复

```java
/*
	定指针在执行helper方法之前检测重复
*/
if(i != 0 && nums[i] == nums[i - 1]) {
		continue;                         
}
/*
	helper方法里，检查重复是在指针缩进之后执行。
*/
if(i != 0 && nums[left] == nums[left - 1]) {
		continue;                         
}
```

​	双指针模版：

```java
int left = 0;
int right = nums.length;
while(left < right) {
  //执行操作
  //移动指针（条件执行）
  left++:
  right--;
}
```

Arrays.sort()

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        for(int i = 0; i < nums.length; i++) {
            if(i != 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            helper(nums, i, res);
        }
        return res;
    }
    
    private void helper(int[] nums, int index, List<List<Integer>> res) {
        int value = nums[index];
        int left = index + 1;
        int right = nums.length - 1;
        while(left < right) {
            if(left < right && nums[left] + nums[right] + value < 0) {
                left++;
            } else if (left < right &&  nums[left] + nums[right] + value > 0) {
                right--;
            } else {
                List<Integer> three = new ArrayList<>();
                three.add(value);
                three.add(nums[left]);
                three.add(nums[right]);
                res.add(three);
                left++;
                right--;
                while(left < right && nums[left] == nums[left - 1]) {
                    left++;
                }
            }
        }
    }
}
```



