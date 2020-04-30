# 迷宫问题
### POJ-3984

定义一个二维数组：

```
int maze[5][5] = {

	0, 1, 0, 0, 0,

	0, 1, 0, 1, 0,

	0, 0, 0, 0, 0,

	0, 1, 1, 1, 0,

	0, 0, 0, 1, 0,

};
```

它表示一个迷宫，其中的1表示墙壁，0表示可以走的路，只能横着走或竖着走，不能斜着走，要求编程序找出从左上角到右下角的最短路线。

## input:

一个5 × 5的二维数组，表示一个迷宫。数据保证有唯一解。

```
0 1 0 0 0
0 1 0 1 0
0 0 0 0 0
0 1 1 1 0
0 0 0 1 0
```

## output:

左上角到右下角的最短路径，格式如样例所示。

```
(0, 0)
(1, 0)
(2, 0)
(2, 1)
(2, 2)
(2, 3)
(2, 4)
(3, 4)
(4, 4)
```

## code:

``` cpp
#include<iostream>
#include<cstring>
#include<queue>
#include<cstdio>
using namespace std;

struct point{
    int x;
    int y;
    int t;
    point * last;
}map[5][5];
queue <point> q;
bool vis[5][5];
int dir[2][4] = {{0,0,1,-1},{1,-1,0,0}};

void bfs(){
    q.push(map[4][4]);
    vis[4][4] = true;
    point *head;

    while(!q.empty()){
        head = &q.front();
        q.pop();
        for(int i=0; i < 4; i++){
            int x = head->x+dir[0][i],y = head->y+dir[1][i];
            if(head->t || vis[x][y] || x < 0 || x >= 5 || y < 0 || y >= 5)
                continue;
            map[x][y].last = head;
            vis[x][y] = true;
            q.push(map[x][y]);
            if(x == 0 && y == 0) return;
        }
    }
}
int main(){
    for(int i=0; i < 5; i++){
        for(int j=0; j < 5; j++){
            map[i][j].t = getchar() - '0'; getchar();
            map[i][j].x = i;
            map[i][j].y = j;
            map[i][j].last = &map[i][j];
        }
    }
    memset(vis,false,sizeof(vis));
    bfs();
    point *print = &map[0][0];
    while(!(print->x == 4 && print->y == 4)){
        cout << "(" << print->x << ", " << print->y << ")\n";
        print = print->last;
    }
    cout << "(4, 4)\n";
    return 0;
}
```

