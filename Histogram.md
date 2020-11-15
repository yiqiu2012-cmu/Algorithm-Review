
# Largest Rectangle
data structure: to maintain a stack to make sure the columns whose indices are stored in the stack form an ascending order
When poping an element from the stack, the element's right border equals the current index - 1, the leftborder of the element == the index of the element on top of the stack + 1
Time = O(n) because every single element can only be inserted and poppped out of the stack once and only once. 
每次check一个column, 和他的前一个column. 如果比前一个矮，前一个的右边界可以确定。如果比前一个高，当前的左边界可以确定