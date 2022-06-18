#climbing stairs, Leetcode 70
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
