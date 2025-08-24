# 80. Remove Duplicates from Sorted Array II

## Intuition

Since the array is sorted, we know that if we look back two places and we still encounter the same element, that must mean we're looking at the third duplicate. Thefore we can safely remove it.

## Approach

Keep track of a read index and a write index. Traverse the whole array.

If the element we are currently looking at is not the same as the element two places back, we know it is at least not the third duplicate, so we can safely write it.

Otherwise, we skip it so that it will be overwritten.

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
        // exit early if vector is small
        if nums.len() <= 2 {
            return nums.len() as i32
        }

        let mut read_idx = 2;
        let mut write_idx = 2;
        while read_idx < nums.len() {
            if nums[read_idx] != nums[write_idx-2] {
                nums[write_idx] = nums[read_idx];
                write_idx += 1;
            }
            read_idx += 1;
        }

        return write_idx as i32;
    }
}
```
