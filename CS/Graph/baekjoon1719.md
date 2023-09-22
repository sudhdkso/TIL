## **택배**


### ***problem***
명우기업은 2008년부터 택배 사업을 새로이 시작하기로 하였다. 우선 택배 화물을 모아서 처리하는 집하장을 몇 개 마련했지만, 택배 화물이 각 집하장들 사이를 오갈 때 어떤 경로를 거쳐야 하는지 결정하지 못했다. 어떤 경로를 거칠지 정해서, 이를 경로표로 정리하는 것이 여러분이 할 일이다.
<p align="left">
<img src="https://www.acmicpc.net/JudgeOnline/upload/201005/taekbae.PNG">
</p>

예시된 그래프에서 굵게 표시된 1, 2, 3, 4, 5, 6은 집하장을 나타낸다. 정점간의 간선은 두 집하장간에 화물 이동이 가능함을 나타내며, 가중치는 이동에 걸리는 시간이다. 이로부터 얻어내야 하는 경로표는 다음과 같다.

<p align="left">
<img src="https://www.acmicpc.net/JudgeOnline/upload/201005/tktk.PNG">
</p>

경로표는 한 집하장에서 다른 집하장으로 최단경로로 화물을 이동시키기 위해 가장 먼저 거쳐야 하는 집하장을 나타낸 것이다. 예를 들어 4행 5열의 6은 4번 집하장에서 5번 집하장으로 최단 경로를 통해 가기 위해서는 제일 먼저 6번 집하장으로 이동해야 한다는 의미이다.

이와 같은 경로표를 구하는 프로그램을 작성하시오.

#### **Input**
첫째 줄에 두 수 n과 m이 빈 칸을 사이에 두고 순서대로 주어진다. n은 집하장의 개수로 200이하의 자연수, m은 집하장간 경로의 개수로 10000이하의 자연수이다. 이어서 한 줄에 하나씩 집하장간 경로가 주어지는데, 두 집하장의 번호와 그 사이를 오가는데 필요한 시간이 순서대로 주어진다. 집하장의 번호들과 경로의 소요시간은 모두 1000이하의 자연수이다.

#### **Output**
예시된 것과 같은 형식의 경로표를 출력한다.

### ***Solution***
#### 다익스트라 풀이
``` java
import java.io.*;
import java.util.*;

public class Main {
    static class Node implements Comparable<Node>{
        int index,cost;

        public Node(int index, int cost){
            this.index = index;
            this.cost = cost;
        }

        @Override
        public int compareTo(Node e){
            return Integer.compare(this.cost, e.cost);
        }
    }
    static int INF =  Integer.MAX_VALUE;
    static List<Node>[] nodes;
    static int N;

    static int[][] result;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        result = new int[N+1][N+1];

        nodes = new List[N+1];
        for(int i=1;i<=N;i++){
            nodes[i] = new ArrayList<>();
        }

        for(int i=0;i<M;i++){
            st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = Integer.parseInt(st.nextToken());

            nodes[a].add(new Node(b,c));
            nodes[b].add(new Node(a,c));
        }

        for(int i=1;i<=N;i++){
            dijkstra(i);
        }

        StringBuilder sb = new StringBuilder();

        for(int i=1;i<=N;i++){
            for(int j=1;j<=N;j++){
                if(i == j){
                    sb.append("- ");
                }
                else{
                    sb.append(result[i][j]+" ");
                }
            }
            sb.append("\n");
        }
        bw.write(sb.toString());
        bw.flush();
    }
    private static void dijkstra(int start){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[N+1];
        boolean[] visited = new boolean[N+1];

        Arrays.fill(dist, INF);
        dist[start] = 0;
        pq.offer(new Node(start,0));

        while(!pq.isEmpty()){
            int index = pq.poll().index;

            if(visited[index]) continue;
            visited[index] = true;

            for(Node n : nodes[index]){
                if(!visited[n.index] && dist[n.index] > dist[index] + n.cost){
                    dist[n.index] = dist[index] + n.cost;
                    result[n.index][start] = index;
                    pq.offer(new Node(n.index, dist[n.index]));
                }
            }
        }
    }
}
```
#### 플로이드-와샬 풀이
``` java
import java.io.*;
import java.util.*;

public class Main {
    
    static int INF = 2_000_001;
    static int N;

    static int[][] result;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        int[][] floyd = new int[N + 1][N + 1];
        result = new int[N + 1][N + 1];

        for(int i=1;i<=N;i++){
            Arrays.fill(floyd[i], INF);
            floyd[i][i] = 0;
        }
        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine(), " ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = Integer.parseInt(st.nextToken());

            if(floyd[a][b] > c){
                floyd[a][b] = floyd[b][a] = c;
                result[a][b] = b;
                result[b][a] = a;
            }
        }

        for(int d = 1; d<= N;d++){
            for(int i=1;i<=N;i++){
                for(int j=1;j<=N;j++){
                    if(floyd[i][j] > floyd[i][d] + floyd[d][j]){
                        floyd[i][j] = floyd[i][d] + floyd[d][j];
                        result[i][j] = result[i][d];
                    }
                }
            }
        }

        StringBuilder sb = new StringBuilder();

        for (int i = 1; i <= N; i++) {
            for (int j = 1; j <= N; j++) {
                if (i == j) {
                    sb.append("- ");
                } else {
                    sb.append(result[i][j] + " ");
                }
            }
            sb.append("\n");
        }
        bw.write(sb.toString());
        bw.flush();
    }
}
```
### **문제 풀이**
- 집하장 간의 최단거리와 A집하장에서 B집하장으로 물건이 이동할 때 가장 먼저 지나가는 집하장의 번호들을 출력해야하는 문제이다.

- 1부터 N번까지 모든 번호에 대해서 다익스트라 알고리즘을 사용해도 되고, 그냥 플로이드-와샬 알고리즘 한번 돌려도 문제를 해결할 수 있다.



### **[출처]**
https://www.acmicpc.net/problem/1719