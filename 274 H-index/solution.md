# 274. H-index

## Intuition

By the definition:
The `h-index` is defined as the **maximum value** of `h` such that the given researcher has published at least `h` papers that have each been cited at least `h` times.

If we sort the vector first in descending order we can simply increment the h value if the current value is bigger than our h value. We know that any previous works have been cited as least as often because we sorted in descending order.

## Approach

Sort in descending order.
For each element in the vector if the element has more citations than the h value we can increment the h value.
Once this stops being true we're done.

## Complexity

### Time

$$O(n \log n)$$
Sorting takes n log n.

### Space

$$O(1)$$
Operation done in place, constant overhead.

## Code

### Rust

```rust
impl Solution {
    pub fn h_index(mut citations: Vec<i32>) -> i32 {
        let mut h = 0;
        citations.sort_by(|a, b| b.cmp(a));
        for citation_amount in citations {
            if (citation_amount > h) {
                h += 1;
            } else {
                return h;
            }
        }
        h
    }
}
```

Alternative solution without sorting

This method relies on buckets, complexity is O(n) (both space and time).

The buckets act similar to a hashmap where the index is the amount of citations a paper has, and the value is the amount of papers with that amount of citations.
Note: Since the h-index can never be higher than the length of the input array, any papers with at least as many citations as the input array will be counted as having exactly that many citations.

Once we populated the buckets, we walk though them in reverse order. Keeping track of a running total.
Once the total is as least as large as the index (remember the index counts backwards starting from `n`, as the H-index can never be more than `n`) we will return the index.

The h-index is the largest `h` where at least `h` papers have at least `h` citations. By starting from `n` (the maximum possible h-index) and moving downward, the first `i` where `total >= i` is the largest valid h-index.

```rust
impl Solution {
    pub fn h_index(citations: Vec<i32>) -> i32 {
        let n = citations.len();
        let mut buckets = vec![0; n + 1];

        for &c in &citations {
            if (c as usize) >= n {
                buckets[n] += 1;
            } else {
                buckets[c as usize] += 1;
            }
        }

        let mut total = 0;
        for i in (0..=n).rev() {
            total += buckets[i];
            if total >= i {
                return i as i32;
            }
        }
        0
    }
}
```
