* TODO: additional: #7 #31 #35
* rand function is not a good hash function as the same key will hash to different location
* map insert take O(log n), as it uses bst as the container under the hood
* hash table may not be better than vector when the `key is sequential`
* when inserting the exist key will fail the insertion and keep the corresponding value type unchanged.
* map will sort the container in key's increasing order by default
* we can not determine the sequence of traversal of unordered_map as we don't know the hash function and structure under the hood
* insertion of hash table has to be insert pairs, only insert key type will fail the insertion
*  `disadcantage of quadratic probing`
    1. two elements that hash to the same position will still have the same probe sequence, regardless of how far they land from their hashed location
    2. depending on the size of the hash table involved, it is possible for quadratic probing to never consider specific indices while searching for an open position
    3.the performance of quadratic probing can deteriorate dramatically as the load factor increases
* load factor = #occupied / #buckets (don't count the deleted element)
* separate chaining hash table can have `load factor > 1`
* 