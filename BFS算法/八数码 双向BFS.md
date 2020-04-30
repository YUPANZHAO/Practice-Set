# Eight
### POJ-1077

The 15-puzzle has been around for over 100 years; even if you don't know it by that name, you've seen it. It is constructed with 15 sliding tiles, each with a number from 1 to 15 on it, and all packed into a 4 by 4 frame with one tile missing. Let's call the missing tile 'x'; the object of the puzzle is to arrange the tiles so that they are ordered as:

```
 1  2  3  4 

 5  6  7  8 

 9 10 11 12 

13 14 15  x 
```

where the only legal operation is to exchange 'x' with one of the tiles with which it shares an edge. As an example, the following sequence of moves solves a slightly scrambled puzzle:

```
 1  2  3  4    1  2  3  4    1  2  3  4    1  2  3  4 

 5  6  7  8    5  6  7  8    5  6  7  8    5  6  7  8 

 9  x 10 12    9 10  x 12    9 10 11 12    9 10 11 12 

13 14 11 15   13 14 11 15   13 14  x 15   13 14 15  x 

           r->           d->           r-> 
```

The letters in the previous row indicate which neighbor of the 'x' tile is swapped with the 'x' tile at each step; legal values are 'r','l','u' and 'd', for right, left, up, and down, respectively.

Not all puzzles can be solved; in 1870, a man named Sam Loyd was famous for distributing an unsolvable version of the puzzle, and
frustrating many people. In fact, all you have to do to make a regular puzzle into an unsolvable one is to swap two tiles (not counting the missing 'x' tile, of course).

In this problem, you will write a program for solving the less well-known 8-puzzle, composed of tiles on a three by three
arrangement.

You will receive a description of a configuration of the 8 puzzle. The description is just a list of the tiles in their initial positions, with the rows listed from top to bottom, and the tiles listed from left to right within a row, where the tiles are represented by numbers 1 to 8, plus 'x'. For example, this puzzle

```
 1  2  3 

 x  4  6 

 7  5  8 
 ```

is described by this list:

```
 1 2 3 x 4 6 7 5 8 
```

You will print to standard output either the word ``unsolvable'', if the puzzle has no solution, or a string consisting entirely of the letters 'r', 'l', 'u' and 'd' that describes a series of moves that produce a solution. The string should include no spaces and start at the beginning of the line.

## input:

```
 2  3  4  1  5  x  7  6  8 
```

## output:

```
ullddrurdllurdruldr
```

## code:

``` cpp
#include<iostream>
#include<cstdio>
#include<queue>
#include<set>
using namespace std;

const int MAX = 400000;

struct Point{
    int n;
    char oper;
    int father;
    int self;
}ans1[MAX], ans2[MAX];
queue <Point> q1;
queue <Point> q2;
set <int> p1;
set <int> p2;
int k1,k2;

int change(int n,char oper){ //返回oper操作后的状态，若操纵非法则返回0，例如：n=234150768,oper='u',返回值为230154768 
    int map[9],x;
    for(int i=8; i >= 0; i--){
        map[i] = n%10;
        if(map[i] == 0) x = i;
        n /= 10;
    }
    int temp;
    switch(oper){ //移动
        case 'u': if(x-3 < 0) return 0; temp = map[x]; map[x] = map[x-3]; map[x-3] = temp; break;
        case 'd': if(x+3 >= 9) return 0; temp = map[x]; map[x] = map[x+3]; map[x+3] = temp; break;
        case 'r': if((x+1)%3 == 0) return 0; temp = map[x]; map[x] = map[x+1]; map[x+1] = temp; break;
        case 'l': if(x%3 == 0) return 0; temp = map[x]; map[x] = map[x-1]; map[x-1] = temp; break;
    }
    x = 0;
    for(int i=0; i < 9; i++)
        x = x*10 + map[i];
    return x;
}

int bfs(){ //双向BFS
    q1.push(ans1[0]);
    q2.push(ans2[0]);
    p1.insert(ans1[0].n);
    p2.insert(ans2[0].n);
    Point *head;
    int next;
    char dir[5] = "udlr";
    while(!q1.empty() && !q2.empty()){
        head = &q1.front();
        q1.pop();
        for(int i=0; i < 4; i++){
            next = change(head->n,dir[i]);
            if(!next) continue;
            if(p1.count(next)) continue;
            ans1[k1].n = next;
            ans1[k1].oper = dir[i];
            ans1[k1].father = head->self;
            q1.push(ans1[k1]);
            p1.insert(next);
            k1++;
            if(p2.count(next)) return next;
        }
        head = &q2.front();
        q2.pop();
        for(int i=0; i < 4; i++){
            next = change(head->n,dir[i]);
            if(!next) continue;
            if(p2.count(next)) continue;
            ans2[k2].n = next;
            ans2[k2].oper = dir[i];
            ans2[k2].father = head->self;
            q2.push(ans2[k2]);
            p2.insert(next);
            k2++;
            if(p1.count(next)) return next;
        }
    }
    return 0;
}

bool isSolveable (int *statue)
{
    int arr[10], n = 0, num = 0;
    for (int i = 0; i < 9; i++)
    {
        if (statue[i] != 0) arr[++n] = statue[i];
    }
    for (int i = 1; i <= 8; i++)
    {
        for (int j = i + 1; j <= 8; j++)
        {
            if (arr[i] > arr[j]) num++;
        }
    }
    if (num & 0x1) return false;
    return true;
}

int main(){
    int n;
    char c;
    while(1){
        if(cin.peek() == EOF) break;
        n = 0;
        int sst[10];
        for(int i=0; i < 9; i++){ //将输入数据转换成状态值，x用数字0代替
            if((c = getchar()) == 'x'){n = n*10; sst[i] = 0;}
            else {n = n*10 + c - '0'; sst[i] = c - '0';}
            getchar();
        }
        if(!isSolveable(sst)){
            printf("unsolvable\n");
            continue;
        }
        ans1[0].n = n;
        ans1[0].father = 0;
        ans2[0].n = 123456780;
        ans2[0].father = 0;
        for(int i=0; i < MAX; i++){
            ans1[i].self = i;
            ans2[i].self = i;
        }
        int mid;
        k1 = k2 = 1;
        while(!q1.empty()) q1.pop();
        while(!q2.empty()) q2.pop();
        p1.clear();
        p2.clear();
        mid = bfs();
        if(mid == 0){
            printf("unsolvable\n");
        }
        else if(mid == ans1[k1-1].n){
            char print[100];
            int m=0,i;
            for(i=k1-1; i != 0; i = ans1[i].father){
                print[m++] = ans1[i].oper;
            }
            for(i=m-1; i >= 0; i--){
                printf("%c",print[i]);
            }
            for(i=k2-1; ans2[i].n != mid; i--);
            //printf("  ");
            for(; i != 0; i = ans2[i].father){
                char c;
                switch (ans2[i].oper){
                    case 'u': c = 'd'; break;
                    case 'd': c = 'u'; break;
                    case 'l': c = 'r'; break;
                    case 'r': c = 'l'; break;
                }
                printf("%c",c);
            }
            printf("\n");
        }
        else{
            char print[100];
            int m=0,i;
            for(i=k1-1; ans1[i].n != mid; i--);
            for(; i != 0; i = ans1[i].father){
                print[m++] = ans1[i].oper;
            }
            for(i=m-1; i >= 0; i--){
                printf("%c",print[i]);
            }
            //printf("  ");
            for(i=k2-1; i != 0; i = ans2[i].father){
                char c;
                switch (ans2[i].oper){
                    case 'u': c = 'd'; break;
                    case 'd': c = 'u'; break;
                    case 'l': c = 'r'; break;
                    case 'r': c = 'l'; break;
                }
                printf("%c",c);
            }
            printf("\n");
        }
    }
    return 0;
}
```