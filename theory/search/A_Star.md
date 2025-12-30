# [A Star Algorithm](https://ko.wikipedia.org/wiki/A*_알고리즘)

최단경로 알고리즘

[데이크스트라](./Dijkstra.md) 알고리즘과 유사하지만<br>
각 꼭짓점을 통과하는 최상의 경로를 추정값(```휴리스틱 추정값 h(x)```)을 매기는 방법을 이용한다.

```h(x)```를 구하는 방식<br>
 - 휴스턴 : 상,하,좌,우 4방향 ```int h = abs(dx) + abs(dy);``` <br>
 - 유클리드 : 대각선 포함 8방향 ```int h = (int)floor(sqrt(dx*dx + dy*dy));```


```f(x) = g(x) + h(x)```<br>
```g(n)``` : 출발 지점부터 꼭짓점 n 까지의 경로 가중치<br>
```h(n)``` : 꼭짓점 n 으로부터 목표까지의 추정 경로 가중치

```cpp
int AStar(int sx, int sy, int gx, int gy)
{
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> q;
    vector<vecotr<vector<int>>> m(1000,vector<vector<int>>(1000, vector<int>(2,INT_MAX)));
    


    q.push({0,0});



}

```