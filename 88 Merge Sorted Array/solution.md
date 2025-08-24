# 88. Merge Sorted Array

## Intuition

The arrays are sorted in non-decreasing order, meaning we merely need to compare the value at each step, and 'zip' the values like a zipper.

Since we combine into the original array, starting from the front means we might often need to shift numbers around, causing inefficiencies. Starting from the back eliminates this issue.

## Approach

Create an index at the end of each of the two arrays, as well as a write index at the end of the first array. Since m or n could be 0, we need to store them as isize or we get an underflow.  
Alternatively we could put in a guard close to handle cases where m or n are 0, but I thought this looked cleaner.

Whenever we have fully traversed the second array, we're done sorting so we can halt at that point. Trivially an empty second array means we're done before we even start.

When the write_index starts to go negative we know we are done, as 0 is the last legal position to write (we start from the back).

If we fully traversed the first array it means we can just short-circuit the if condition and immediately write the next value of the second array until we also fully traversed the second array.

If both arrays still have elements, we write the larger of the elements and adjust the correct index (the write index always moves, since we always write a value). In case of a tie, prefer the second array, because the sooner the second array is traversed, the sooner we can halt the function.

## Complexity

### Time

$$O(m+n)$$
We need to fully traverse both arrays to ensure all the elements are in place.

### Space

$$O(1)$$
As the operation is done in-place, there is only a small constant overhead (the index pointers).

## Code

### Rust

```rust
impl Solution {
    pub fn merge(nums1: &mut Vec<i32>, m: i32, nums2: &mut Vec<i32>, n: i32) {
        let mut index1 = (m - 1) as isize;
        let mut index2 = (n - 1) as isize;
        let mut write_index = (m + n - 1) as isize;

        if index2 < 0 { return; }

        while write_index >= 0  {
            if index1 >= 0 && nums1[index1 as usize] > nums2[index2 as usize] {
                nums1[write_index as usize] = nums1[index1 as usize];
                index1 -= 1;
            } else {
                nums1[write_index as usize] = nums2[index2 as usize];
                index2 -= 1;
                if index2 < 0 { break; }
            }
            write_index -= 1;
        }
    }
}
```
