# 27. Remove Element

## Intuition

We can simply overwrite any element that we don't want in the array and keep track of how many times we have written a value.

Overwriting the same value won't matter, and overwriting a value we don't want to keep effectively erases it. A value we already wrote, will become an unwanted value. Since the reader moves at least as fast as the writer, we will never accidently overwrite a value we need to keep.

## Approach

We need to keep track of 3 values. The reader, the writer, and the amount of times we have written a value.

We continue for as long as there are elements to read.
Whenever the value we encounter is not a value to be removed, we write the current value to the current write index, and then move the write index and update the size.

Regardless of if we write or not, we always move the reader.

Once we have traversed the whole array like this, we will have a new array excluding the element that should be removed.

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
    pub fn remove_element(nums: &mut Vec<i32>, val: i32) -> i32 {
        let mut write_idx = 0;
        let mut read_idx = 0;
        let mut size = 0;

        while read_idx < nums.len() {
            if nums[read_idx] != val {
                nums[write_idx] = nums[read_idx];
                write_idx += 1;
                size += 1;
            }
            read_idx += 1;
        }

        return size;
    }
}
```
