# 55. Jump Game

## Intuition

You can't simply always jump the max distance as you might get stuck if you jump over another space that could allow you to jump further.

It would be possible to solve this problem recursively, but I opted not to do that this time.

The easiest approach is to continue traversing the array, and update the maximum known distance you can traverse as you go. If you ever arrive at the maximum known distance and can't move further, you know you are stuck. Otherwise, simply continue until you either get stuck or reach the end.

## Approach

For each element in the array calculate how far it can bring you.
If it can bring you further than the furthest known distance, update the max.

If it allows you to jump to the end or beyond, exit early.
If you are currently at the max reach at this point, you're stuck.

If you traverse the whole array without getting stuck, you made it to the end --> true.

## Complexity

### Time

$$O(n)$$
Worst case we need to traverse the whole array.

### Space

$$O(1)$$
The space complexity does not depend on the input.

## Code

### Rust

```rust
impl Solution {
    pub fn can_jump(nums: Vec<i32>) -> bool {
        let mut index = 0;
        let mut max_reach = 0;
        while index <= max_reach {
            let jump = nums[index] as usize;
            max_reach = max_reach.max(jump + index);
            if max_reach >= nums.len() - 1 {
                return true;
            }
            if index == max_reach {
                return false;
            }
            index += 1;
        }
        return true;
    }
}
```
