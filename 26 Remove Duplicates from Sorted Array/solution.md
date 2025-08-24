# 28. Remove Duplicates from Sorted Array

## Intuition

Since the array is sorted, duplicates will be directly adjacent. This means whenever the previous value we have seen is the same as the current value, we should overwrite it. This way we will always keep exactly one of each value.

## Approach

We need to keep track of:

- read index (to traverse the array)
- write index (keep track of where to write)
- the previous element (to compare with current element)

Once we traverse the whole array, we're done. We can not exit early.
As the first element is always a value we haven't seen before and the guard clause ensures it exists (or else we return 0) we can start reading and writing at index 1.

Only if the current element is different than the last seen element, include it and update the write index.

In all cases, update the last seen element and the read index.

Once the whole array is traversed, we ensure each value is unique.

## Complexity

### Time

$$O(n)$$
We need to fully traverse the array to ensure we removed all elements needed.

### Space

$$O(1)$$
As the operation is done in-place, there is only a small constant overhead (the index pointers).

## Code

### Rust

```rust
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
        if nums.len() == 0 {return 0};
        let mut read_idx = 1;
        let mut write_idx = 1;
        let mut last_seen = nums[0];

        while read_idx < nums.len() {
            if nums[read_idx] != last_seen {
                nums[write_idx] = nums[read_idx];
                write_idx += 1;
            }
            last_seen = nums[read_idx];
            read_idx += 1;
        }
        return write_idx as i32;
    }
}
```
