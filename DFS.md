# DFS
Tries to construct a solution incrementally, one piece at a time. For each step, it will make consistent decision as before and recursively try out all possible alternatives before choosing the best one. 

### Recursion tree
1. How many levels are there in the recursion tree, what does it store on each level
2. How many states we need to try on each level

**If you are using a list or StringBuilder to hold result, for every append, there must be a delete to restore the original state of the parent node**

***Time complexity:*** states^height
***Space complexity:*** height

### All Subset
结果中每个元素的顺序无关，每一个step的选择是常数（加，不加）
放， 不放
```
sb.append(input[index])
dfs(res,sb,input, index + 1)
sb.deleteCharAt(sb.length() - 1)

dfs(res, sb, input, index+ 1)
```
Time: 2^n
Space: n
### All Valid Parenthsis
结果中每个元素的顺序无关，每个元素的选择是常数，但加了剪枝（只有符合条件的才加）
所有parenthesis的题，在加之前加了判断条件
```
if (l < n) {
    sb.append('(');
    dfs(res, sb, index + 1);
    sb.deleteCharAt(sb.length() - 1);
}
if (l > r) {
    sb.append(')');
    dfs(res, sb, index + 1);
    sb.deleteCharAt(sb.length() - 1);
}
```
Time: 2 ^ 2n
Space: n

### All combination fo coins
结果中每个元素的顺序无关，每个元素的选择是dynamically changing
所有数字的问题，用一个for loop来表示所有可以尝试的情况，每一种情况都做dfs
```
for (int i = 0; i < target/coins[index]; i++) {
    dfs(res, sb.append(i), target - i * coins[index], index + 1);
    sb.deleteCharAt(sb.length() - 1);
}
```

### All permutation (SWAP SWAP)
When the result contains all the elements in the input, consider using swap swap, with some sort of condition checking for proning
结果包括input中的所有元素，需要达到某种顺序，每次尝试的选择是递减的
用一个for loop, starting from current index, and swap with all the elements that are yet to be processed
```
for (int i = index, i < input.length; i++) {
    swap(input, index, i);
    dfs(res, index + 1, input);
    swap(input, index, i);
}
```
**每次swap之后需要swap回来**


