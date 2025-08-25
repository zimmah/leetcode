# 13. Roman Integer

## Intuition

Rust is a bit more tricky when it comes to strings than other languages I am used to.

We can make a Hashmap to map each Roman numeral to the integer value it represents. Then we split up the string into chars, and convert each numeral to a value one by one.

We decide if we should add or subtract it based on the number that's following it. If the number following it is larger, we subtract, otherwise we add.

Then we return the total

## Approach

Improvements after the first pass:
I noticed I had several of the same iterators, so I wanted to clean them up. Then I realized solving it from the back is slightly more efficient because there is no need to look ahead. That way we also don't need to deal with accessing characters by index.

The overall approach is still similar.

## Complexity

### Time

$$O(n)$$
We have to traverse the whole string.

### Space

$$O(1)$$
The space requirements are constant, as they do not depend on input

## Code

### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn roman_to_int(s: String) -> i32 {
        let roman_number = HashMap::from([
            ('I', 1),
            ('V', 5),
            ('X', 10),
            ('L', 50),
            ('C', 100),
            ('D', 500),
            ('M', 1000),
        ]);

        let mut total = 0;
        let mut prev = 0;

        for c in s.chars().rev() {
            let value = roman_number[&c];
            if value < prev {
                total -= value;
            } else {
                total += value;
            }
            prev = value;
        }

        total
    }
}
```
