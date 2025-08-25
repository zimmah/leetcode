# 58. Length of Last Word

## Intuition

Splitting the string will result in a vector with words. However, if there's multiple spaces, we could end up with a space as the last word. Therefore we should trim first, then split, and then we can safely return the length of the last word in the vector.

## Approach

See intuition.

- Sanitize (trim)
- Split
- Return the length of the last item in the vector (= the last word)

## Complexity

### Time

$$O(n)$$
The time scales linearly with the input as the split function traverses the input array.

### Space

$$O(k)$$
Where `k` is the number of words in the string.

## Code

### Rust

```rust
impl Solution {
    pub fn length_of_last_word(s: String) -> i32 {
        let words: Vec<&str> = s.trim().split(" ").collect();
        return words[words.len() - 1].len() as i32;
    }
}
```
