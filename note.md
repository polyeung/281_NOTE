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


### unordered_map vs. ordered_map
---|Unordered_map | ordered_map
--- | --- | ---
container | hash table(vector with struct) | `AvL Tree`
search time | O(log(n)) | Average O(1), worst : O(n)
Insertion time  | log(n) + Rebalance  | Average O(1), worst : O(n)          
Deletion time   | log(n) + Rebalance  | Average O(1), worst : O(n)




### Tree Fundamentals

Simple Tree | Rooted Tree
--- | ---
Acyclic connected graph, considered undirected | a simple tree with a selected node(root), all edges are directed away from root

Tree Terminology | `Ancestor ` | ` Descendent`
--- | --- | ---
---| parent of a parent | child of a child (further from root) 

`Height`
````C++
height(empty) = 0;
height(node) = max(height(node -> left), height(node-> right)) + 1;
````

`Size`
````C++
size(empty) = 0
size(ndoe) = size(left_child) + size(right_child) + 1;
````

`Depth`
````C++
depth(empty) = 0
depth(node) = depth(parent) + 1; //
````

`Tree Definition`
Complete Binary tree| Binary tree
--- | ---
the levels are completely filled except possibly the lowest one, which is filled from the left. | every node can have at most two childs

`Binary tree implementation with array`
* space (best case) : O(n) , worst case : O(2^n)

`Complexity for pointer-base binary tree`
Insert Key (best)| O(1)| ---
--- | --- | ---
Insert Key(worst)| O(n)| (a stick)
space:(best and worse are same) | O(n) | ---

`level ordering`
````C++
//sudo code. BFS
woid levelorder(Node *p){
  if(!p) {return;}
    queue <Node *> q;
    q.push(p);
    while(!q.empty()){
    1.take top node
    2.pop top node
    3.read node value
    4.if exsit, push left child
    5. push right child
  }
}
````
* we can use inorder traversal to sort the binary search tree

* How many worst-case trees are possible for n unique values: stick tree, worst case: 2^(n-1)
* For left n-1 nodes, we can put either left or right, two choices to form a stick

`Binary Search tree complexity`
* Insert: Best: O(logn), worst: O(n) stick

`Binary search tree remove node`
1. if this node has one child or no child, trivial
2. if this ndoe has two childs, swap with the inorder successor(the smalles thing on its right subtree) `Go right once and continue left, until get leaf`

`Tree Removal`
````C++
void remove(Node* &tree,const int &val){
  Node *nodeToDelete = tree;
  Node *inorderSuccessor;

  //recursively find the node to delete
  if(tree == nullptr)
   return;
  else if(val < tree->value)
    remove(tree->left, val);
  else if(val > tree->value)
    remove(tree->right,val);
  else{
    //check the situation of that node
    if(tree->left == nullptr){
    // TODO
      tree = tree->right;
      delete nodeToDelete;
    }else if(tree->riht == nullptr){
      tree = tree->left;
      delete nodeTODelete;
    }else{
    inorderSuccessor = tree->right;
      while(inorderSuccessor -> left != nullptr){
        inorderSuccessor = inorderSuccessor->left;
        
        //replace value with the inorder successor's value
        noteToDelete->value = inorderSuccessor->value;
        
        //remove the inorder sucessor from right subtree
        `remove(tree->right,inorderSuccessor->value);`
      }
    }
  }
}
````

#### AVL Tree

* worst case and best case insertion are now both O(log n)
* Hight Balance Property: For every internal node, the heights of the children of v differ by at most 1.

`Rotation`
* Rotation is a local change involving only three pointers and two nodes

<img src="Image/tree_rotate.png" width="600">

`Insert / Four cases`


1. Single left rotation: RL(a) , Rotate Left
2. Single right rotation: RR(a)

<img src="Image/single_rotate.png" width="600">

4. Double rotation right-left: RR(c), RL(a)

<img src="Image/double_rotate1.png" width="600">

6. Double rotation left-right RL(a), RR(c)

<img src="Image/double_rotate2.png" width="600">



<img src="1280px-Tree_Rebalancing.gif" width="600">

`Checking and Balancing`
````C++
Algorighm checkAndBalance(Node* n){
  if(balance(n) > 1)
    if(balance(n->left) < 0)
      rotateL(n->left)
  rotateR(n)
  
  else if(balance(n) < -1)
    if(balance(n->right) > 0)
        rotateR(n->right)
     rotateL(n)

}
````

## GRAPH

* MST is lowest-cost sub-graph that: 1. includes all nodes in a graph 2. Keeps all nodes connected
* Two algorithms to find MST:
  `Prim`: iteratively adds closest node to current tree - very similar to Dijkstra, O(V^2) or O(ElogV)
  `Kruskal`: iteratively builds forest by adding minimal edges, O(ElogV)

* For dense G, use the nested-loop Prim variant
* For sparse G, Kruskal is faster
  Kruskal relies on the efficiency of sorting algorithms
  also relies on the efficiency of union-find.


 ## Alogorithm
 
 #### Brute Force
 1. solve problem in simplest way
 2. Generate entire solution set, pick best
 3. will give `optimal solution` with poor efficiency

#### Greedy
1. make local, best decision, and don't look back(once decided, can not regret)
2. `may` give optimal solution with 'better' efficiency
3. may not produce the global-optimal solution

#### Diveide and Conquer Algorithms
Definition : Divide a problem solution into two or more smaller problems, preferably of equal size
1. often recursive
2. often involve log n complexity

Eg.: Binary search of sorted list
     quicksort

pros : 
1. efficiency
2. 'Elegance ' of recursion

cons : 
1. Recursive calls to small subdomains often expensive
2. Some times dependent upon initial state of subdomains (eg. binary search requires sorted array)

#### Combind and Conquer
Definition : start with smallest subdomain possible. Then combine increasingly larger subdomains until size = n.
eg.; merge sort

Divide and Conquer: Top down 
Combine and conquer: Bottom up


## Dynamic Programming
1.similar to divide and conquer, but used for `overlapping` subspaes.
2. used when partial solutions are needed later
3. often times looking "nearby" for previously calculated values

## Backtracking Algorithm
1. constraint satisfaction problems
2. allows pruning of branches that are not promising
3. most difficult part is determining nature of promising

General From:
````C++
Algorithm checknode(node v){
  if(promising(v))
    if(solution(v))
        write solution
     else
        for each node u adjacent to v
          checknode(`u`)


}
````
`Note`
solution(v) : check 'depth' of solution(constraint satisfaction)
promising(v): pruning , whether it is promissing to be a solution
checknode(v): called only if partial solution is both promising and not a solution (on the way of checking whether it is solution, should be promissing )



## Branch and Bound Algorithm
1. for optimization problems, must satisfy all constraints
2. must minimize an objective function subject to those constraints
3. the idea of backtracking `extended` to optimization problems
4. A partial solution is pruned if its cur_cost >= best_cost, if the cost of a partial solution is too big, then `drop this partial solution`
  eg. the length of a path or tour

General From
````C++
Algorithm checknode(Node v,Best currBest){
  Node u
  if(promissing(v,curBest))
    if(solution(v)) then
        update(currBest)
     else
        for each child u of v
           checknode(u,currBest)
           
  return currBest
}
````










##### Algorithm Family
Algorithm | Note
--- | ---
Brute Force|Guarantees optimality. `But it is slow`
Greedy | pick the next best option- `not gurantess to be optimal`
Divide and Conquer | Combining solutions to subproblems( e.g mergesort)
Backtracking | constraint satisfaction problems. `satisfying constraints is not necessarily optimal`
Branch and Boung | Optimization problems
Dynamic Programming | Save answers to overlapping subproblems to optimize time complexity (optimize brute force)





## String and Sequence
* .size() for c string is O(n), 
*  there are two pointers at the begin and end of c++ string, so .size() is O(1), `.end() - .begin()`

#### std::equal
* ra and rad are equal

````C++
bool isPalindrome(string s){
 return std::equal(begin(s),begin(s)+s.size()/2,rbegin(s));
}
````
* after C++ 11, string object has ptr points to the beginning of string, and a size variable
* locality is better as ptr, size, string are all contigous in memory

#### String compare O(n), n is the length of shortest string
* if str1.len() < str2.len() -> return -1, else return 1
*  compare common area, if char1 < char2 - > return -1, vice versa
*  same -> return 0;

`constexpr`
* constant at complie time 
*
## Review the fingerprints 




