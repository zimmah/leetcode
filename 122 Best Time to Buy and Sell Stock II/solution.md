# 122. Best Time to Buy and Sell Stock II

## Intuition

The problem is very similar to problem 121, except instead of only buying and selling once, we can buy and sell more often.

Key insight:
Whenever a sale would be profitable, we should add it to the running total. Even if it would be more profitable to sell later, the rule that we can buy and sell in the same day means we can just buy back the share we sold and still take advantage of the better future price.

Note that it doesn't matter if you buy a share for $1, then sell at $7 ($6 profit) or buy at $1, then sell for $3, then buy again for $3 and then sell for $7. The profit will be the same. This significantly simplifies the problem.

We no longer need to track the lowest number, but rather we just compare to the previous number. Whenever the price goes up, we sell, and if it turns out we sold too early, we simply buy back. This way we ensure the running total remains accurate at all times.

## Approach

If the price is higher than the previous value, we add the difference to the running total.

We always keep track of the previous value.

Return the running total once we traversed the whole array.

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
        let mut previous = 10_001;
        let mut profit = 0;
        for price in prices {
            if price > previous {
                profit += price - previous;
            }
            previous = price;
        }
        profit
    }
}
```
