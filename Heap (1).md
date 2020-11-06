### Heap
- Heap is a complete binary tree
- The root is a max or min among the nodes values
##### Implementation
implemented as an unsorted array
lchild: my index x 2 + 1
rchild: my index x 2 + 2
parent: (myindex - 1) / 2

##### Operations:
- offer()-percolateUp
- poll()-percolateDown
- peek()
- heapify()

**percolateDown:**: 
0. place the last element in the array to the root
1. startnig from the root, compare with its two children nodes. If the smallest among the two is smaller than the element, swap the element with that child. Do this until the element does not need to be moved

**percolateUp:**: 
compare the element with its parent, if the element is smaller than the parent, swap element with its parent. Do this until the element does not need to be mvoed

**Heapify**
Staring from the first non-leaf element in the array, from right to left, do percolateDown for each element
**If an array has n element, the range of indices that need to perform percolateDown is [0, n/2-1].

##### Implementing a heap
**Always implement percolateUp and percolateDown first because offer, poll, peek depends on them
**In offer and poll, update size before enter percolates because percolate uses size to index and check bounds
**offer():** 
1. Check if size == array.length, if so, expand array
2. append to the end of array and percolateUp

**poll():**
1. Store the root
2. Move the last element to array[0]
3. percolateDown

**percolateUp:**
1. while index > 0
2. find parentIndex, if element is smaller than array[parent], swap, if not, break
3. update index to parentIndex

**percolateDown:**
1. while index <= size / 2 - 1 --> first non-leaf node
2. get leftchild, and rightchild, candidate equals leftchild index
3. update candidate if rightchild <= size -1 && array[left] > array[right] --> find the smaller one among the two children
4. swap of element is larger than candidate, else break
5. update index to candidate 

**Update(index, val):**
1. return the old value

```
public class MinHeap{
    private int[] array;
    private int size;
    public MinHeap(int[] array) {
        if (array == null || array.length == 0) {
            throw new IllegalArgumentException("input array can not be null or empty");
        }
        this.array = array;
        this.size = array.length;
        heapify();
    }
    private void percolateUp(int index) {
        while (index > 0) {
            int parentIndex = (index - 1) / 2;
            if (array[parentIndex] > array[index]) {
                swap(parentIndex, index);
            } else {
                break;
            }
            index = parentIndex;
        }
    }
    private void percolateDown(int index) {
        while (index <= size / 2 - 1) {
            int left = index * 2 + 1;
            int right = index * 2 + 2;
            int candidate = left;
            // Update candidate if right exists and it is smaller than left
            if (right <= size - 1 && array[left] >= array[right]) {
                candidate = right;
            }
            if (array[index] > array[candidate]) {
                swap(index, candidate);
            } else {
                break;
            }
            index = candidate;
        }
    }
    private void swap(one, two) {
        int temp = array[one];
        array[one] = array[two];
        array[two] = temp;
    }
    public void offer(int val) {
        if (size == array.length) {
            array = Arrays.copyof(array, (int)array.length * 1.5)
        }
        array[size] = val;
        size++;
        percolateUp(size - 1);
    }
    public int poll(int val) {
        int res = array[0];
        array[0] = array[size-1];
        size--;
        percolateDown(0);
        return result;
    }
    public int peek() {
        return array[0];
    }

    public void update(int index, int val) {
        int old = array[index];
        array[index] = value
        if (val < old) {
            percolateUp(index);
        } else if (val > old) {
            percolateDown(index);
        }
        return old;
    }
    public void heapify() {
        for (int i = size / 2 - 1; i >= 0; i--) {
            percolateDown(i);
        }
    }
}
```

