# DFS
Tries to construct a solution incrementally, one piece at a time. For each step, it will make consistent decision as before and recursively try out all possible alternatives before choosing the best one. 

### Recursion tree
1. How many levels are there in the recursion tree, what does it store on each level
2. How many states we need to try on each level

**If you are using a list or StringBuilder to hold result, for every append, there must be a delete to restore the original state of the parent node**

***Time complexity:*** states^height

***Space complexity:*** height

### All Subset
结果中每个元素的顺序无关，每一个元素的选择是常数（加，不加）
放， 不放
```
sb.append(input[index])
dfs(res,sb,input, index + 1)
sb.deleteCharAt(sb.length() - 1)

dfs(res, sb, input, index+ 1)
```
Time: 2^n
Space: n
##### Alternative #1: dedup, each subset in the result must be unique
##
**dedup:** 1. sort input array (Arrays.sort()) 2. before not add, while (index < input.length - 1 && input[index] == input[index + 1]). We want to always add the first elements
 如果要加，看到了就加。 如果不加，之后的都不加。这样子结果里会有同一个元素的duplicates 但不会有同样的组合。结果中元素本身是可以有duplicates，但组合不能有重复。
```
sb.append(input[index])
dfs(res,sb,input, index + 1)
sb.deleteCharAt(sb.length() - 1)
//Essentially we want to move the index to point to the last duplicates of cur element before enter dfs(not add) because dfs(index+1) will take us to the first of new element
while (index < input.length - 1 && input[index] == input[index + 1]) {
    index++;
}
dfs(res, sb, input, index+ 1)
```
##### Alternative #2: Pruning
eg.size k subset, combination sum
多一个base case, 存结果的时机变了.在满足要求的时候存结果，这时候不一定走完了所有的input. 存结果的时候记得要new一个新的instance存储，当前的stringbuilder/list是一个global container,会变的。当index == input.length的时候只是return 
```
if (sb.length == k) {
    res.add(sb.toString());
    return;
}
if (index == input.length) {
    return;
}
```
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

##### Alternative #1 {}()<>
##
**Pruning:** 
1. When add left, make sure there is remaining left to be added
2. When add right, make sure a. there is a matching left b. there is remaining to be added

Use extra data structures to store previous result
1. stack: store the left added so far. Whenever adding a left, push it to the stack. Whenever adding a right, check if the top element on stack matches with the left counterpart of current right. When checking stack, make sure to check if (!stack.isEmpty() before dereferencing). 
2. int[] remain: store the remainih number of each type of parenthesis
3. char[] set: store the different type of parenthsis (set.length == remain.length)
4. target: the target length of your StringBuilder
5. StringBuilder and result list

We are still checking each position of the result list, each corresponds to a parenthesis to be added. The recursion tree has level that is equal to the total number of parenthesis and each node has states equal to the total number of types of parenthesis. 
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


