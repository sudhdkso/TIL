## **최단경로**


### ***problem***
방향그래프가 주어지면 주어진 시작점에서 다른 모든 정점으로의 최단 경로를 구하는 프로그램을 작성하시오. 단, 모든 간선의 가중치는 10 이하의 자연수이다.

#### **Input**
첫째 줄에 정점의 개수 V와 간선의 개수 E가 주어진다. (1 ≤ V ≤ 20,000, 1 ≤ E ≤ 300,000) 모든 정점에는 1부터 V까지 번호가 매겨져 있다고 가정한다. 둘째 줄에는 시작 정점의 번호 K(1 ≤ K ≤ V)가 주어진다. 셋째 줄부터 E개의 줄에 걸쳐 각 간선을 나타내는 세 개의 정수 (u, v, w)가 순서대로 주어진다. 이는 u에서 v로 가는 가중치 w인 간선이 존재한다는 뜻이다. u와 v는 서로 다르며 w는 10 이하의 자연수이다. 서로 다른 두 정점 사이에 여러 개의 간선이 존재할 수도 있음에 유의한다.

#### **Output**
첫째 줄부터 V개의 줄에 걸쳐, i번째 줄에 i번 정점으로의 최단 경로의 경로값을 출력한다. 시작점 자신은 0으로 출력하고, 경로가 존재하지 않는 경우에는 INF를 출력하면 된다.

### ***Solution***
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
    static List<Node>[] nodeList;
    static int INF = Integer.MAX_VALUE;
    static int[] dist;
    static boolean[] visited;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int V = Integer.parseInt(st.nextToken());
        int E = Integer.parseInt(st.nextToken());

        int start = Integer.parseInt(br.readLine());
        nodeList = new List[V+1];

        for(int i=1;i<=V;i++){
            nodeList[i] = new ArrayList<>();
        }

        for(int i=0;i<E;i++){
            String[] s = br.readLine().split(" ");
            int a = Integer.parseInt(s[0]);
            int b = Integer.parseInt(s[1]);
            int c = Integer.parseInt(s[2]);
            nodeList[a].add(new Node(b,c));
        }

        dijkstra(V,start);
        StringBuilder sb = new StringBuilder();

        for(int i=1;i<=V;i++){
            if(!visited[i]){
                sb.append("INF").append("\n");
            }
            else{
                sb.append(dist[i]).append("\n");
            }
        }
        bw.write(sb.toString() + "\n");
        bw.flush();
    }

    public static void dijkstra(int v, int start){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        dist = new int[v+1];
        visited = new boolean[v+1];
        Arrays.fill(dist, INF);

        dist[start] = 0;
        pq.offer(new Node(start, 0));

        while(!pq.isEmpty()){
            int now = pq.poll().index;
            visited[now] = true;
            for(Node n : nodeList[now]){
                if(!visited[n.index] && dist[n.index] > dist[now] + n.cost){
                    dist[n.index] = dist[now] + n.cost;
                    pq.offer(new Node(n.index, dist[n.index]));
                }
            }
        }
    }
}
```
### **문제 풀이**
- 최단 거리 알고리즘인 다익스트라를 사용하는 문제이다.

- 최단거리 알고리즘은 다익스트라와 플로이드-와샬 알고리즘 두가지 종류가 있지만, start로 주어지는 하나의 정점에서 모든 정점에 대한 최단 거리를 구하는 문제이기 때문에 다익스트라 알고리즘을 사용하였다.


### **[출처]**
https://www.acmicpc.net/problem/1753