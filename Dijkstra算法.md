## Dijkstra算法

【浙江大学】数据结构 P92 7.1.3有权图的单源最短路算法

URL:https://www.bilibili.com/video/BV1Kb41127fT?p=92

``` cpp
#include<iostream>
#include<stack>
using namespace std;

int e[10][10], dist[10], path[10], vis[10], n, m;
void init(){//初始化数组
	for(int i=1; i <= n; i++){
		for(int j=1; j <= n; j++)
			e[i][j] = INT_MAX;
		dist[i] = INT_MAX;
		vis[i] = 0;
		path[i] = -1;
	}
}
void Dijkstra(int s){//Dijkstra算法
	while(1){
		int v = -1;
		for(int i=1; i <= n; i++)//找出为收录顶点中dist最小者
			if(vis[i] == 0)
				if(v == -1) v = i;
				else if(dist[v] > dist[i]) v = i;
		if(v == -1) break;//未找到这样的v
		vis[v] = 1;
		for(int i=1; i <= n; i++)//寻找v的每个邻接点
			if(vis[i] == 0 && e[v][i] != INT_MAX)
				if(dist[v]+e[v][i] < dist[i]){
					dist[i] = dist[v] + e[v][i];
					path[i] = v;
				}
	}
}
int main(){
	cin >> n >> m;//点数以及边数
	init();//初始化
	int a, b, c;
	for(int i=0; i < m; i++){//读入边，从a到b的单向边，权值为c
		cin >> a >> b >> c;
		e[a][b] = c;
	}
	int s;
	cin >> s;//读入起始点
	dist[s] = 0; vis[s] = 1;//预处理
	for(int i=1; i <= n; i++)
		if(i != s && e[s][i] != INT_MAX){
			dist[i] = e[s][i];
			path[i] = s;
		}
	Dijkstra(s);//Dijkstra算法
	stack<int> sta;//利用堆栈输出路径
	while(cin >> a){//输出从起始点到a点的最短距离以及其路径
		cout << dist[a] << endl;
		while(path[a] != -1){
			sta.push(a);
			a = path[a];
		}
		cout << s;
		while(!sta.empty()){
			cout << " -> " << sta.top();
			sta.pop();
		}
		cout << endl;
	}
}
```

## input:

```
7 12
1 2 2
3 1 4
1 4 1
2 4 3
2 5 10
4 3 2
4 5 2
3 6 5
4 6 8
4 7 4
5 7 6
7 6 1
1
```

## output:

```
6
6
1 -> 4 -> 7 -> 6
5
3
1 -> 4 -> 5
4
1
1 -> 4
2
2
1 -> 2
7
5
1 -> 4 -> 7
1
0
1
3
3
1 -> 4 -> 3
```