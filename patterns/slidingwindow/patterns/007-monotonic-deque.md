# Monotonic Deque

## 🧠 Pattern

Use when you need the **maximum or minimum value in every fixed-size sliding window**.

> **REMOVE USELESS → ADD → REMOVE OUTDATED → CHECK**

The deque stores **indices**, not values.

For **maximum**:

* Keep values in **decreasing order**.
* The **front** always contains the maximum.

For **minimum**:

* Keep values in **increasing order**.
* The **front** always contains the minimum.

## 💻 Template

### Maximum in Every Window

```js
function maxSlidingWindow(arr, k) {
    const deque = [];
    const answer = [];

    for (let right = 0; right < arr.length; right++) {

        // REMOVE USELESS
        while (
            deque.length > 0 &&
            arr[deque[deque.length - 1]] <= arr[right]
        ) {
            deque.pop();
        }

        // ADD
        deque.push(right);

        // REMOVE OUTDATED
        if (deque[0] <= right - k) {
            deque.shift();
        }

        // CHECK
        if (right >= k - 1) {
            answer.push(arr[deque[0]]);
        }
    }

    return answer;
}
```

# Monotonic Deque — Example Problem

## 🧩 Problem

Given an array of integers `arr` and an integer `k`, find the **maximum value in every subarray of size `k`**.

### Example

```text
Input:

arr = [1, 3, -1, -3, 5, 3, 6, 7]

k = 3

Output:

[3, 3, 5, 5, 6, 7]
```

### Explanation

The windows are:

```text
[1, 3, -1]  → max = 3
[3, -1, -3] → max = 3
[-1, -3, 5] → max = 5
[-3, 5, 3]  → max = 5
[5, 3, 6]   → max = 6
[3, 6, 7]   → max = 7
```

So:

```text
[3, 3, 5, 5, 6, 7]
```

## 💡 Approach

Use a **Monotonic Deque**.

> **REMOVE USELESS → ADD → REMOVE OUTDATED → CHECK**

### 1. REMOVE USELESS

If the new value is bigger than values at the back:

```js
while (
    deque.length > 0 &&
    arr[deque[deque.length - 1]] <= arr[right]
) {
    deque.pop();
}
```

Those smaller values can never become the maximum while the new value is inside the window.

### 2. ADD

Add the new index:

```js
deque.push(right);
```

### 3. REMOVE OUTDATED

If the front index has moved outside the current window:

```js
if (deque[0] <= right - k) {
    deque.shift();
}
```

### 4. CHECK

The front always contains the maximum:

```js
answer.push(arr[deque[0]]);
```

## 🌊 Simple Flow

```text
New element arrives
        ↓
Remove smaller values from BACK
        ↓
Add new index to BACK
        ↓
Remove expired index from FRONT
        ↓
FRONT = maximum
        ↓
Add maximum to answer
```

## 🔍 Why Do We Remove Smaller Values?

Example:

```text
Window: [1, 3, -1]
```

Suppose the deque contains:

```text
[3, -1]
```

Now `5` arrives:

```text
[3, -1, 5]
```

Once `5` enters the window:

```text
3 can never be maximum
-1 can never be maximum
```

So remove them:

```text
[5]
```

Now the maximum is immediately available at the front.

## 🧠 Important Idea

The deque is **not storing every element**.

It only stores **useful candidates** for becoming the maximum.

```text
Normal Queue:

[1, 3, -1, 5, 3]
 ↑ stores everything


Monotonic Deque:

[5, 3]
 ↑
only useful candidates
```

## ⏱️ Complexity

```text
Time:  O(n)

Space: O(k)
```

Why `O(n)`?

Each index:

```text
→ enters deque once
→ leaves deque once
```

So even though there are `while` loops, the total work is `O(n)`.

## 🧠 Remember

```text
Need MAX of every fixed window
            ↓
     Monotonic Deque
            ↓
   Keep values decreasing
            ↓
   FRONT = maximum
            ↓
REMOVE smaller from BACK
            ↓
   ADD new index
            ↓
REMOVE expired from FRONT
            ↓
      CHECK FRONT
```

### 🔑 One-Line Memory

```text
MAX → decreasing deque → FRONT is MAX
MIN → increasing deque → FRONT is MIN
```
