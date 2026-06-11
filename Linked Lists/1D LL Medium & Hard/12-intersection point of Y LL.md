
## PROBLEM:

Given the heads of two singly linked lists that merge at some point forming a Y-shaped structure, return the node where the intersection begins.

Important:

Intersection means:

```cpp
temp1 == temp2
```

NOT:

```cpp
temp1->data == temp2->data
```

The intersecting nodes are the exact same nodes in memory.

Example:

```text
List A:

10 → 15 → 30
      ↑
      Intersection

List B:

3 → 6 → 9 ↗
```

Answer:

```text
15
```

---

## PATTERN:

### Linked List Alignment Pattern

or

### Two Pointer Synchronization Pattern

The goal is to make two pointers reach the same relative position even when the lists have different lengths.

---

## WHY THIS PATTERN:

The challenge is not finding the common node.

The challenge is:

```text
The lists have different lengths.
```

Because of this:

```text
Pointer A may be farther away from the intersection.
Pointer B may be closer to the intersection.
```

We must somehow align them before comparing nodes.

---

## CORE IDEA:

There are two optimal ways:

### Method 1: Length Difference Alignment

Manually align the pointers.

```text
Find lengths
Compute difference
Move longer list ahead
Move together
```

### Method 2: Two Pointer Switching

Automatically align the pointers.

```text
When a pointer reaches NULL,
send it to the other list's head.
```

No length calculation required.

---

## BRUTE FORCE:

### Hashing Approach

#### Idea

Store every node of List A inside a HashSet.

Traverse List B.

The first node already present in the set is the intersection node.

---

### Code

```cpp
Node* intersectPoint(Node* head1, Node* head2)
{
    unordered_set<Node*> st;

    Node* temp = head1;

    while(temp != NULL)
    {
        st.insert(temp);
        temp = temp->next;
    }

    temp = head2;

    while(temp != NULL)
    {
        if(st.find(temp) != st.end())
        {
            return temp;
        }

        temp = temp->next;
    }

    return NULL;
}
```

---

### Dry Run

```text
A: 10 → 15 → 30

B: 3 → 6 → 9 → 15 → 30
```

Store:

```text
{10,15,30}
```

Traverse B:

```text
3  → not found
6  → not found
9  → not found
15 → found
```

Return:

```text
15
```

---

### Complexity

```text
Time  : O(N + M)

Space : O(N)
```

---

# OPTIMAL APPROACH

# Approach 1: Length Difference Method

This is usually the first O(1) space solution most interviewers expect.

---

### Intuition

Suppose:

```text
A: 1 → 2 → 3 → 4 → 5 → 6

B: 9 → 4 → 5 → 6
```

Intersection:

```text
4 → 5 → 6
```

Distance from head to intersection:

```text
A needs 3 steps
B needs 1 step
```

Not aligned.

So if we move both together immediately:

```text
They won't reach the intersection together.
```

---

### Fix

Find lengths.

```text
Length(A)=6
Length(B)=4
```

Difference:

```text
2
```

Move the longer list ahead by:

```text
2 nodes
```

Now both pointers have equal distance remaining.

Move together.

They meet at the intersection.

---

### Algorithm

1. Find length of both lists.
2. Compute difference.
3. Move pointer of longer list ahead by difference.
4. Move both pointers together.
5. First common node is the answer.

---

### Dry Run

```text
A:

1 → 2 → 3 → 4 → 5 → 6

B:

9 → 4 → 5 → 6
```

Lengths:

```text
6 and 4
```

Difference:

```text
2
```

Move longer list ahead:

```text
temp1:

3 → 4 → 5 → 6

temp2:

9 → 4 → 5 → 6
```

Now both have:

```text
4 nodes left
```

Move together:

```text
3 , 9

4 , 4
```

Meet at:

```text
4
```

Return:

```text
4
```

---

### Code

```cpp
class Solution {
public:

    int getLength(Node* head)
    {
        int count = 0;

        while(head != NULL)
        {
            count++;
            head = head->next;
        }

        return count;
    }

    Node* intersectPoint(Node* head1, Node* head2)
    {
        int count1 = getLength(head1);
        int count2 = getLength(head2);

        Node* temp1 = head1;
        Node* temp2 = head2;

        int diff = abs(count1 - count2);

        if(count1 > count2)
        {
            while(diff > 0)
            {
                temp1 = temp1->next;
                diff--;
            }
        }
        else
        {
            while(diff > 0)
            {
                temp2 = temp2->next;
                diff--;
            }
        }

        while(temp1 != temp2)
        {
            temp1 = temp1->next;
            temp2 = temp2->next;
        }

        return temp1;
    }
};
```

---

# Approach 2: Two Pointer Switching Technique

---

### Intuition

Think of two runners.

Runner 1 starts from:

```text
head1
```

Runner 2 starts from:

```text
head2
```

Initially:

```text
One runner may start on the longer list.
One runner may start on the shorter list.
```

So they are not aligned.

Instead of calculating lengths manually:

```text
Let both runners keep walking.
```

When a runner reaches NULL:

```text
Don't stop.

Send it to the other list.
```

---

### Runner Analogy

Example:

```text
A:

1 → 2 → 3 → 4 → 5 → 6

B:

9 → 4 → 5 → 6
```

Runner 1 walks:

```text
1 → 2 → 3 → 4 → 5 → 6
```

Runner 2 walks:

```text
9 → 4 → 5 → 6
```

Runner 1 already covered the extra nodes:

```text
1,2,3
```

Runner 2 only covered:

```text
9
```

Now switch.

Runner 1 starts walking B.

Runner 2 starts walking A.

Eventually:

```text
Both runners have walked through
both lists completely.
```

The alignment happens automatically.

The first common place they can meet is:

```text
The intersection node.
```

---

### Comparison With Length Difference Method

Length Difference Method:

```text
Let's align first.
Then walk together.
```

Switching Method:

```text
Let's walk first.

Alignment will happen automatically.
```

Both solve the same problem.

One does it manually.

One does it automatically.

---

### Algorithm

1. Initialize two pointers.
2. Move both one step at a time.
3. If pointer reaches NULL:
   - redirect to other list's head.
4. Continue until both pointers become equal.
5. Return either pointer.

---

### Dry Run

```text
A:

1 → 2 → 3 → 4 → 5 → 6

B:

9 → 4 → 5 → 6
```

Start:

```text
temp1 = 1
temp2 = 9
```

After traversing:

```text
temp1 reaches NULL
temp2 reaches NULL
```

Switch:

```text
temp1 = head2
temp2 = head1
```

Continue walking.

Eventually:

```text
temp1 = 4
temp2 = 4
```

Loop stops.

Return:

```text
4
```

---

## ALGORITHM:

### Length Difference Method

```text
Find length of both lists.

Find difference.

Move longer list ahead by difference.

Move both together.

Return first common node.
```

### Two Pointer Switching

```text
Initialize two pointers.

Move together.

If pointer reaches NULL:

switch to other list.

Continue until both pointers become equal.

Return either pointer.
```

---

## IMPORTANT CODE SNIPPETS:

### Finding Length

```cpp
int getLength(Node* head)
{
    int count = 0;

    while(head)
    {
        count++;
        head = head->next;
    }

    return count;
}
```

### Aligning Longer List

```cpp
while(diff > 0)
{
    temp1 = temp1->next;
    diff--;
}
```

### Moving Together

```cpp
while(temp1 != temp2)
{
    temp1 = temp1->next;
    temp2 = temp2->next;
}
```

### Pointer Switching

```cpp
temp1 = (temp1 == NULL)
        ? head2
        : temp1->next;

temp2 = (temp2 == NULL)
        ? head1
        : temp2->next;
```

---

## COMMON MISTAKES:

### Mistake 1

Comparing values.

Wrong:

```cpp
temp1->data == temp2->data
```

Correct:

```cpp
temp1 == temp2
```

### Mistake 2

Forgetting:

```cpp
diff--;
```

inside alignment loop.

### Mistake 3

Thinking same values imply intersection.

They do not.

Memory address must be same.

---

## WHY I MIGHT FORGET THIS:

Because the switching solution feels like magic.

Remember:

```text
The switch is not finding the intersection.

The switch is fixing the alignment problem.
```

Once aligned:

```text
Both pointers naturally meet.
```

---

## INTERVIEW FLOW:

1. Explain hashing approach.
2. Mention O(N) extra space.
3. Observe that the real issue is different lengths.
4. Introduce Length Difference Method.
5. Explain manual alignment.
6. Mention O(1) space.
7. Then discuss Two Pointer Switching.
8. Explain runner analogy.
9. Explain automatic alignment.
10. Write final optimal code.

---

## TIME COMPLEXITY:

### Hashing

```text
O(N + M)
```

### Length Difference

```text
O(N + M)
```

Reason:

```text
Length calculation = O(N+M)

Alignment = O(|N-M|)

Final traversal = O(min(N,M))
```

Overall:

```text
O(N + M)
```

### Two Pointer Switching

Each pointer traverses at most:

```text
List A + List B
```

Therefore:

```text
O(N + M)
```

---

## SPACE COMPLEXITY:

### Hashing

```text
O(N)
```

### Length Difference

```text
O(1)
```

### Two Pointer Switching

```text
O(1)
```

---

## EDGE CASES:

### Intersection at head

```text
A: 1 → 2 → 3
B: 1 → 2 → 3
```

Answer:

```text
1
```

### One list much longer

```text
A: 1 → 2 → 3 → 4 → 5 → 6 → 7
B: 5 → 6 → 7
```

### Intersection at last node

```text
A: 1 → 2 → 8
B: 4 → 5 → 8
```

Answer:

```text
8
```

---

## PATTERN RECOGNITION:

Look for:

```text
Two linked lists

Different lengths

Need to find common node

Need to synchronize pointers
```

Keywords:

```text
Intersection

Merge Point

Y-Shaped Linked List

Common Node

Shared Tail
```

Whenever you see these:

```text
Think:

1. Length Difference Alignment

or

2. Two Pointer Switching
```

---

# CLEAN C++ CODE (OPTIMAL)

```cpp
class Solution {
public:

    Node* intersectPoint(Node* head1, Node* head2)
    {
        Node* temp1 = head1;
        Node* temp2 = head2;

        while(temp1 != temp2)
        {
            temp1 = (temp1 == NULL)
                    ? head2
                    : temp1->next;

            temp2 = (temp2 == NULL)
                    ? head1
                    : temp2->next;
        }

        return temp1;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
Node* temp1 = head1;
Node* temp2 = head2;
```

Start traversal from both lists.

```cpp
while(temp1 != temp2)
```

Keep moving until both pointers reach the same node.

```cpp
temp1 = (temp1 == NULL)
        ? head2
        : temp1->next;
```

If List A ends, start traversing List B.

```cpp
temp2 = (temp2 == NULL)
        ? head1
        : temp2->next;
```

If List B ends, start traversing List A.

```cpp
return temp1;
```

At this point:

```cpp
temp1 == temp2
```

Both point to the intersection node.

---

# EASY-TO-REMEMBER SUMMARY

```text
Hashing:
Store nodes of first list.

Length Difference:
Align manually.
Then walk together.

Two Pointer Switching:
Walk first.
Alignment happens automatically.

Golden Rule:

Compare nodes,
not node values.

temp1 == temp2

NOT

temp1->data == temp2->data
```
````
