
# Check if an Array is a Max Heap / Min Heap

### PROBLEM:

Given an array representing the **level-order traversal** of a complete binary tree, determine whether it satisfies the **Heap Property**.

- **Max Heap:** Every parent is **greater than or equal to** its children.
- **Min Heap:** Every parent is **less than or equal to** its children.

Return `true` if the given array represents the required heap; otherwise, return `false`.

---

## PATTERN:

**Heap Validation using Array Representation**

---

## WHY THIS PATTERN:

A Heap is always a **Complete Binary Tree**.

A complete binary tree can be stored directly in an array, so there is **no need to build the tree**.

Children can be accessed directly using indices:

- Left Child = `2*i + 1`
- Right Child = `2*i + 2`

Therefore, we simply verify the Heap property for every parent.

---

## CORE IDEA:

The array already represents the Heap.

For every parent node:

- Find its left child.
- Find its right child.
- Compare the parent with its children.

### Max Heap

```
Parent >= Left Child
Parent >= Right Child
```

### Min Heap

```
Parent <= Left Child
Parent <= Right Child
```

If any comparison fails, the Heap property is violated.

> **Important:** We are **not using a Heap (priority_queue)**. We are only checking whether the given array already satisfies the Heap property.

---

## BRUTE FORCE:

There is **no meaningful brute-force solution**.

The direct traversal itself is already the simplest and optimal approach.

---

## OPTIMAL APPROACH:

Iterate through all **non-leaf nodes**.

For every parent:

- Compute left child index.
- Compute right child index.
- Compare parent with existing children.
- Return `false` immediately if the Heap property is violated.

Otherwise return `true`.

---

## ALGORITHM:

### Max Heap

1. Iterate from `0` to `(n/2)-1`.
2. Compute left and right child.
3. If

```cpp
arr[left] > arr[parent]
```

return `false`.

4. If

```cpp
arr[right] > arr[parent]
```

return `false`.

5. Return `true`.

---

### Min Heap

1. Iterate from `0` to `(n/2)-1`.
2. Compute left and right child.
3. If

```cpp
arr[left] < arr[parent]
```

return `false`.

4. If

```cpp
arr[right] < arr[parent]
```

return `false`.

5. Return `true`.

---

## DRY RUN

### Max Heap Example

```
arr = [90,15,10,7,12,2]

            90
          /    \
        15      10
       /  \    /
      7   12  2
```

### i = 0

```
90 >= 15 ✔
90 >= 10 ✔
```

---

### i = 1

```
15 >= 7 ✔
15 >= 12 ✔
```

---

### i = 2

```
10 >= 2 ✔
```

Return **true**.

---

### Max Heap Invalid

```
arr = [9,15,10]

        9
      /   \
    15    10
```

```
15 > 9
```

Violation.

Return **false**.

---

### Min Heap Example

```
arr = [5,8,10,15,20]

        5
      /   \
     8     10
    / \
   15 20
```

### i = 0

```
5 <= 8 ✔
5 <= 10 ✔
```

---

### i = 1

```
8 <= 15 ✔
8 <= 20 ✔
```

Return **true**.

---

### Min Heap Invalid

```
arr = [10,5,20]

       10
      /  \
     5   20
```

```
5 < 10
```

Violation.

Return **false**.

---

# IMPORTANT CODE SNIPPETS

## Left Child

```cpp
int left = 2*i + 1;
```

---

## Right Child

```cpp
int right = 2*i + 2;
```

---

## Max Heap Validation

```cpp
if(left < n && arr[left] > arr[i])
    return false;

if(right < n && arr[right] > arr[i])
    return false;
```

---

## Min Heap Validation

```cpp
if(left < n && arr[left] < arr[i])
    return false;

if(right < n && arr[right] < arr[i])
    return false;
```

---

## Iterate Only Non-Leaf Nodes

```cpp
for(int i=0;i<=(n/2)-1;i++)
```

Reason:

Leaf nodes have no children.

Only parent nodes can violate the Heap property.

---

# COMMON MISTAKES

### Mistake 1

Using wrong child indices.

Correct:

```
Left = 2*i+1
Right = 2*i+2
```

---

### Mistake 2

Forgetting boundary checks.

Always check:

```cpp
left < n
right < n
```

---

### Mistake 3

Checking leaf nodes.

Leaf nodes have no children.

Only non-leaf nodes need validation.

---

### Mistake 4

Building the binary tree first.

Not needed.

The array itself is already the tree.

---

### Mistake 5

Confusing Heap validation with Heap operations.

No insertion.

No deletion.

No heapify.

Only validation.

---

### Mistake 6

Using the wrong comparison operator.

For Max Heap:

```cpp
child > parent
```

For Min Heap:

```cpp
child < parent
```

Only the comparison changes.

---

# WHY I MIGHT FORGET THIS

Whenever I see "Heap", I immediately think of `priority_queue`.

But this question never asks me to use one.

Instead:

> Treat the array as a complete binary tree and verify the parent-child relationship.

Remember:

**Heap Validation ≠ Heap Operations**

---

# INTERVIEW FLOW

> The given array already represents the level-order traversal of a complete binary tree. Since a Heap is stored as an array, I can directly compute the left and right child indices using `2*i+1` and `2*i+2`. Then I iterate through all non-leaf nodes and compare each parent with its children. For a Max Heap, every parent should be greater than or equal to its children. For a Min Heap, every parent should be less than or equal to its children. If any comparison fails, I return `false`; otherwise, the array represents a valid Heap.

---

# TIME COMPLEXITY

### Time Complexity: **O(n)**

Only parent nodes are checked.

There are approximately

```
n/2
```

parents.

Each parent performs at most **2 constant-time comparisons**.

```
(n/2) × O(1)

= O(n)
```

### Why not O(log n)?

Because we are **checking every parent node**, not moving along one root-to-leaf path.

`O(log n)` is for operations like:

- Insert
- Delete
- Heapify

Those traverse only one path.

Here, a violation can occur anywhere, so every parent must be inspected.

---

# SPACE COMPLEXITY

### O(1)

No extra space is used.

Only a few integer variables.

---

# EDGE CASES

### Single Element

```
[10]
```

Valid Max Heap and Min Heap.

---

### Two Elements

```
[10,5]
```

Valid Max Heap.

---

```
[5,10]
```

Valid Min Heap.

---

### Equal Elements

```
[10,10,10]
```

Valid for both.

Heap property allows equality.

---

### Root Violates Property

```
[5,10]
```

Invalid Max Heap.

---

```
[10,5]
```

Invalid Min Heap.

---

### Last Parent Has Only Left Child

Always check:

```cpp
left < n
right < n
```

---

# PATTERN RECOGNITION

Use this pattern whenever:

- The input is an array representing a Heap.
- The question asks whether the Heap property holds.
- Children are accessed using indices.
- No insertion or deletion is performed.
- The problem asks to validate a Heap.

---

# CLEAN C++ CODE (MAX HEAP)

```cpp
class Solution {
public:
    bool isMaxHeap(vector<int>& arr) {
        int n = arr.size();

        for(int i = 0; i <= (n/2)-1; i++) {

            int left = 2*i + 1;
            int right = 2*i + 2;

            if(left < n && arr[left] > arr[i])
                return false;

            if(right < n && arr[right] > arr[i])
                return false;
        }

        return true;
    }
};
```

---

# CLEAN C++ CODE (MIN HEAP)

```cpp
bool isMinHeap(vector<int>& arr) {

    int n = arr.size();

    for(int i = 0; i <= (n/2)-1; i++) {

        int left = 2*i + 1;
        int right = 2*i + 2;

        if(left < n && arr[left] < arr[i])
            return false;

        if(right < n && arr[right] < arr[i])
            return false;
    }

    return true;
}
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### `int left = 2*i + 1`

Find the left child.

---

### `int right = 2*i + 2`

Find the right child.

---

### `left < n`

Ensures the child exists.

---

### Max Heap

```cpp
arr[left] > arr[i]
```

If a child is greater than its parent,

the Max Heap property is violated.

---

### Min Heap

```cpp
arr[left] < arr[i]
```

If a child is smaller than its parent,

the Min Heap property is violated.

---

### `(n/2)-1`

Last non-leaf node.

Only parent nodes need checking.

---

# EXPLAIN EVERY TRICKY CONDITION

### Why iterate only till `(n/2)-1`?

Every node after that is a leaf.

Leaf nodes have no children.

---

### Why check `left < n`?

The last parent might have only one child.

---

### Why use `>` for Max Heap?

Because

```
Parent >= Child
```

Equality is allowed.

Only

```
Child > Parent
```

is invalid.

---

### Why use `<` for Min Heap?

Because

```
Parent <= Child
```

Equality is allowed.

Only

```
Child < Parent
```

is invalid.

---

# EASY-TO-REMEMBER SUMMARY

- Heap is already stored as an array.
- Never build the tree.
- Left = `2*i+1`
- Right = `2*i+2`
- Check only non-leaf nodes.
- Max Heap → child should never be greater than parent.
- Min Heap → child should never be smaller than parent.
- Time = **O(n)**
- Space = **O(1)**

---

# HEAP-SPECIFIC NOTES

### Why is a Heap the correct data structure?

Because the input itself is already the array representation of a Heap.

---

### Which Heap?

Depends on the question.

- Max Heap → Parent ≥ Children
- Min Heap → Parent ≤ Children

---

### What is stored?

The array stores the Heap in level-order.

---

### Why are elements pushed?

Not applicable.

---

### Why are elements popped?

Not applicable.

---

### What invariant is maintained?

**Max Heap**

```
Parent >= Left Child
Parent >= Right Child
```

**Min Heap**

```
Parent <= Left Child
Parent <= Right Child
```

---

### Fixed-Size Heap (Top K)?

Not applicable.

This is purely a Heap validation problem.
````
