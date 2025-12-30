# [데이크스트라 알고리즘](https://ko.wikipedia.org/wiki/데이크스트라_알고리즘)

최단경로 알고리즘

미방문 노드를 모두 ```infinity```로 초기화<br>
방문하지 않은 노드들 중 거리가 가장 가까운 노드를 우선탐색<br>
저장된 최단거리보다 현재 노드를 통해 가는 거리가 짧으면 덮어쓰기를 반복

```cpp
//우선순위 큐 정렬 구조체
int DK(vector<vector<int>> graph, int start, int goal)
{
    //변수 초기화
    int nodes = graph.size();
    vector<int> dist(nodes, INT_MAX);
    vector<bool> visit(nodes, false);
    priority_queue < pair<int, int>, vector<pair<int, int>>, greater<pair<int,int>>> q;

    dist[start] = 0;
    q.push({ dist[start],start});

    //알고리즘실행
    while (!q.empty()) {
        int current = q.top().second;
        q.pop();

        for (int i = 0; i < nodes; i++) {
            if (current == i)
                continue;

            if (graph[current][i] != -1)
            {
                dist[i] = min(dist[i], dist[current] + graph[current][i]);

                if (!visit[i]) {
                    visit[i] = true;
                    q.push({ dist[i], i });
                }
            }

        }
    }

    if (visit[goal])
        return dist[goal];
    else
        return -1;
}


```