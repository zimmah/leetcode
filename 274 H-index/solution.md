# 274. H-index

## Intuition

By the definition:
The `h-index` is defined as the **maximum value** of `h` such that the given researcher has published at least `h` papers that have each been cited at least `h` times.

Each time we encounter a value, we add it to the running total if the amount of citations it has is strictly higher than the running total.

## Approach

## Complexity

### Time

$$O(n)$$
Reason

### Space

$$O(1)$$
Reason

## Code

### Rust

```rust
impl Solution {
    pub fn h_index(citations: Vec<i32>) -> i32 {

    }
}
```
