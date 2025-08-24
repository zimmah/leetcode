# 1431. Kids with the Greatest Number of Candies

## Intuition

Get the max number of candies found in the array first, then simply go through the array and compare each value to the max.

If it's equal or larger, assign true to that index, otherwise keep the default false.

## Approach

See intuition.

## Complexity

### Time

$$O(n^2)$$
We need to traverse the array twice, first to find the max, then once again to assign true/false.

### Space

$$O(n)$$
We need to build a result array of equal length as the input array.  
The only other option would be to overwrite the input array.

## Code

### Rust

```rust
impl Solution {
    pub fn kids_with_candies(candies: Vec<i32>, extra_candies: i32) -> Vec<bool> {
        let max = candies.iter().copied().max().unwrap_or(0);
        let mut result = vec![false; candies.len()];
        for (index, amount) in candies.iter().enumerate() {
            if amount + extra_candies >= max {
                result[index] = true;
            }
        }
        result
    }
}
```
