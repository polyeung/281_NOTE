## 1029 two city scheduling
greedy method: sort the vector based on the difference of Cost_cityB - cost_cityA

smaller number should send to cityB as their city B cost are smaller than city A cost compared with others
remember that half of the people should go to city B while the other half to city B

````C++
int twoCitySchedCost(vector<vector<int>>& costs) {
        sort(costs.begin(),costs.end(),[](vector<int> &v1,vector<int> &v2){
        return v1.back() - v1.front() < v2.back() - v2.front();
    });
    
    int total_cost = 0;
    for(size_t i = 0;i < costs.size();i++){
        //element in the front should send to city B ,as they have relatively
        //small cost_B - cost_A,
        if(i < costs.size() / 2)
            total_cost += costs[i].back();
        else
            total_cost += costs[i].front();
    }
    
    return total_cost;
    }
````

## count number of rectangle, eecs281 question

<img width="800" alt="image" src="https://user-images.githubusercontent.com/81163933/174462110-b9639612-abba-4bf0-a411-1199a8102adb.png">

IDEA: using hash map, counting how many vertical edges you can have. for example (1,1) and (1,2) form one vertical edges, we can stores pair( 1, 2), into the hash map, using this key, we can know how many vertical edges with same length we have and thus calculate how many rectangles we can have

````C++
int count_rectangles(vector<Point> &points) {
map<pair<int, int>, int> lines;
int count = 0;
for (auto &p1 : points) {
        for (auto &p2 : points) {
            if (p1.x == p2.x && p1.y < p2.y) {
                pair<int, int> vertical_line{ p1.y, p2.y };
                count += lines[vertical_line]++;
             }
        }
}
return count;
}
````

## Leetcode 752. open the lock

we have a helper function that can return all sequences that can be attained in a single turn from the original sequence orig_seq.

`Idea:`
        
1.try different combination using DFS, we start at "0000"
2. using unordered_set<string> dead_end and visited to check whether we reach the dead end and mark sequence as visited.

`Mistakes I made`
1. we can not check the dead end in the innest for loop and return -1. if we do so, we have not tried all possiblities in that layer, simply             drop that solution and don't push to the queue if it has been visited or it is dead end
 2. we need cur_layer_size variable to make sure we look into every sequences within same layer in one pass of while loop
 
````C++
        vector<string> single_turn_seqs(string orig_seq) {
         vector<string> result;
        for (int i = 0; i < 4; ++i) {
             string temp = orig_seq;
             temp[i] = (orig_seq[i] - '0' + 1) % 10 + '0';
            result.push_back(temp);
            temp[i] = (orig_seq[i] - '0' - 1 + 10) % 10 + '0';
            result.push_back(temp);
    }
    return result;
        }
int openLock(vector<string>& deadends, string target) {
    unordered_set<string> dead(deadends.begin(),deadends.end());
    unordered_set<string> visited;
    queue<string> my_q; // dfs
    int result = 0;
    my_q.push("0000");
    
    if(dead.find("0000") != dead.end()){return -1;}
    
    while(!my_q.empty()){
        size_t cur_layer_size = my_q.size();
        
        for(size_t i = 0;i < cur_layer_size;i++){
            string cur_s = my_q.front();
            my_q.pop();
            //target?
            if(cur_s == target){return result;}

            auto possible = single_turn_seqs(cur_s);
            
            //for all possible sequences, check visited? then check deadend? if not result++
            for(auto &i: possible){
                
                //visited?
                if(visited.find(i) == visited.end()
                   && dead.find(i) == dead.end()){
                    visited.insert(i); //mark as visited
                    my_q.push(i);
                }
                        
            }//for
        }//current layer for

        result++;
        
    }//while
    
    return -1;
    
}
````
               
## Leetcode 743 Network Delay time
                                                 
* Idea : using dijstra's algorithm
* using min-heap , priority queue, need dist vector to record all distance to that nodes, need hash map to search for certain edges

````C++
                
  //   total length | node
//
int networkDelayTime(vector<vector<int>>& times, int n, int k) {
    priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>> my_q;
    unordered_map<int, vector<pair<int,int>>> my_map;
    vector<int> dist(n + 1, std::numeric_limits<int>::max());
    
    for(auto & i: times){
        my_map[i[0]].push_back(make_pair(i[1],i[2]));
    }
    
    dist[k] = 0;
    my_q.push(make_pair(0,k));//start from number k
    
    while(!my_q.empty()){
        int cur_node = my_q.top().second;
        my_q.pop();
        
        //find for this cur_node, find and update all reachable nodes
        
        for(auto it = my_map[cur_node].begin(); it != my_map[cur_node].end();++it){
            //new_length to add = it->second
            //new_node  = it->first
            
            int new_node = it->first; int new_length_to_add = it->second;
            
            if(dist[cur_node] + new_length_to_add < dist[new_node]){
                dist[new_node] = dist[cur_node] + new_length_to_add;
                my_q.push(make_pair(dist[new_node],new_node));
            }
            
        }
    }
    
    //find max distance
    int max = numeric_limits<int>::min();
    
    for(size_t i = 1;i < dist.size();i++){if(dist[i] > max){max = dist[i];}}
    
    if(max == numeric_limits<int>::max())
        return -1;
    else
        return max;
  
        
}              
````

## Zero Sum Game 0. 281  problem
![IMG_1D14B45E67DD-1](https://user-images.githubusercontent.com/81163933/174652052-b9ae94a3-5907-4cd4-a87f-6751c00190de.jpeg)

* Idea: using hase_set to record the trends of the cumulative sum of sequence, if there are two points a and b that cumulative sum are the same, this means that the sequence between point a and point b must be zero
````C++
                bool zero_contiguous_sum(vector <int> &nums){
                        if(nums.empty()){return false;}
                        
                        unordered_set <int> my_set;
                        int cur_sum = 0;
                        my_set.insert(cur_sum);
                        for(auto &i : nums){
                                cur_sum += i;
                                if(my_set.find(cur_sum) != my_set.end()){return true; //same cumulative sum point found}
                                else{my_set.insert(cur_sum);}
                        }
                return false;
                
                }
````
                
                
## 128. Generate Parentheses
                
* Method I: back tracking.
                
* Mistakes I have: when doing recursive function you can do func(a + 1) but not func(++a). You are not really increasing this element, but rather make    it seems like increasing from the perspective of next recursive function.(pass the increased value in but not modify value from current function)
                
````C++
void back_track(vector<string> &result,string cur, int num_L,int num_R, int size){
        //base case
        if(cur.size() == size*2){
            result.push_back(cur);
            return;
        }
        
        //decision  I have
        if(num_L < size){
            //add a Left parentheses
            back_track(result,cur+'(',num_L+1,num_R,size);
        }
        
        if(num_L > num_R){
            //add a right parentheses
 
            back_track(result,cur+')',num_L,num_R + 1,size);
        }
    
    }
    
    
    vector<string> generateParenthesis(int n) {
        vector<string> my_vec;
        back_track(my_vec,"",0,0,n);
        return my_vec;
    }                
````
                
##  Programming : Range Queries (281)
![IMG_E5B9C8E74909-1](https://user-images.githubusercontent.com/81163933/174868036-2dbb4241-3232-4900-9c09-d1893ece0046.jpeg)
* Idea : using hashmap to record how many times each number appear at each position of data vector, this can reduce complexity 
````C++
                void my_range_queries(const vector<unsigned int> &data,
                      const vector<Query> &queries){
    //creating hash map need O(N^2) time
    
    //step 1, create hash_map: preprocessing
    unordered_map <int,vector<int>> my_map;
    for(size_t i = 0;i < data.size();i++){
        //current we deal with data[i]
        if(my_map.find(data[i]) != my_map.end()){continue;}
        auto &vec = my_map[data[i]];
        
        vec.resize(data.size(),2);
    
        int count = 0;
        for(int j = 0;j< data.size();j++){
            if(data[j] == data[i]){count++;}
            vec[j] = count;
        }
    }
    
    //step 2 query
    for(auto& s: queries){
        if(my_map.find(s.id) == my_map.end())
            cout << "0 ";
        else
            if(s.start == 0){
                cout << my_map[s.id][s.end] << " ";}
            else{
            cout << my_map[s.id][s.end] - my_map[s.id][s.start] << " ";
            }
    }
    
    
    
}
````
## leetcode 1647 Minimum Deletions to make character counts unique
* Idea : using hash_set to record whether there are same frequency before and if yes, we can delete the frequency and increment the num_deletion untill it becomes unique -> if frequency = 0, don't insert to hash_set , if frequency > 0, insert to hash_set

````C++
                int minDeletions(string s) {
    vector<int> vec(26,0);
    unordered_set<int> my_set;
    for(auto &c : s){
        ++vec[(c - 'a')];
    }
    
    
    int num_deletion = 0 ;
    
    for(auto & c :vec){
        if(c == 0){continue;}
        
        int num = c;
        
        if(my_set.find(num) == my_set.end()){
            my_set.insert(num);
        }else{
            //repeated , delete until it is unique
            while(my_set.find(num) != my_set.end()){
                num--;
                num_deletion++;
                if(num == 0){break;}
            }
            if(num != 0)
                my_set.insert(num);
        }
    }
    
    return num_deletion;
}
````
## Leetcode 199 , Binary Tree Right Side View
 
 `Idea`: * using level order traversal from right to left for each level, we always push the right node first into the queue, so that for each level, the first node out from the queue is the right-most node
  
 ````C++
        queue<TreeNode*> my_q;
    vector<int> ret;
    if(root == nullptr){return vector<int>{};}
    
    my_q.push(root);
    
    while(!my_q.empty()){
        size_t size = my_q.size();
        
        
        for(int i = 0; i < size;i++){
            //for each level the first one is right-most
            //as we always push the right-most one first
            auto this_node = my_q.front();
            my_q.pop();
            if(i == 0){ret.push_back(this_node->val);}
            
            if(this_node->right){my_q.push(this_node->right);}
            if(this_node->left){my_q.push(this_node->left);}
        }
        
        
    }
    return ret;
 ````
        
 ## Leetcode 663 Equal Tree Partition
 `Idea: `: using get total sum function for each node, and check whether there is a subtree's sum equals to half of the total sum.
        if total sum is odd, it is not possible to find the solution, thus we return false;
 ````C++
         void get_sum(TreeNode *start_node, int & sum){
        if(!start_node){return;}
        sum+=start_node->val;
        get_sum(start_node->left,sum);
        get_sum(start_node->right,sum);
    }
    
    bool bfs(TreeNode *root, int &target){
        
        int sum1 = 0;
        if(!root){return false;}
        if(!root->right && !root->left){return false;}
        
        if(root->left){
            sum1 = 0;
            get_sum(root->left,sum1);
            if(sum1 == target){return true;}
        }
        if(root->right){
            sum1 = 0;
            get_sum(root->right,sum1);
            if(sum1 == target){return true;}
        }
        
        return bfs(root->left,target) || bfs(root->right,target);
    }
    
    bool checkEqualTree(TreeNode* root) {
        int sum = 0;
        get_sum(root,sum);
        if(sum %2 != 0){return false;}
        
        int target = sum/2;
        
        return bfs(root,target);
    }
 ````

## Leetcode 699 Trim Binary Search Tree
`IDEA: ` 
1. if the node is smaller than the low value, directly drop leftTree of this node(must be smaller), run recersive on right sub tree
2. if the node is larger than the high value, directly drop rightTree of this node(must be larger), run recersive on left sub tree
3. if this node is valid, left pointer points to the result of recursion on left subtree, vice versa
4. intuitively, this function check from the bottom of tree to top, and drop invalid subtree while throwing up the valid trees
````C++
TreeNode* trimBST(TreeNode* root, int low, int high) {
        if(!root){return nullptr;}
        
        if(root->val < low){
            //we can drop all the left subtree and continue run on right subtree
            return trimBST(root->right,low,high);
        }
        
        if(root->val > high){
            return trimBST(root->left,low,high);
        }
        
        root->left = trimBST(root->left,low,high);
        root->right = trimBST(root->right,low,high);
        
        return root;
    }
        
````
## Leetcode 77 Combinations : Backtracking problem

![IMG_FF17CB19A8F3-1](https://user-images.githubusercontent.com/81163933/175179968-5879e561-b794-4d08-a651-e89e8571a02f.jpeg)

`Idea`:
1. drawing decesion tree for backtracking problems is pretty helpful
2. pay attention to the for loop , pattern is quite similar to the genperm function, whenever you insert one new value and call recursively, you have to undo the changes and try new value, `this is the key of backtracking: try every combinations/ possibility`
3. basecase put at the top
        
````C++
void back_track(vector<vector<int>> &result,vector<int> cur_vec,int start, int end, int size){
    if(cur_vec.size() == size){
        result.push_back(cur_vec);
        return;
    }
    
    //for each layer, push one number range from start to end
    //and do recursive call
    for(int i = start;i < end+1;i++){
        cur_vec.push_back(i);    // |
                                 /* V set start to i+1, maintain increasing sequence*/
        back_track(result,cur_vec,i+1,end,size);
        cur_vec.pop_back();/*make sure we only insert one num, try different combination*/
    }
 
    
}
vector<vector<int>> combine(int n, int k) {
    vector<vector<int>> result{};
    vector<int> cur_vec{};
    back_track(result,cur_vec,1,n,k);
    return result;
}
````
        
        
## Leetcode 17 phone number combination
'IDEA: '
* use hash map to store the digits number and its corresponding string
* use backtracking to print out the combination
* for backtracking, key is to use index to serve as the permutation length.
* for each index, we need to find the digits number and its corresponding string(using map), and loop through that string to try different combination.
        
![IMG_7FDBA984AC82-1](https://user-images.githubusercontent.com/81163933/175774154-13b92865-a9b1-42ab-81f3-c1c673413204.jpeg)
````C++
void back_track(vector<string> &ret,string cur_str,int sz,int index, string &digits){
        //base case
       if(cur_str.size() == sz){
           ret.push_back(cur_str);
           return;
       }
       
       string str = my_map[digits[index] - '0'];
       
       //for this func loop through all the character of str
       for(int i = 0; i < str.size();i++){
           cur_str += str[i];
           
           back_track(ret, cur_str,sz,index + 1, digits);
           
           cur_str.pop_back();
       }
     
       
    }
    
    vector<string> letterCombinations(string digits) {
        if(digits.empty()){return vector<string>{};}
        set_map();
        vector<string> ret;
      
        back_track(ret,"",int(digits.size()),0,digits);
        
        return ret;
   
    }
````
## Leetcode 53 maximum contigous subarray
`Idea: `
1. using max_sum to record current best
2. whenever curSum is `negative` reset it
3. using cur_sum to record the contigous sum
4. cur_sum is in the idea of linear scaning and thus can make sure that the result is determined from contigous array
````C++
int maxSubArray(vector<int>& nums) {
         if(nums.empty()){return 0;}
    
    int curSum = 0;
    int max_sum = nums[0];
    
    for(auto & n : nums){
        if(curSum < 0){
            curSum = 0;
        }
        curSum += n;
        max_sum = max(curSum,max_sum);
    }
    
    return max_sum;
    }
````
## Leetcode 39, combinational sum
`Idea` 
 1. backtracking
 2. allow duplicate elements/ multiple use, try index first , not work, then pop it, try next index,
 3. TODO : Draw the decision tree
 ````C++
 void back_track(vector<vector<int>> &ret, vector<int> cur_vec,int &target,int index,vector<int>& candidates){
        if(index >= candidates.size()){return;}
    
       //base case
       if(!cur_vec.empty()){
           int sum = accumulate(cur_vec.begin(),cur_vec.end(),0);
           if(sum == target){
               ret.push_back(cur_vec);
               return;
           }
           
           //not possible
           if(sum > target){return;}
          
       }
       
       
            cur_vec.push_back(candidates[index]);
            back_track(ret,cur_vec,target,index,candidates);
            cur_vec.pop_back();
        back_track(ret,cur_vec,target,index + 1, candidates);
           
       
       
  
   }
   
   
   vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
       if(candidates.empty()){return vector<vector<int>>{};}
       
       vector<vector<int>> ret;
       back_track(ret,vector<int>{},target,0,candidates);
       return ret;
   }
 ````
