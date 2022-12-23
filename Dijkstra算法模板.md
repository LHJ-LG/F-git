c++版

```c++
// Dijkstra 算法模板
    // 返回从 start 到每个点的最短路
    vector<int> dijkstra(vector<vector<pair<int, int>>> &g, int start) {
        vector<int> dist(g.size(), INT_MAX);
        dist[start] = 0;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> pq;
        pq.emplace(0, start);
        while (!pq.empty()) {
            auto[d, x] = pq.top();
            pq.pop();
            if (d > dist[x]) continue;
            for (auto[y, wt] : g[x]) {
                int new_d = dist[x] + wt;
                if (new_d < dist[y]) {
                    dist[y] = new_d;
                    pq.emplace(new_d, y);
                }
            }
        }
        return dist;
    }
```

go版

```go
// 以下为 Dijkstra 算法模板
type neighbor struct{ to, weight int }

func dijkstra(g [][]neighbor, start int) []int {
    dist := make([]int, len(g))
    for i := range dist {
        dist[i] = math.MaxInt32
    }
    dist[start] = 0
    h := hp{{start, 0}}
    for len(h) > 0 {
        p := heap.Pop(&h).(pair)
        x := p.x
        if dist[x] < p.dist {
            continue
        }
        for _, e := range g[x] {
            y := e.to
            if d := dist[x] + e.weight; d < dist[y] {
                dist[y] = d
                heap.Push(&h, pair{y, d})
            }
        }
    }
    return dist
}

type pair struct{ x, dist int }
type hp []pair
func (h hp) Len() int              { return len(h) }
func (h hp) Less(i, j int) bool    { return h[i].dist < h[j].dist }
func (h hp) Swap(i, j int)         { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v interface{})   { *h = append(*h, v.(pair)) }
func (h *hp) Pop() (v interface{}) { a := *h; *h, v = a[:len(a)-1], a[len(a)-1]; return }

func min(a, b int) int { if a > b { return b }; return a }
func max(a, b int) int { if a < b { return b }; return a }
```

