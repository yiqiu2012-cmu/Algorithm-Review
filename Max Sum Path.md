
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
### Math Sum Path (leaft to root)
##### Prefix sum approach
信息至上而下传递，在每一层都先update信息，把update好的信息传入下一层recursion call, 只在base case return信息
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
 信息只下而上传递，在base case return 0, 在每一层先call recursion在从用返回的结果update信息
 
 