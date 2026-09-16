# Exactly K / Counting Sliding Window

## 🧠 Pattern

Use when you need to **count subarrays / substrings that contain exactly `k` elements or satisfy exactly `k` conditions**.

A common trick is:

> **EXACTLY K = AT MOST K − AT MOST (K − 1)**

For counting valid subarrays:

> **EXPAND → SHRINK → COUNT**

## 💻 Template

```js
function exactlyK(arr, k) {

    function atMostK(k) {
        let left = 0;
        let count = 0;
        const freq = new Map();

        for (let right = 0; right < arr.length; right++) {

            // ADD
            freq.set(arr[right], (freq.get(arr[right]) || 0) + 1);

            // SHRINK
            while (freq.size > k) {
                freq.set(arr[left], freq.get(arr[left]) - 1);

                if (freq.get(arr[left]) === 0) {
                    freq.delete(arr[left]);
                }

                left++;
            }

            // COUNT all valid windows ending at right
            count += right - left + 1;
        }

        return count;
    }

    return atMostK(k) - atMostK(k - 1);
}
```

# Exactly K / Counting — Example Problem

## 🧩 Problem

Given an array `arr` and an integer `k`, count the number of **subarrays containing exactly `k` distinct integers**.

### Example

```text
Input:
arr = [1, 2, 1, 2, 3]
k = 2

Output:
7
```

### Explanation

Subarrays containing exactly `2` distinct integers:

```text
[1, 2]
[1, 2, 1]
[1, 2, 1, 2]
[2, 1]
[2, 1, 2]
[1, 2]
[2, 3]
```

Total:

```text
7
```

## 💡 Approach

Instead of directly counting **exactly `k`**, calculate:

```text
Exactly K
=
At Most K
-
At Most K - 1
```

For this example:

```text
Exactly 2
=
At Most 2
-
At Most 1
```

### Counting with `At Most K`

```js
function subarraysWithKDistinct(arr, k) {

    function atMostK(k) {
        let left = 0;
        let count = 0;
        const freq = new Map();

        for (let right = 0; right < arr.length; right++) {

            // ADD
            freq.set(arr[right], (freq.get(arr[right]) || 0) + 1);

            // SHRINK
            while (freq.size > k) {
                freq.set(arr[left], freq.get(arr[left]) - 1);

                if (freq.get(arr[left]) === 0) {
                    freq.delete(arr[left]);
                }

                left++;
            }

            // COUNT
            count += right - left + 1;
        }

        return count;
    }

    return atMostK(k) - atMostK(k - 1);
}
```

## ⏱️ Complexity

```text
Time:  O(n)
Space: O(k)
```

### 🧠 Remember

```text
EXACTLY K
    ↓
AT MOST K
    -
AT MOST K - 1
    ↓
ANSWER
```

For counting:

```text
Valid window ending at right
        ↓
right - left + 1
        ↓
Add to count
```
