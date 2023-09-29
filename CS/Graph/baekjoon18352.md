## **특정 거리의 도시 찾기**


### ***problem***
어떤 나라에는 1번부터 N번까지의 도시와 M개의 단방향 도로가 존재한다. 모든 도로의 거리는 1이다.

이 때 특정한 도시 X로부터 출발하여 도달할 수 있는 모든 도시 중에서, 최단 거리가 정확히 K인 모든 도시들의 번호를 출력하는 프로그램을 작성하시오. 또한 출발 도시 X에서 출발 도시 X로 가는 최단 거리는 항상 0이라고 가정한다.

예를 들어 N=4, K=2, X=1일 때 다음과 같이 그래프가 구성되어 있다고 가정하자.

<p align = "center">
    <img src= "https://upload.acmicpc.net/a5e311d7-7ce4-4638-88a5-3665fb4459e5/-/preview/" width="300px">
</p>

이 때 1번 도시에서 출발하여 도달할 수 있는 도시 중에서, 최단 거리가 2인 도시는 4번 도시 뿐이다.  2번과 3번 도시의 경우, 최단 거리가 1이기 때문에 출력하지 않는다.

#### **Input**
첫째 줄에 도시의 개수 N, 도로의 개수 M, 거리 정보 K, 출발 도시의 번호 X가 주어진다. (2 ≤ N ≤ 300,000, 1 ≤ M ≤ 1,000,000, 1 ≤ K ≤ 300,000, 1 ≤ X ≤ N) 둘째 줄부터 M개의 줄에 걸쳐서 두 개의 자연수 A, B가 공백을 기준으로 구분되어 주어진다. 이는 A번 도시에서 B번 도시로 이동하는 단방향 도로가 존재한다는 의미다. (1 ≤ A, B ≤ N) 단, A와 B는 서로 다른 자연수이다.

#### **Output**
X로부터 출발하여 도달할 수 있는 도시 중에서, 최단 거리가 K인 모든 도시의 번호를 한 줄에 하나씩 오름차순으로 출력한다.

이 때 도달할 수 있는 도시 중에서, 최단 거리가 K인 도시가 하나도 존재하지 않으면 -1을 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    static class Node implements Comparable<Node>{
        int index, cost;
        public Node(int index, int cost){
            this.index = index;
            this.cost = cost;
        }

        @Override
        public int compareTo(Node e){
            return Integer.compare(this.cost, e.cost);
        }
    }
    static int[] dist;
    static List<Integer>[] list;
    static int INF = 1_000_001;


    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());
        int X = Integer.parseInt(st.nextToken());

        list = new ArrayList[N+1];

        for(int i=1;i<=N;i++){
            list[i] = new ArrayList<>();
        }

        for(int i=0;i<M;i++){
            String[] s = br.readLine().split(" ");
            int a = Integer.parseInt(s[0]);
            int b = Integer.parseInt(s[1]);
            list[a].add(b);
        }
        bfs(N, X, K);
        StringBuilder sb = new StringBuilder();

        List<Integer> result = new ArrayList<>();

        for(int i=1;i<=N;i++){
            if(dist[i] == K){
                result.add(i);
            }
        }
        if(result.size() == 0){
            sb.append("-1");
        }
        else{
            for(int i=0;i<result.size();i++){
                sb.append(result.get(i));
                if(i == result.size()-1){
                    continue;
                }
                sb.append("\n");
            }
        }
        bw.write(sb.toString());
        bw.flush();
    }

    private static void bfs(int v, int start, int k){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        dist = new int[v+1];
        boolean[] visited = new boolean[v+1];
        Arrays.fill(dist, INF);
        dist[start] = 0;

        pq.offer(new Node(start,0));

        while(!pq.isEmpty()){
            int now = pq.poll().index;
            if(dist[now] >= k){
                continue;
            }

            for(int n : list[now]){
                if(!visited[n] && dist[n] > dist[now]+1){
                    visited[n] = true;
                    dist[n] = dist[now]+1;
                    pq.offer(new Node(n, dist[n]));
                }
            }
        }
    }

}
```
### **문제 풀이**
- 1번 도시에서 다른 도시들에 대한 최단거리를 구하는 기본적인 최단 거리 알고리즘 문제이다. 

- 각 노드간의 길이가 `1`이기 때문에 bfs로도 문제를 해결할 수 있다.

- 노드의 최대 갯수가 300,000이기 때문에 인접행렬이 아닌 인접리스트를 생성해준다.

- 문제에서 bfs로 문제를 해결할 수 있고 각 노드간의 길이가 1씩 차이가 난다고 우선순위큐를 사용할 때 dist에 대해서 우선순위를 만들어주지 않으면 출력초과가 발생할 수 있다.
    - 가장 가까운 index에 대해서 먼저 처리하지 않는 경우가 발생하기 때문이다.

### **[출처]**
https://www.acmicpc.net/problem/18352