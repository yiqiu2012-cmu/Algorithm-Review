### Heap
- Heap is a complete binary tree
- The root is a max or min among the nodes values
##### Implementation
implemented as an unsorted array
lchild: my index x 2 + 1
rchild: my index x 2 + 2
parent: (myindex - 1) / 2

##### Operations:
- insert()
- update()
- get/top()
- pop()
- heapify