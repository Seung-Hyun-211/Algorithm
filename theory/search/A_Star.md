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
#define COST_UNIT 10        //g값과 단위를 맞추기 위한 수
#define STRAIGHT_COST 10    //상하좌우 이동시 g에 추가하는 코스트
#define DIAGONAL_COST 14    //대각선 이동시 g에 추가하는 코스트
#define MOVEABLE_DIRECTION 8
#define MAX_MAP_SIZE 1000

struct Node {
    Node(int px, int py, int g, int h) : px(px), py(py), g(g), h(h) {}

    int px;
    int py;
    int g;
    int h;

    int F() const { return g + h; };
    void Print(){ 
        cout << "/ x " << px << "/ y " << py << "/ g " << g << "/ h " << h <<"/ f" << F() << endl;
    }
};
struct NodeCompare
{
    bool operator()(const Node& n1, const Node& n2) const {
        return n1.F() > n2.F();
    }
};
int GetH(const int& cx, const int& cy, const int& gx, const int& gy) {
    int dx = cx - gx;
    int dy = cy - gy;
    return (int)floor(COST_UNIT * sqrt(dx * dx + dy * dy));
}


int AStar(const vector<vector<bool>>& m, const int& startx, const int& starty, const int& goalx, const int& goaly)
{
    int step = 0;
    priority_queue<Node, vector<Node>, NodeCompare> q;
    vector<vector<bool>> visit(MAX_MAP_SIZE, vector<bool>(MAX_MAP_SIZE, false));
    q.push(Node(startx, starty, 0, GetH(startx, starty, goalx, goaly)));

    //8방향 이동
    //0포함 짝수 -> 십자, 홀수->대각
    int dirx[8] = {  0, 1, 1, 1, 0,-1,-1,-1 };
    int diry[8] = { -1,-1, 0, 1, 1, 1, 0,-1 };

    while (!q.empty()) {
        Node n = q.top();
        visit[n.py][n.px] = true;
        n.Print();
        if (n.px == goalx && n.py == goaly)
        {
            return step;
        }
        q.pop();
        for (int i = 0; i < MOVEABLE_DIRECTION; i++)
        {
            int nx = n.px + dirx[i];
            int ny = n.py + diry[i];
            if (nx < 0 || ny < 0 || nx >= MAX_MAP_SIZE || ny >= MAX_MAP_SIZE || m[ny][nx] == false || visit[ny][nx] == true)
                continue;

            int gCost = i % 2 == 0 ? STRAIGHT_COST : DIAGONAL_COST;
            q.push(Node(nx, ny, n.g + gCost, GetH(nx, ny, goalx, goaly)));
        }

        step++;
    }

    return -1;
}
```