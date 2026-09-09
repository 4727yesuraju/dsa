# Frequency Map Sliding Window

## 🧠 Pattern

Use when you need to **track the frequency/count of elements or characters inside the window**.

> **ADD → UPDATE FREQUENCY → CHECK → SHRINK → UPDATE**

## 💻 Template

```js
    function frequencyMapWindow(arr) {
        let left = 0;
        let answer = 0;
        const freq = new Map();

        for (let right = 0; right < arr.length; right++) {

            // ADD
            freq.set(arr[right], (freq.get(arr[right]) || 0) + 1);

            // CHECK → SHRINK
            while (windowIsInvalid()) {

                // REMOVE
                freq.set(arr[left], freq.get(arr[left]) - 1);

                if (freq.get(arr[left]) === 0) {
                    freq.delete(arr[left]);
                }

                left++;
            }

            // UPDATE
            answer = Math.max(answer, right - left + 1);
        }

        return answer;
    }
```

# Frequency Map Sliding Window — Example Problem

## 🧩 Problem

Given a string `s` and an integer `k`, find the **length of the longest substring containing at most `k` distinct characters**.

### Example

```text
Input:
s = "eceba"
k = 2

Output:
3
```

### Explanation

```text
right = 0 → "e"
           distinct = 1 ✅
           length = 1

right = 1 → "ec"
           distinct = 2 ✅
           length = 2

right = 2 → "ece"
           distinct = 2 ✅
           length = 3  ← maximum

right = 3 → "eceb"
           distinct = 3 ❌
           ↓ shrink

           remove 'e' → "ceb"
           distinct = 3 ❌
           ↓ shrink

           remove 'c' → "eb"
           distinct = 2 ✅
           length = 2

right = 4 → "eba"
           distinct = 3 ❌
           ↓ shrink

           remove 'e' → "ba"
           distinct = 2 ✅
           length = 2
```

The longest valid substring is:

```text
"ece"
```

Answer:

```text
3
```

## 💡 Approach

> **ADD → UPDATE FREQUENCY → CHECK → SHRINK → UPDATE**

```js
function longestAtMostKDistinct(s, k) {
    let left = 0;
    let answer = 0;
    const freq = new Map();

    for (let right = 0; right < s.length; right++) {

        // ADD + UPDATE FREQUENCY
        freq.set(s[right], (freq.get(s[right]) || 0) + 1);

        // CHECK → SHRINK
        while (freq.size > k) {
            freq.set(s[left], freq.get(s[left]) - 1);

            if (freq.get(s[left]) === 0) {
                freq.delete(s[left]);
            }

            left++;
        }

        // UPDATE
        answer = Math.max(answer, right - left + 1);
    }

    return answer;
}
```

## ⏱️ Complexity

```text
Time:  O(n)
Space: O(k)
```

### 🧠 Remember

```text
EXPAND
   ↓
Add to Frequency Map
   ↓
Distinct > K?
   ↓
YES → SHRINK
   ↓
Valid Window
   ↓
UPDATE longest
```
