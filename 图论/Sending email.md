# Sending email
### UVA-10986

There are n SMTP servers connected by network cables. Each of the m cables connects two computers and has a certain latency measured in milliseconds required to send an email message. What
is the shortest time required to send a message from server S to server T along a sequence of cables?
Assume that there is no delay incurred at any of the servers.

Input

The first line of input gives the number of cases, N. N test cases follow. Each one starts with a line
containing n (2 ≤ n ≤ 20000), m (0 ≤ m ≤ 50000), S (0 ≤ S < n) and T (0 ≤ T < n). S ̸= T. The
next m lines will each contain 3 integers: 2 different servers (in the range [0, n − 1]) that are connected
by a bidirectional cable and the latency, w, along this cable (0 ≤ w ≤ 10000).

Output

For each test case, output the line ‘Case #x:’ followed by the number of milliseconds required to send
a message from S to T. Print ‘unreachable’ if there is no route from S to T.

## input:

```
3
2 1 0 1
0 1 100
3 3 2 0
0 1 100
0 2 200
1 2 50
2 0 0 1
```

## output:

```
Case #1: 100
Case #2: 150
Case #3: unreachable
```

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
vector<Edge> edge[20005];
typedef pair<int,int> P;
int n, m, s, t, dist[20005];
bool vis[20005];

void Dijkstra(){
    dist[s] = 0;
    priority_queue<P, vector<P>, greater<P> > pq;
    pq.push(P(0,s));

    while(!pq.empty()){
        P v = pq.top(); pq.pop();
        int v_dist = v.first;
        int v_n = v.second;
        if(v_dist > dist[v_n]) continue;
        vis[v_n] = true;
        if(v_n == t) break;

        for(int i=0; i < edge[v_n].size(); i++){
            Edge e = edge[v_n][i];
            int w_dist = v_dist + e.cost;
            int w_n = e.to;
            if(vis[w_n] == false && w_dist < dist[w_n]){
                dist[w_n] = w_dist;
                pq.push(P(dist[w_n], w_n));
            }
        }
    }
}
int main(){
    int kase, k=0;
    cin >> kase;
    while(kase--){
        cin >> n >> m >> s >> t;
        for(int i=0; i < n; i++){
            dist[i] = INF;
            vis[i] = false;
            edge[i].clear();
        }
        int a, b, c;
        Edge e;
        for(int i=0; i < m; i++){
            cin >> a >> b >> c;
            e.to = b;
            e.cost = c;
            edge[a].push_back(e);
            e.to = a;
            edge[b].push_back(e);
        }
        Dijkstra();
        cout << "Case #" << ++k << ": ";
        if(dist[t] == INF) cout << "unreachable\n";
        else cout << dist[t] << endl;
    }
}
```