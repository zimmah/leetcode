> this is a template

# 380. Insert Delete GetRandom O(1)

## Intuition

To achieve O(1) for all operations, we need two data structures:

- A **hashmap** (`val → index`) for O(1) existence checks and fast lookup of positions.
- A **vector** for O(1) access by index, which enables random selection.

Hashmaps alone can’t support efficient random access (choosing a random element would require iteration), while vectors alone can’t remove values efficiently (removal requires shifting elements). Combining both gives us the best of both worlds.

## Approach

### Insert

Check if the value exists in the hashmap. If not, append it to the end of the vector and record its index in the hashmap. Both operations are O(1).

### Remove

1. Look up the index of the value in the hashmap (O(1)).
2. Get the last element of the vector.
3. Swap the value to remove with the last element in the vector.
4. Pop the last element (removes the target in O(1)).
5. Update the hashmap to reflect the new index of the swapped element.
6. Remove the deleted value from the hashmap.

### Get Random

Pick a random index from `0..len-1` and return the vector element at that index (O(1)).

## Complexity

- **Time:** O(1) average for all operations. Hashmap operations are O(1) amortized but can degrade in worst-case scenarios. With randomized hashing (like Rust’s SipHash), expected runtime is O(1).
- **Space:** O(n), since both the vector and hashmap grow linearly with the number of elements.

## Code

### Rust

```rust
use rand::prelude::*;
use std::collections::HashMap;

struct RandomizedSet {
    data: Vec<i32>,
    index_map: HashMap<i32, usize>,
}

impl RandomizedSet {
    fn new() -> Self {
        RandomizedSet {
            data: Vec::new(),
            index_map: HashMap::new(),
        }
    }

    fn insert(&mut self, val: i32) -> bool {
        if self.index_map.contains_key(&val) {
            return false;
        }
        self.data.push(val);
        self.index_map.insert(val, self.data.len() - 1);
        true
    }

    fn remove(&mut self, val: i32) -> bool {
        if let Some(&idx) = self.index_map.get(&val) {
            let last = *self.data.last().unwrap();
            let last_idx = self.data.len() - 1;
            self.data.swap(idx, last_idx);
            self.data.pop();
            self.index_map.insert(last, idx);
            self.index_map.remove(&val);
            true
        } else {
            false
        }
    }

    fn get_random(&self) -> i32 {
        let mut rng = thread_rng();
        let idx = rng.gen_range(0..self.data.len());
        self.data[idx]
    }
}
```

A more "rustic" way of doing it I found in the solutions of other LeetCoders (same method, just more commonly used syntax)

```rust
use std::collections::HashMap;
use rand::seq::SliceRandom;

struct RandomizedSet {
    hash: HashMap<i32, usize>,
    v: Vec<i32>,
}

impl RandomizedSet {

    fn new() -> Self {
        Self {
            hash: HashMap::new(),
            v: Vec::new(),
        }
    }

    fn insert(&mut self, val: i32) -> bool {
        if self.hash.contains_key(&val) {
            return false;
        }
        self.hash.insert(val, self.v.len());
        self.v.push(val);
        true
    }

    fn remove(&mut self, val: i32) -> bool {
        match self.hash.remove(&val) {
            None => false,
            Some(i) => {
                self.v.swap_remove(i);
                if i < self.v.len() {
                    self.hash.insert(self.v[i], i);
                }
                true
            }
        }
    }

    fn get_random(&self) -> i32 {
        *self.v.choose(&mut rand::thread_rng()).unwrap()
    }
}
```
