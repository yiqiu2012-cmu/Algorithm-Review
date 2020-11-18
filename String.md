# Sliding Window
two pointers, when to move right, when to move left, (these two are determined by the condition of the substring) when to stop (usually when fast is out of bound)

### Longest substring without repeating charater

**substring: consecutive

**subsequence: non-consecutive

**data structure:**

Set<Integer>: contains all the non-repeating characters in the substring
    
window: [slow, fast) or [slow, fast - 1]

slow pointer: move left pointer when array[fast] points to a char that is already in set

fast pointer: move fast pointer when array[fast] points to a char that is not in set

global longest: update global longest when move either of the pointers longest = Math.max(longest, fast - slow)

```
public int lengthOfLongestSubstring(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }
    int slow = 0;
    int fast = 0;
    int longest = 0;
    Set<Character> set = new HashSet<>();
    while (fast < s.length()) {
        if (set.contains(s.charAt(fast))){
            set.remove(s.charAt(slow));
            slow++;
        } else {
            set.add(s.charAt(fast));
            fast++;
        }
        longest = Math.max(longest, fast - slow);
    }
    return longest;
    }
```
**Time:** O(n), each element will be visited once by slow and fast

**Space:** 0(min(m,n)), m size of window, n size of array

### Group Anagram

**data structure**: 

- Map<String, List<String>>
- char[26] ca: for each char c in String s, set ca[c- 'a'] to 1, then use String.valueOf(ca) to generate String

Iterate through s in String array, for each char in s, set the corresponding index to 1, and use String.valueOf(int[]) API to convert to string


