

## PROBLEM:

Implement a **Min Heap** from scratch that supports the following operations:

- `push(x)` → Insert an element into the heap.
- `pop()` → Remove the minimum element (root).
- `peek()` → Return the minimum element.
- `size()` → Return the number of elements.

The heap must always maintain the **Min Heap property**.

---

## PATTERN:

**Heap Implementation (Array Representation + Heapify)**

Sub-patterns:

- **Heapify Up (Bubble Up)** → Insertion
- **Heapify Down (Bubble Down)** → Deletion

---

## WHY THIS PATTERN:

A **Heap** is the correct data structure because:

- We need fast insertion.
- We need fast deletion of the minimum element.
- We need constant-time access to the minimum element.

A **Min Heap** guarantees:

- Minimum element is always at the root.
- Insert → **O(log n)**
- Delete Min → **O(log n)**
- Peek Min → **O(1)**

A sorted array makes insertion **O(n)**.

A BST does not guarantee balanced height.

Hence, a **Min Heap** is the ideal choice.

---

## CORE IDEA:

Store the **Complete Binary Tree** inside a vector.

The heap always satisfies:

```
Parent ≤ Left Child
Parent ≤ Right Child
```

Parent and child indices:

```cpp
Parent = (i - 1) / 2
Left Child = 2 * i + 1
Right Child = 2 * i + 2
```

Two operations maintain this property:

1. **Heapify Up**
   - Used after insertion.

2. **Heapify Down**
   - Used after deletion.

---

## BRUTE FORCE:

**Not required.**

There is no meaningful optimization progression here.

The expected interview solution is direct Heap implementation.

---

## OPTIMAL APPROACH:

Maintain the heap inside a vector.

### `push(x)`

- Insert at the end.
- The inserted element may violate the heap property with its parent.
- Keep swapping upward until the property is restored.

(**Heapify Up**)

### `pop()`

- Replace the root with the last element.
- Remove the last element.
- The new root may violate the heap property with its children.
- Keep swapping downward until the property is restored.

(**Heapify Down**)

### `peek()`

Return `heap[0]`.

### `size()`

Return `heap.size()`.

---

## ALGORITHM:

### `push(x)`

1. Insert `x` at the end of the vector.
2. Let `i` be the last index.
3. While `i` is not the root:
   - Find parent.
   - If parent ≤ current:
     - Stop.
   - Else:
     - Swap.
     - Move to parent.
4. Heap property restored.

---

### `pop()`

1. If heap is empty:
   - Return.
2. Replace root with last element.
3. Remove last element.
4. Start from the root.
5. Repeatedly:
   - Find left child.
   - Find right child.
   - Find the smallest among:
     - Current
     - Left
     - Right
   - If current is already smallest:
     - Stop.
   - Otherwise:
     - Swap with smaller child.
     - Continue from child.
6. Heap property restored.

---

## DRY RUN:

Initial Heap

```
      2
    /   \
   5     8
  / \
 9  10
```

Vector

```
{2,5,8,9,10}
```

Operation

```
pop()
```

### Step 1

Replace root with last element.

```
{10,5,8,9,10}
```

### Step 2

Remove last element.

```
{10,5,8,9}
```

### Step 3

Current = 10

Children:

```
5
8
```

Smaller child = 5

Swap.

```
{5,10,8,9}
```

### Step 4

Current = 10

Child:

```
9
```

Swap.

```
{5,9,8,10}
```

### Step 5

Current = 10

No children.

Stop.

Final Heap

```
{5,9,8,10}
```

---

## IMPORTANT CODE SNIPPETS:

### Parent

```cpp
parent = (i - 1) / 2;
```

### Left Child

```cpp
left = 2 * i + 1;
```

### Right Child

```cpp
right = 2 * i + 2;
```

### Heapify Up

```cpp
while (i > 0)
{
    int parent = (i - 1) / 2;

    if (heap[parent] <= heap[i])
        break;

    swap(heap[parent], heap[i]);

    i = parent;
}
```

### Heapify Down

```cpp
while (true)
{
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    int smallest = i;

    if (left < heap.size() && heap[left] < heap[smallest])
        smallest = left;

    if (right < heap.size() && heap[right] < heap[smallest])
        smallest = right;

    if (smallest == i)
        break;

    swap(heap[i], heap[smallest]);

    i = smallest;
}
```

---

## COMMON MISTAKES:

1. Forgetting:

```cpp
i = parent;
```

after Heapify Up.

2. Forgetting:

```cpp
i = smallest;
```

after Heapify Down.

3. Swapping with the larger child instead of the smaller child.

4. Accessing children without checking bounds.

5. Using `erase(begin())` instead of replacing the root with the last element.

6. Assuming the last element is always the largest (it is not).

7. Forgetting that the heap is represented as a complete binary tree inside an array.

---

## WHY I MIGHT FORGET THIS:

Because people try to memorize the code instead of understanding what is moving.

Remember:

### `push()`

A newly inserted element may be too small.

It moves **UP**.

### `pop()`

The replaced root may be too large.

It moves **DOWN**.

Think of:

- **Heapify Up** → A balloon floating upward.
- **Heapify Down** → A heavy rock sinking downward.

---

## INTERVIEW FLOW:

1. A Heap is stored using an array because it is a Complete Binary Tree.
2. Explain the index formulas.
3. Explain the Min Heap property.
4. Explain `push()`:
   - Insert at end.
   - Heapify Up.
5. Explain `pop()`:
   - Replace root with last element.
   - Delete last element.
   - Heapify Down.
6. Explain `peek()`.
7. Explain `size()`.
8. State complexities.

---

## TIME COMPLEXITY:

### `push()`

- Insert → **O(1)**
- Heapify Up → **O(log n)**

Overall:

```
O(log n)
```

---

### `pop()`

- Replace root → **O(1)**
- Delete last → **O(1)**
- Heapify Down → **O(log n)**

Overall:

```
O(log n)
```

---

### `peek()`

```
O(1)
```

---

### `size()`

```
O(1)
```

---

## SPACE COMPLEXITY:

Auxiliary Space

- `push()` → **O(1)**
- `pop()` → **O(1)**
- `peek()` → **O(1)**
- `size()` → **O(1)**

Reason:

Only a constant number of variables are used.

The heap itself occupies **O(n)** space to store `n` elements.

---

## EDGE CASES:

1. Empty heap.
2. Heap with one element.
3. Duplicate values.
4. Inserting the smallest element.
5. Removing until the heap becomes empty.

---

## PATTERN RECOGNITION:

Use this pattern whenever:

- You need repeated access to the smallest or largest element.
- You need efficient insertion and deletion.
- The problem mentions:
  - Priority
  - K Smallest/Largest
  - Top K
  - Running Minimum/Maximum
  - Median
  - Merge K Structures
  - Priority Queue

Recognition clues:

- Need minimum repeatedly → **Min Heap**
- Need maximum repeatedly → **Max Heap**
- Need both minimum and maximum simultaneously → **Two Heaps**
- Need Top K elements → **Fixed-size Heap**

Underlying Heap Pattern:

- Array-based Complete Binary Tree.
- Heapify Up after insertion.
- Heapify Down after deletion.
- Maintain the Heap Invariant:
  - **Min Heap:** Parent ≤ Children.
  - **Max Heap:** Parent ≥ Children.

---

# Clean C++ Code

```cpp
class minHeap {
private:
    vector<int> heap;

public:

    void push(int x) {
        heap.push_back(x);

        int i = heap.size() - 1;

        while (i > 0) {
            int parent = (i - 1) / 2;

            if (heap[parent] <= heap[i])
                break;

            swap(heap[parent], heap[i]);

            i = parent;
        }
    }

    void pop() {
        if (heap.empty())
            return;

        heap[0] = heap.back();
        heap.pop_back();

        int i = 0;

        while (true) {
            int left = 2 * i + 1;
            int right = 2 * i + 2;

            int smallest = i;

            if (left < heap.size() && heap[left] < heap[smallest])
                smallest = left;

            if (right < heap.size() && heap[right] < heap[smallest])
                smallest = right;

            if (smallest == i)
                break;

            swap(heap[i], heap[smallest]);

            i = smallest;
        }
    }

    int peek() {
        if (heap.empty())
            return -1;

        return heap[0];
    }

    int size() {
        return heap.size();
    }
};
```

---

# Intuition Behind Every Important Line

### `heap.push_back(x)`

Preserve the Complete Binary Tree by inserting at the next available position.

### `i = heap.size() - 1`

Start tracking the newly inserted element.

### `parent = (i - 1) / 2`

Find the parent in the array representation.

### `heap[parent] <= heap[i]`

If the heap property already holds, stop.

### `swap(...)`

Restore the heap property one level at a time.

### `i = parent`

Continue tracking the inserted element after it moves.

---

### For `pop()`

### `heap[0] = heap.back()`

Preserve the tree shape by replacing the root with the last element.

### `heap.pop_back()`

Remove the duplicate last element.

### `left/right`

Locate the children.

### `smallest = i`

Assume the current node is correct.

### Compare with children

Find where the heap property is violated.

### `smallest == i`

Heap property restored.

### `swap(...)`

Push the violating element downward.

### `i = smallest`

Continue tracking the moved element.

---

# Easy-to-Remember Summary

- Heap = Complete Binary Tree stored in a vector.
- `push()` = Insert at the end → Bubble Up.
- `pop()` = Replace root with last element → Bubble Down.
- `peek()` = Root.
- `size()` = Vector size.
- Heapify Up fixes parent-child violations after insertion.
- Heapify Down fixes parent-child violations after deletion.
- Always maintain the Min Heap invariant:

```
Parent ≤ Children
```
````
