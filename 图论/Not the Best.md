# Not the Best

### LightOJ-1099

Robin has moved to a small village and sometimes enjoys returning to visit one of his best friends. He does not want to get to his old home too quickly, because he likes the scenery along the way. He has decided to take the second-shortest rather than the shortest path. He knows there must be some second-shortest path.

The countryside consists of R bidirectional roads, each linking two of the N intersections, conveniently numbered from 1 to N. Robin starts at intersection 1, and his friend (the destination) is at intersection N.

The second-shortest path may share roads with any of the shortest paths, and it may backtrack i.e., use the same road or intersection more than once. The second-shortest path is the shortest path whose length is longer than the shortest path(s) (i.e., if two or more shortest paths exist, the second-shortest path is the one whose length is longer than those but no longer than any other path).

Input

Input starts with an integer T (≤ 10), denoting the number of test cases.

Each case contains two integers N (1 ≤ N ≤ 5000) and R (1 ≤ R ≤ 105). Each of the next R lines contains three space-separated integers: u, v and w that describe a road that connects intersections u and v and has length w (1 ≤ w ≤ 5000).

Output

For each case, print the case number and the second best shortest path as described above.

## input:

```
2
3 3
1 2 100
2 3 200
1 3 50
4 4
1 2 100
2 4 200
2 3 250
3 4 100
```

## output:

```
Case 1: 150
Case 2: 450
```
## 题解

Dijkstra算法

求次短路，用 dist1 储存最短路长度，dist2 储存次短路长度

取消 vis 判断各节点是否被收录，让每条边可以走多次，往回走

## code:

``` cpp
#include<iostream>
#include<queue>
#include<vector>
using namespace std;

const int INF = 2000000000;
struct Edge{
    int to;
    int cost;
};
vector<Edge> edge[5005];
typedef pair<int,int> P;
int dist1[5005], dist2[5005];
int n, r;

void Dijkstra(){
    dist1[0] = 0;
    priority_queue<P,vector<P>,greater<P> > pq;
    pq.push(P(0,0));
    while(!pq.empty()){
        P v = pq.top(); pq.pop();
        int v_dist = v.first;
        int v_n = v.second;
        if(v_dist > dist2[v_n]) continue;

        for(int i=0; i < edge[v_n].size(); i++){
            Edge e = edge[v_n][i];
            int w_dist = v_dist + e.cost;
            int w_n = e.to;
            if(dist1[w_n] > w_dist){ // 判断是否为最短路
                swap(dist1[w_n], w_dist);
                pq.push(P(dist1[w_n], w_n));
            }
            if(dist2[w_n] > w_dist && dist1[w_n] < w_dist){ //判断是否为次短路
                dist2[w_n] = w_dist;
                pq.push(P(dist2[w_n], w_n));
            }
        }
    }
}
int main(){
    int t, k = 0;
    cin >> t;
    while(t--){
        cin >> n >> r;
        for(int i=0; i < n; i++){
            dist1[i] = INF;
            dist2[i] = INF;
            edge[i].clear();
        }
        int u, v, w;
        Edge e;
        for(int i=0; i < r; i++){
            cin >> u >> v >> w;
            e.to = v-1;
            e.cost = w;
            edge[u-1].push_back(e);
            e.to = u-1;
            edge[v-1].push_back(e);
        }
        Dijkstra();
        cout << "Case " << ++k << ": " << dist2[n-1] << endl;
    }
}
```