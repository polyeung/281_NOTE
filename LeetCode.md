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

`# count number of rectangle, eecs281 question`

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
