`#climbing stairs, Leetcode 70`
````C++
 int climbStairs(int n) {
        if(n == 0 || n == 1){return 1;}
        if(n == 2){return 2;}
        
        const size_t SIZE = n+1;
        vector<int> memo(SIZE,0);
        memo[0] = 1;memo[1] = 1;memo[2] = 2;
        for(size_t i = 3; i < n+1;i++){
            memo[i] = memo[i-1] + memo[i-2] ;
        }
        return memo[n];
    } 
};
````

`#can jump out leetcode 55`

* method I;(Problematic). 
times: < O(n*nums.size()), space: O(n)
use dp, vector<bool> memo. from back to the start and check whether ecery node's accessible range have true value(means able to reach the end)
 Problem : Too slow, somehow exceed the runtime limits in leetcode
 ````C++
 vector<bool> dp(nums.size(),false);
    dp[nums.size()-1] = true;
    
    for(int i = static_cast<int>(nums.size()-2); i >=0 ;i--){
        for(size_t j = i; j < min(static_cast<int>(nums.size()),nums[i] + 1 +i);j++){
            if(dp[j]){dp[i] = true;}
        }
    }
    
    
    return dp[0];
 ````
 
 * method II: 
 time: O(n) space:O(n)
 dynamic programming: vector<int> memo, traverse from the front, record the range each element can reach
 ````C++
 // Method II: dp from the front
    if(nums.empty()){return true;}
            if(nums[0] == 0){return nums.size() == 1;}
        vector<int> memo(nums.size(),nums[0]);
        for(size_t i = 1;i < memo.size();i++){
            memo[i] = max(memo[i-1] -1, nums[i]);
            //the maximum range this position can reach
            
            if(memo[i] == 0 && i!=nums.size()-1){return false;}
        }
            return true;
 ````
                                              
 * method III:
 dynamic programming from the back
 
 run from the back, chech whether each element can reach the good index(which can reach to the end)
 Time: O(n) space: O(1)
 ````C++
        if(nums.empty()){return true;}
        if(nums.size() == 1){return true;}
        if(nums[0] == 0){return nums.size() == 1;}
        
        int lastGoodIndex = nums.size() - 1;
        for(int i = static_cast<int>(nums.size() - 2);i >= 0;i--){
            if(nums[i] + i >= lastGoodIndex){
                lastGoodIndex = i;
            }//if
        }//for
    
    return lastGoodIndex == 0;
 ````                           
                                              
                                             
 
