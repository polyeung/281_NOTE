# EECS281 note
## Hash Table
#### Separate Chaining
store colliding key-value pairs in a **linked list for that bucket**
#### Open Addressing
store colliding key-value pairs in **another bucket/location**

#### Linear Probing
if bucket not available, try buckets after that linearly. 
* `(H(k) + i) % N`

#### Hash function
hash function can match the key to certain position, may cause collision, best hash function is hard to write. 


Here is an example
````C++
int hash(string s){
if(s.empty())
  return 0
else
  return s[0] - 'a';

}

````

#### Quadratic probing
Interval between probes is increased by adding a quadratic polynomial until open spot is found
* `(H(k) + i^2) % N`

#### Double Hashing
Interval probes is computed using another hash function
Try buckets H(k), H(k) + F(k), H(k) + 2F(k)....

* `(H(k) + i*F(k)) % N`

H(k) is the first hash function, F(K) is the second hash function
Linear | Quadratic | Double Hashing
--- | --- | ---
`(H(k) + i) % N`| `(H(k) + i^2) % N` | `(H(k) + i*F(k)) % N`

#### Hash function deletion
// TODO
* if hash_key spot is empty
* if hash_key spot is occupied, just make it the type deleted
* if hash_key spot is deleted

#### Hash Table Performance
`load factor = # elements/# buckets`
* use prime numbers for table sizes, important for modulus and reduuce collision
* keep load factor low: `< 0.75`

####Time Complexities
k = number of keys, n = table size

Search | Inseart | Delete
--- | --- | ---
O(1 + k/n) | O(1 + k/n) | O(1 + k/n) 

`Space Complexity: O(n)`

Fundamentally, `still O(1)` assuming a reasonable load factor (a <= ~0.5)

#### Comparison
Separate Chaining | Open Addressing
--- | ---
faster handling collisions | requires deleted elements for erase
can have load factor > 1 | Cache locality improves runtime
`Fast insert and delete in middle of list ` | `can random access`





