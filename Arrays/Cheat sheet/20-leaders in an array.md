
## PATTERN: Reverse Traversal + Running Maximum (Suffix Maximum)
→ **Trigger:** "when I see an element depends on all elements to its right / compare with every element on the right"

---

## BRUTE FORCE

→ **Idea:** For every element, scan all elements on its right. If no greater element exists, it is a leader.

**Crucial Snippet**

```cpp
for(int i = 0; i < n; i++) {

    bool leader = true;

    for(int j = i + 1; j < n; j++) {

        if(arr[j] > arr[i]) {
            leader = false;
            break;
        }
    }

    if(leader)
        ans.push_back(arr[i]);
}
```

→ **TC / SC:** **O(N²)** / **O(1)** (excluding output)

---

## OPTIMAL

→ **First instinct:** "I immediately traverse from right to left while maintaining the maximum element seen so far."

→ **Core idea:** Maintain a variable `maxRight` representing the largest element seen while traversing from right to left. If `arr[i] >= maxRight`, the current element is a leader, so store it. Then update `maxRight = max(maxRight, arr[i])`. Since leaders are collected from right to left, reverse the answer at the end.

**Crucial Snippets**

```cpp
int maxRight = INT_MIN;
```

```cpp
for(int i = n - 1; i >= 0; i--)
```

```cpp
if(arr[i] >= maxRight)
    ans.push_back(arr[i]);
```

```cpp
maxRight = max(maxRight, arr[i]);
```

```cpp
reverse(ans.begin(), ans.end());
```

→ **TC / SC:** **O(N)** / **O(1)** (excluding output)

---

## WATCH OUT FOR

→ Using `>` instead of `>=`. Equal elements are also leaders.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Brute force checks every element against all elements on its right (O(N²)).
2. We only need the maximum element on the right, not every element.
3. Traverse from right to left while maintaining `maxRight`.
4. If `arr[i] >= maxRight`, store it and update `maxRight`.
5. Reverse the collected leaders since they were added from right to left.
