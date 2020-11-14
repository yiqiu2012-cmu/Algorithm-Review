
## Two Sum
### Apporach #1: Linear scan + hashset/hashmap
Two Sum uses a hashmap to find complement values, and therefore achieves O(N) time complexity.

**data structure:** Map<Value, indice> or Set<Integer>
**if return wants true/false, use set, if want indices, use map
traverse through the input array, check if target - a[i] in map, if so, return true. Else put <a[i], i> in
```
class Solution {
    public int[] twoSum(int[] nums, int target) {
       Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(target - nums[i])) {
                return new int[]{map.get(target - nums[i]), i}; 
            } 
            map.put(nums[i], i);
        }
        return null;
    }
}
```
##### Map operations
1. containsKey(Object key): Returns true if this map contains a mapping for the specified key.
2. conatinsvalue(Object value): returns true if this map contains a value
3. entrySet(): return a Set view of the mappings in the map
4. get(Object key): returns the value the mapped by the key, or null if map contains no such value
5. put(K key, V value): associates the specified value with the specified key in this map.
6. remove(Object key): removes the mapping for the specified key from this map if present.
7. remove(Object key, Object value): remove the entry only if the key is mapped to the value
Time: O(n)
Space: O(n) worst case

### Approach #2: Sort the array first and use two pointers moving towrad each other
Two Sum II uses the two pointers pattern and also has O(n) time complexity for a sorted array. We can use this approach for any array if we sort it first, which bumps the time complexity to O(nlogn).
int left = 0;
int right = array.length - 1
while(left < right) { //must be < because i needs to be different from j to have two different numbers
    case1: a[i] + a[j] == target return true
    case2: a[i] + a[j] < target
            i++;
    case3 a[i] + a[j]  > target
            j--;
}
**This approach only works when return true/false, because we are changing the order of the orginal array
Time: O(n) if sorted, O(nlogn + n) if unsorted
Space: O(1)

### 3 Sum
For the main function:
1. Sort the input array nums.
2. Iterate through the array:
    - If the current value is the same as the one before, skip it.
    - Otherwise, call twoSumII for the current position i.
    
For twoSumII function:
1. Set the low pointer lo to i + 1, and high pointer hi to the last index.
2. While low pointer is smaller than high:
    - If sum of nums[i] + nums[lo] + nums[hi] is less than zero, increment lo.
    - If sum is greater than zero, decrement hi.
    - Otherwise, we found a triplet:
        - Add it to the result res.
        - Decrement hi and increment lo.
        - Increment lo while the next value is the same as before to avoid duplicates in the result.
3. Return the result res.
```
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (i == 0 || nums[i] != nums[i - 1]) {
                helper(nums, i, res);
            }
        }
        return res;
    }
    private void helper(int[] nums, int i, List<List<Integer>> res) {
        int left = i + 1; 
        int right = nums.length - 1;
        while (left < right) {
            int sum = nums[i] + nums[left] + nums[right];
            if (sum < 0) {
                left++;
            } else if (sum > 0) {
                right--; 
            } else {
                res.add(Arrays.asList(nums[i], nums[left], nums[right]));
                left++;
                right--;
                while (left < right && nums[left] == nums[left - 1]){
                    left++;
                }
            }
        }
    }
}
```
Time Complexity: O(n^2), twosum is O(n), and we call it n times for each element in the array
Space Complexity: O(1)

### 3 SUM CLOSEST
Two pointers 
```
public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int diff = Integer.MAX_VALUE;
        for (int i = 0; i < nums.length; i++) {
            int left = i + 1;
            int right = nums.length - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (Math.abs(target - sum) < Math.abs(diff)) {
                    diff = target - sum;
                }
                if (sum < target) {
                    ++left;
                } else if (sum > target) {
                    --right;
                } else {
                    return sum;
                }
            }
        }
        return target - diff;
    }
```