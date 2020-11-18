
### Max Sum Path (leaf to leaf)
**1. What to ask from your left and right child**
left: max single path in my left subtree(from lchild all the way down to the leaf node)
right: max single path in my right subtree(from rchild all the way down to the leaf node)
**2. What to do at current level**
if both lchild and rchild are non-null, update globalmax[0] with left + right + root.val
**3. What to return to my parent**
Math.max(left, right) + 1
```
public int maxPathSum(TreeNode root) {
    int[] globalMax = new int[1];
    globalMax[0] = Integer.MIN_VALUE;
    helper(root, globalMax);
    return globalMax[0];
  }
private int helper(TreeNode root, int[] globalMax) {
    if (root == null) {
        return 0;
    }
    int left = helper(root.left, globalMax);
    int right = helper(root.right, globalMax);
    if (root.left != null && root.right != null) {
        globalMax[0] = Math.max(root.key + left + right, globalMax[0]);
        return root.key + Math.max(left, right);
    }
    return root.left == null ? right + root.key : left + root.key;
}
 ```
 **为什么需要maintain一个int[] gloabl**: 因为recursion call返回的结果（max single path from leaf to node)是与最终返回结果不同的（max path from leaf to leaf)所以需要用一个额外的data structure来retain最终返回结果并update along the way
 
 ### Max Sum Path(any node to any node) 
 **1. What to ask from your left and right child**
left: max single path in my left subtree(starting from lchild)
right: max single path in my right subtree(strating from rchild)
**2. What to do at current level**
update globalmax[0] with max(left,0) + max(right,0) + root.val
**3. What to return to my parent**
Math.max(left, right) + 1
```
public int maxPathSum(TreeNode root) {
  int[] globalMax = new int[1];
    globalMax[0] = Integer.MIN_VALUE;
    helper(root, globalMax);
    return globalMax[0];
}
private int helper(TreeNode root, int[] global) {
    if (root == null) {
        return 0;
    }
    int left = Math.max(helper(root.left, global), 0);
    int right = Math.max(helper(root.right, global), 0);
    global[0] = Math.max(global[0], left + right + root.val);
    return Math.max(left, right) + root.val;
}
```
### Math Sum Path (leaf to root)
##### Prefix sum approach
信息至上而下传递并update，再自下而上返回的过程。在每一层都先update信息，把update好的信息传入下一层recursion call, 只在base case return信息
Pass a prefix sum from root to left, update along the way, return at leaf node. Prefix sum contains the sum of the nodes on a single path from root to current node, excluding current node

**1. What to ask from your left and right child**
left: max single path in my left subtree from root to leaf
right: max single path in my right subtree from root to leaf

**2. What to do at current level**
update prefix sum and continue based on different cases
base cases: 
1. if both lchild and rchild are null, we reached a leaf node, therefore return the prefix sum
2. if one root is non-null, keep recursing for that node, update prefix sum along the way
3. if both are non-null, recurse left and right

**3. What to return to my parent**
- if both children are null, return sum
- if both are non-null, return the larger one
- if one is non-null, return the non null one
 ```
 private helper(TreeNode root, int sum) {
     sum += root.val;
     if (root.left == null && root.right == null) {
         return sum;
     } else if (root.left == null) {
         return helper(root.right, sum);
     } else if (root.right == null) {
         return helper(root.left, sum);
     }
     return Math.max(helper(root.left, sum), helper(root.right, sum));
 }
 ```
 ##### Suffix sum approach
 信息自下而上传递，在base case return 0, 在每一层先call recursion在从用返回的结果update信息
 
**1. What to ask from your left and right child**
left: max single path in my left subtree from root to leaf
right: max single path in my right subtree from root to leaf

**2. What to do at current level**
update prefix sum and continue based on different cases
base cases: 
1. if both lchild and rchild are null, we reached a leaf node, and return the value of the leaf node
2. if one root is non-null, keep recursing for that node, return the result + root.val
3. if both are non-null, recurse left and right

**3. What to return to my parent**
- if both children are null, return root.key
- if both are non-null, return the larger one + root.vale
- if one is non-null, return the non null one + root.val
```
private int helper(TreeNode) {
    if (root.left == null && root.right == null) {
        return root.key;
    } else if (root.left == null) {
        return helper(root.right) + root.val;
    } else if (root.right == null) {
        return helper(root.left) + root.val;
    } 
    return  Math.max(helepr(root.left), helper(root.right)) + root.val;
}
```
### Path Sum to Target (subarray sum)
**Use prefix sum + two sum hashset**
prefix sum: to record the path sum from root to current node, excluding current node
hashset: to store all the prefix sums from previous nodes
for every node, we update the prefix sum and check if prefux_cur - target exists in set, if so, return true. 
Otherwise, we add prefix_cur to set and keep recurse. Recurse only if root.left != null || root.right != null, and return true if any of the child node return true. Otherwise false. 

 **1. What to ask from your left and right child**
left (non-null): whether in the path from root to left, exists a subarray whose sum equal to target
right(non_null): whether in the path from root to right, exists a subarray whose sum equal to target
we pass the prefix sum set to the recursion function, remember to restore set after recursive call

**2. What to do at current level**
update prefix sum and check if Set<prefixsum> contains previfx_cur - target. If so, return true

**3. What to return to my parent**
if there exists prevfix_cur - target in set, return true
if any of helper(root.children, set) returns true, return true
else return false
```
private boolean helper(TreeNode root, int prefixSum, Set<Integer> sums, int target) {
    prefixSum += root.val;
    if (sums.contains(prefixSum - target) {
        return true;
    }
    boolean needRemove = sums.add(prefixSum);
    if (root.left != null && helper(root.left, prefixSum, sums, target)) {
        return true;
    }
    if (root.right != null && helper(root.right, prefixSum, sums, target)) {
        return true;
    }
    if (needRemove) {
        sums.remove(prefixSum);
    }
    // 如果到了leaf node还没找到， return false, 意味着此条root-to-leaf路径找不到
    return false;
}


 
 
