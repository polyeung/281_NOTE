* TODO: additional: #7 #31 #35 `#40` #41 #58 tripple rotation #63 #71 #79 #89 #90
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

* Returns the number of internal node
````C++
else if(!root->left && !root->right){return 0;}
````

* Returns the number of all nodes
````C++
else if(!root->left && !root->right){return 1;}
````
* preorder and post order are not enough to determine the bst
*  balance from the toppest unbalance node????? // TODO

* both prim's and Kruskal's algorithms fail to work on directed graph
*  kruskal's and prim's algorithm both work on graph `with negative edge`
*  For the graph with two or more edges with same length, prim and kruskal mal produce different MST

`Adjacency list`
A -> B -> C means that A has direct connect with B and C, `does not mean that B has direct connect with C`

* elementary sorts are all greedy. Insertion, bubble, selection
*  Kruskal's Prim's Dikstra's Algorithm are all greedy
*  `Unlike backtracking` branch and bound can be used to find an optimal solution to a problem
*  branch and bound does not prunes solutions that do not work(`you will never know whether this solution works or not untill the end`) but prune the less optimal solution

`TSP Heuristics`
1. Heuristics are algorithms that `can not` be used to calculate an optimal solution very quickly
2. Heuristics often have time complexities much faster than that of brute force
3. Heuristics are extermely useful if the standard method for solving a problem takes too long
4. Heuristics can be used to estimate an upper-bound to a TSP problem

* branch and bound is not gruaranteed to be faster compared with the brute force especially when the sample size is small and the promissing function may cause more time.
* Dynamic programming `can not` be used to solve a problem that can also be solved using divide and conquer. because divide and conquer is for no overlapping subproblem but dynamic programing is for subproblems that have overlap.

* Top-down dynamic programming only computes the answers to subproblems that are needed
* Bottom-up dynamic programming can be used any time top=down dynamic programming is used.

* Divide and conquer algorithm divides the problem into subproblems and combines those solutions to find the solution to the original problem. However, dynamic programming does not solve the subproblems independently.

* 1. dikstra's can not work with edges where some of them are negative but some of them are positive
* 2. if all edges are positive, dikstra can find the minimum route
* 3. if all edges are negative, dikstra can find the route whose value is less negative( -1 is less negative than -2)
