# 121. Best Time to Buy and Sell Stock

## Intuition

We can simply check if the difference between the current value and the lowest value we have seen so far is more profitable than the highest profit we have seen so far.

Key insight for branching logic, we always either:

- update the lowest value seen
- check the profit

As whenever a value is the lowest we have seen so far, it can never be more profitable to sell it at that point and vice versa. These are mutually exclusive.

## Approach

For each price in the array we check if it's lower than what we have seen before.

If lower: we store it as the lowest price and move one
If not lower: we check if selling now would be more profitable than the max profit we have seen so far.

After traversing the whole array, return the max profit

Example:
`[7,1,5,3,6,4]`

### Step 1, index 0:

Lowest seen so far: 10_001  
7 is lower than 10_001 so we store it.
We don't need to bother checking the profit because 7 is the lowest value we have seen so there is no way selling it would be profitable.

### Step 2, index 1:

Lowest seen so far: 7  
1 is lower than 7 so we store it.  
Skip ahead, selling not needed. (see step 1)

### Step 3, index 2:

Lowest seen so far: 1  
5-1 = 4  
4 > 0 ? true  
potential profit updated to 4.

### Step 4, index 3:

Lowest seen so far: 1  
3-1 = 2  
2 > 4 ? false  
skip

### Step 5, index 4:

Lowest seen so far: 1.  
6-1 = 5.  
5 > 4 ? true  
store 5 as max profit

### Step 6, index 5:

Lowest seen so far: 1  
4-1 = 3  
3 > 5 ? false

### Done

return 5 because it's the largest profit we have seen.

## Complexity

### Time

$$O(n)$$
We need to fully traverse the arrays to ensure we get the best possible deal.

### Space

$$O(1)$$
The space complexity does not depend on the input

## Code

### Rust

```rust
impl Solution {
    pub fn max_profit(prices: Vec<i32>) -> i32 {
        let mut lowest = 10_001; // out of bounds on purpose, so any new value will be lower
        let mut profit = 0;
        for price in prices {
            if price < lowest {
                lowest = price;
            } else if profit < price - lowest {
                profit = price - lowest;
            }
        }
        profit
    }
}
```
