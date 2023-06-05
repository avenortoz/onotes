## Intro
A Bloom filter is a probabilistic data structure used to test whether an element is a member of a set. It provides a space-efficient way to represent a large set of items, typically by using a bit array and a set of hash functions. The primary advantage of a Bloom filter is its efficiency in terms of both time and space.

A Bloom filter works as follows:

1.  Initialization: Create a bit array of a fixed size, typically initialized with all zeros.
    
2.  Hash Functions: Choose a set of independent hash functions. These functions take an input (the item to be added or checked) and generate a number within the range of the bit array size.
    
3.  Insertion: To add an element to the Bloom filter, apply each hash function to the element and set the corresponding bits in the bit array to 1.
    
4.  Membership Test: To check if an element is a member of the set, apply each hash function to the element and check if the corresponding bits in the bit array are set to 1. If any of the bits are 0, then the element is definitely not in the set. However, if all the bits are 1, the element is probably in the set. Note that there is a possibility of false positives (indicating that an element is in the set when it is not), but there are no false negatives (if an element is in the set, it will always be identified as such). In the summary, you definitely know when element is not present in the set, but you cannot be sure if the element is in the set
    

Bloom filters are commonly used in applications where approximate membership tests are sufficient, such as [[cach]]ing, spell checkers, network routers, and distributed systems. They offer advantages in terms of memory efficiency and speed at the cost of a controlled probability of false positives. By adjusting the size of the bit array and the number of [[hash _function]]s, the trade-off between false positives and memory usage can be managed.

## Implementation in rust
```
use std::collections::HashSet;
use std::hash::{Hash, Hasher};

struct BloomFilter<T> {
    bit_array: Vec<bool>,
    hash_functions: Vec<fn(&T) -> u64>,
}

impl<T> BloomFilter<T> {
    fn new(size: usize, hash_functions: Vec<fn(&T) -> u64>) -> Self {
        Self {
            bit_array: vec![false; size],
            hash_functions,
        }
    }

    fn insert(&mut self, item: &T) {
        for hash_function in &self.hash_functions {
            let hash = hash_function(item) % (self.bit_array.len() as u64);
            self.bit_array[hash as usize] = true;
        }
    }

    fn contains(&self, item: &T) -> bool {
        for hash_function in &self.hash_functions {
            let hash = hash_function(item) % (self.bit_array.len() as u64);
            if !self.bit_array[hash as usize] {
                return false;
            }
        }
        true
    }
}

fn main() {
    // Define hash functions (using default hasher as an example)
    let hash_functions = vec![|item: &String| {
        let mut hasher = std::collections::hash_map::DefaultHasher::new();
        item.hash(&mut hasher);
        hasher.finish()
    }];

    // Create a new Bloom filter
    let mut bloom_filter = BloomFilter::new(1000, hash_functions);

    // Add some elements
    bloom_filter.insert(&String::from("apple"));
    bloom_filter.insert(&String::from("banana"));
    bloom_filter.insert(&String::from("cherry"));

    // Check if an element is in the Bloom filter
    println!("Is 'apple' probably in the Bloom filter? {}", bloom_filter.contains(&String::from("apple")));
    println!("Is 'orange' probably in the Bloom filter? {}", bloom_filter.contains(&String::from("orange")));
}

```