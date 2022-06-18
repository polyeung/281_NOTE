`# 1029 two city scheduling`
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
