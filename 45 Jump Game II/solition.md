# 45. Jump Game II

## Intuition

This looks like a BFS problem.

As you traverse the array, for each position you check how far that jump can bring you, and which options that brings.

You could represent this as a graph, but it's not actually neccesary to create the graph as we can similate the behavior without actually generating it, saving space.

example:

`[2,3,1,1,4]`  
The first index allows us to jump `2` spaces, therefore giving us `2` options.  
These options can be represented as `[3,1]`, although we also need to take into account the index. As the `3` is already in index `1` and the `1` is in index `2`. The actual max index that can be reached would become `4` and `2` respectively.

Let's now consider the first option from the `[3,1]` slice. A jump to index `4` (remember index `1` + jump distance `3` = `4`) would take us to the last index of the array, so we have arrived.

Would the array be longer, we would need to continue this logic.

Of course, as said before, we don't need to actually simulate the entire graph, we can simply only update the max jump distance and traverse the array until we get to the end. We know this will be the shortest route because we are going breadth-first. Meaning if there would have been a shorter route, we would have considered it earlier.

## Approach

We need to keep track of the current index because it matters for the relative jump distance (we jump from our current position after all).

For each location we update the max reach.
Whenever we reach our current limit (the distance we can traverse with our current jump) without hitting the end of the array, we know we need to jump at least once more.  
We will jump, and update the counter and our max_reach so far will be our new current reach.  
We will check until we exchaust all options (breadth first search).
This will continue until a solution is found.

This will guarantee our solution is indeed the least amount of jumps.

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
    pub fn jump(nums: Vec<i32>) -> i32 {
        let mut jumps = 0;
        let mut current_reach = 0;
        let mut max_reach = 0;
        for i in 0..(nums.len() - 1) {
            max_reach = max_reach.max(i + nums[i] as usize);
            if i == current_reach {
                jumps += 1;
                current_reach = max_reach;
            }
        }
        jumps
    }
}
```
