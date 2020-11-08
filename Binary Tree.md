# Binary Tree
### Concepts
**Binary search tree:** binary tree in which for every single node, the values in its left subtree are all smaller than its value, and the values in its right subtree are all larger than its value
**Balanced binary tree:** binary tree in which the height of left and right subtree of every node differ by 1 or less
**Complete binary tree:** a binary tree in which every level, except possibly the last, is completely filled, and all the nodes are as far left as possible

- Use recursion to solve 
    - Each node in the recursion tree is a function call, asking for information recursively
    - Base case is always null
    - Consider
        - What do I want to ask my children
        - what do I do with the answer from my children
        - what do I return to my parents
    - **How to analyze Big-O complexity**
        - **Time complexity**: the sum of time complexity of **ALL nodes in recursion tree**
        - **Space complexity**: the sum of the space complexity of **ALL node on a path from top to bottom** 
    - ### **For tree problems, space is O(height)!**

** getHeight**

**LeftChild:**
```
getHeight(leftChild)
```
**rightChild:**
```
getHeight(rightChild)
```
**root:**
```
return max(getHeight(left), getHeight(right)) + 1
```
The input tree is the recursion tree. Each node has O(n) time, there are n nodes in the recursion tree. The height of the recursion is, at worst, n. 
**Time: O(n)**
**Space: O(n)**


### isBalanced
To check if a tree is balanced, we need to consider three criteria:
- the height of the left subtree must be < 1 dfferent from the height of the right tree (here we need to get the height of both subtrees)
- both the right and left subtrees are balanced

**root: is the height difference between my left and right children equal or smaller than 1**
```
getHeight(left) - getHeight(right) <= 1
```
**leftChild: is my left subtree balanced**
```
isBalanced(leftChild)
```
**rightChild: is my right subtree balanced**
```
isBalanced(rightChild)
```
The input tree is the recursion tree (check parameter), there are n node in the recursion tree. At each node, the time complexity is O(2n) because of 2 getHeight() call (each is O(n)). Space is the height of the tree, which is O(n) in worst case
**Time:** O(nlogn)
**Space:** O(height)

### isSymmetrical
The input tree is different from recursion tree. To check if a tree is symmetrical, we need to take two nodes for comparison. Root node is the left and right child of the input root. There are four cases:
- when both nodes in comparison are null-->true
- when one of the node is not null-->false
- when both nodes contain different values-->false
- when both nodes contain the same values, continue to check the subtrees (recursive fn call)
There are n/2 nodes in recursion tree, each has O(1) time, so the time complexity is O(n/2)=O(n). Space is O(height)
```
isSymmetrical(TreeNode left, TreeNode right) {
    if (left == null && right == null) {
        return true;
    } else if (left == null || right == null) {
        return false;
    } else if (left.val != right.val) {
        return false;
    } 
    return isSymmetrical(left.left, right.right) && isSymmetrical(left.right, right.left);
}
```
**Time:** O(n)
**Space:** O(height)

### isTweaked
Two trees are tweaked when they are identical or symmetrical
- Identical: one.left, two.left && one.right, two.right
- Symmetrical: one.left, two.right && one.right, two.left

Either one is met, the two trees are tweaked

In recursion tree has four branches, each brach check two nodes, its two pairs of && condition, either one is met will lead to true
For a multibrach recursion tree, the total number of nodes linearly scales with the number of nodes on the last level. In this example, the recursion is a quadrual tree, therefore the # of nodes scales with level by a factor 4 (1,4,16,64...). The height of the recursion tree is the same as that of the input tree. Last level has 4^log_2(n) nodes. 
```
isTweaked(TreeNode left, TreeNode right) {
    if (left = null && right = null) {
        return true;
    } else if (left = null || right = null) {
        return false;
    } else if (left.val != right.val) {
        return false;
    } 
    return isTweaked(left.left, right.left) && isTweaked(left.right, right.right) 
    || isTweaked(left.left, right.right) && isTweaked(left.right, right.left);
}
```
**Time:** 4^log(n) = O(n^2) **Note: if the trees are not balanced, time << O(n^2)
**Space:** log(n)

### isBST
Take three argument, root, min_value, max value. We pass the boundary along as we go down the recursion tree, and return the answer back to parent. For each node, we check if its value is within the boundary passed down to it. Each node's value delimits the boundary of next level. 
```
isBST(Treenode root, int min, int max) {
    if (root == NULL) {
        return true;
    }
    if (root.val >= max || root.val <= min) {
        return false;
    } else {
        return isBST(root.left, min, root.val) && isBST(root.right, root.val, max);
    }
}
```
**Time:** O(n)
**Space:** O(height)

## Binary Search Tree
Search time is O(height) at best, O(logn) for balanced BST. When searching in BST, we can discard half of the nodes at current node. 
##### Search()
This function takes the root of a BST, and a value to searhc for, returns the node that contains that value

**Recursive rule:**
- Base case: when root == null or root.val == input
- Recursive rule: if root.val > input, go to the left subtree. if root.val < input, go to the right subtree
```
public TreeNode Search(TreeNode root, int val) {
    if (root == null || root.val == val) {
        return root;
    }
    if (val > root.val) {
        return Search(root.right, val);
    } else if (val < root.val) {
        return Search(root.left, val);
    }
}
```
**Time:** O(height), single branched tree
**Space:** O(height), single branched tree

##### This is a tail recursion, can be easily transformed to iterative! 

##### Iterative search
The base case will be the terminating condition of a while loop, recursive call will be the opertaion inside the while loop
```
public TreeNode Search(TreeNode root, int val) {
    while (root != null && root.val != val) {
        if (val > root.val) {
            root = root.right;
        } else if (val < root.val) {
            root = root.left;
        }
    }
    return root;
}
```

### Insert()
Search the BST, if there exists the val, return root, if not, new TreeNode and append
- how to traverse
    - if val > node.val, go right, else go left
- what to do with the return value of previous call stack  
    - append to root
- what to return to my parent
    - current root

##### Recursion
Base case: new TreeNode and return
Recursive rule:
- Traversal: if val > node, go right else go right
- What to do with the return value: append to tree

At current level: return root
```
public TreeNode insert(TreeNode root, int val) {
    if (root == null) {
        return new TreeNode(val);
    }
    if (val < root.val) {
        root.left = insert(root.left, val);
    } else if (val > root.val) {
        root.right = insert(root.right, val);
    return root;
}
```
**Time:** O(h)
**Space:** O(h)

##### Tail Recursion
Use a helper function that solely modify the tree. Return the root of the new tree outside the recursive function. 
```
public TreeNode insert(TreeNode root, int val) {
    if (root == null) {
        return new TreeNode(val);
    }
    helper(TreeNode root, int val);
    return root;
}
private void helper(TreeNode root, int val) {
    if (root == null) {
        return;
    }
    while(root != null) {
       if (val < root.val) {
           if (root.left == null) {
               root.left = new TreeNode(val);
           } else {
               helper(root.left);
           }
       } else if (val > root.val){
           if (root.right == null) {
               root.right = new Treenode(val);
           } else {
               helper(root.right);
           }
       }
    }
}
```
##### Iterative
```
public TreeNode insert(TreeNode root, int val) {
    if (root == null) {
        return new TreeNode(val);
    }
    TreeNode cur = root;
    while (cur.key != target) {
        if (cur.val < target) {
            if (cur.right == null) {
                cur.right = new TreeNode(val);
                break;
            } else {
                cur = cur.right;
            }
        } else if (cur.val > target) {
            if (cur.left == null) {
                cur.left = new TreeNode(val);
                break;
            } else {
                cur = cur.left;
            }
        } 
    }
    return root;
}
```

### Delete()
1. search
2. delete
    - case 1: the root has no children, delete
    - case 2,3: the root has one children, append
    - case 4: the root has two children-->find the smallest in the right subtree and append
        - 4.1 the root.right has no left children --> root.right is the smallest in right subtree, take over root.left, append root.right
        - 4.2 root.right has left chidren, search through root.left subtree, find the node that has no left children, append node.right, take over root.left, root.right.right, append node

### Max sum path
Clarification:
    input: root
    output: max sum from one node to another
Assumption:
    if root == null, return special value --> Integer.MIN_VALUE because max sum might be negative number
Reasoning:
High-level: Use recursion to solve tree porblem
Mid-level: 
1. What to ask from my left and right child
- the max sum from root to leaf
2. What to do at current level
- calculate leftmax + rightmax + rootval, update globalMax as needed (this is because the root does not need to be included in the path)
3. What to return to my parent
- max(left, right) + node.val

There are two types of problem having different requirements
1. The path must be from a leaf node to another leaf node
2. The path can be from any node to any node

For type 1:
1.  What to ask from my left and right child
left and right must include leaf node, therefore they are simply the return value of helper
2. What to do at current level
only when both root.left and root.right are non-null can we calculate left + right + node.val and update globalmax
3. What to return to my parent
for the case where one of the child nodes is null, return the other path to parent.
when both are non-null, return max(left, right) + node.val

For type 2: 
1.  What to ask from my left and right child
The path does not need to include leaf, therefore, can start from current root
```
left = max(helper(), 0)
right = max(helper(), 0)
```
2. What to do at current level
add left, right, node.val and update globalmax discriminatively
3. What to return to my parent
max(left, right) + node.val
