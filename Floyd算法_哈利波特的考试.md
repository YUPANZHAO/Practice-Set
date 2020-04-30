## 哈利波特的考试 Floyd算法

``` cpp
#include<iostream>
#include<iomanip>
using namespace std;

int dp[10][10], n, m, path[10][10];

void print_path(int a, int b){
	if(path[a][b] == -1){
		cout << a+1 << " -> ";
		return;
	}
	else{
		print_path(a,path[a][b]);
		print_path(path[a][b],b);
	}
}
int main(){
	cin >> n >> m;//n代表点数、m代表边数
	for(int i=0; i < n; i++)//初始化
		for(int j=0; j < n; j++){
			dp[i][j] = 100000;//100000为较大值
			path[i][j] = -1;
		}
	int a, b, c;//从a点到b点有一条权值为c的边
	for(int i=0; i < m; i++){
		cin >> a >> b >> c;
		dp[a-1][b-1] = c;
		dp[b-1][a-1] = c;
	}
	for(int k=0; k < n; k++)//Floyd算法
		for(int i=0; i < n; i++)
			for(int j=0; j < n; j++)
				if(i != j && dp[i][k]+dp[k][j] < dp[i][j]){
					dp[i][j] = dp[i][k]+dp[k][j];
					path[i][j] = k;
				}
	cout << endl;
	for(int i=0; i < n; i++){//打印dp数组
		for(int j=0; j < n; j++)
			if(dp[i][j] == 100000) cout << setw(5) << -1;
			else cout << setw(5) << dp[i][j];
		cout << endl;
	}
	cout << endl;
	for(int i=0; i < n; i++){//打印path数组
		for(int j=0; j < n; j++)
			cout << setw(5) << path[i][j];
		cout << endl;
	}
	while(cin >> a >> b){
		cout << dp[a-1][b-1] << endl;//输出从a点到b点的最小权值和
		print_path(a-1,b-1);//输出路径
		cout << b << endl;
	}
}
```

## input:

```
6 11
3 4 70
1 2 1
5 4 50
2 6 50
5 6 60
1 3 70
4 6 60
3 6 80
5 1 100
2 4 60
5 2 80
```

## output:

```
   -1    1   70   61   81   51
    1   -1   71   60   80   50
   70   71   -1   70  120   80
   61   60   70   -1   50   60
   81   80  120   50   -1   60
   51   50   80   60   60   -1

   -1   -1   -1    1    1    1
   -1   -1    0   -1   -1   -1
   -1    0   -1   -1    3   -1
    1   -1   -1   -1   -1   -1
    1   -1    3   -1   -1   -1
    1   -1   -1   -1   -1   -1

1 3
70
1 -> 3
3 1
70
3 -> 1
1 5
81
1 -> 2 -> 5
1 6
51
1 -> 2 -> 6
6 1
51
6 -> 2 -> 1
6 5
60
6 -> 5
```