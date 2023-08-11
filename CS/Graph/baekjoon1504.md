## **특정한 최단경로**


### ***problem***
방향성이 없는 그래프가 주어진다. 세준이는 1번 정점에서 N번 정점으로 최단 거리로 이동하려고 한다. 또한 세준이는 두 가지 조건을 만족하면서 이동하는 특정한 최단 경로를 구하고 싶은데, 그것은 바로 임의로 주어진 두 정점은 반드시 통과해야 한다는 것이다.

세준이는 한번 이동했던 정점은 물론, 한번 이동했던 간선도 다시 이동할 수 있다. 하지만 반드시 최단 경로로 이동해야 한다는 사실에 주의하라. 1번 정점에서 N번 정점으로 이동할 때, 주어진 두 정점을 반드시 거치면서 최단 경로로 이동하는 프로그램을 작성하시오.
#### **Input**
첫째 줄에 정점의 개수 N과 간선의 개수 E가 주어진다. (2 ≤ N ≤ 800, 0 ≤ E ≤ 200,000) 둘째 줄부터 E개의 줄에 걸쳐서 세 개의 정수 a, b, c가 주어지는데, a번 정점에서 b번 정점까지 양방향 길이 존재하며, 그 거리가 c라는 뜻이다. (1 ≤ c ≤ 1,000) 다음 줄에는 반드시 거쳐야 하는 두 개의 서로 다른 정점 번호 v1과 v2가 주어진다. (v1 ≠ v2, v1 ≠ N, v2 ≠ 1) 임의의 두 정점 u와 v사이에는 간선이 최대 1개 존재한다.

#### **Output**
첫째 줄에 두 개의 정점을 지나는 최단 경로의 길이를 출력한다. 그러한 경로가 없을 때에는 -1을 출력한다.

### ***Solution***
#### 다익스트라 풀이
``` java
import java.io.*;
import java.util.*;

public class Main {

    static int[][] arr;
    static int INF = 200_000_001;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int N = Integer.parseInt(st.nextToken());
        int E = Integer.parseInt(st.nextToken());

        arr = new int[N+1][N+1];

        for(int i=1;i<=N;i++){
            Arrays.fill(arr[i],INF);
        }

        for(int i=0;i<E;i++){
            String[] s = br.readLine().split(" ");
            int a = Integer.parseInt(s[0]);
            int b = Integer.parseInt(s[1]);
            int c = Integer.parseInt(s[2]);
            arr[a][b] = arr[b][a] = c;
        }

        String[] s = br.readLine().split(" ");
        int v1 = Integer.parseInt(s[0]);
        int v2 = Integer.parseInt(s[1]);
        int[] oneToPoint = dijkstra(N,1);
        int[] v1ToPoint = dijkstra(N,v1);
        int[] v2ToPoint = dijkstra(N,v2);
        int answer = -1;
        if(oneToPoint[v1] == INF || oneToPoint[v2] == INF || v1ToPoint[v2] == INF || v2ToPoint[v1] == INF || v1ToPoint
        [N] == INF || v2ToPoint[N] == INF){
            answer = -1;
        }
        else{
            answer = Math.min(oneToPoint[v1]+v1ToPoint[v2]+v2ToPoint[N], oneToPoint[v2]+v2ToPoint[v1]+v1ToPoint[N]);
        }
        bw.write(String.valueOf(answer) + "\n");
        bw.flush();
    }
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

    public static int[] dijkstra(int v, int start){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[v+1];
        boolean[] visited = new boolean[v+1];
        Arrays.fill(dist, INF);

        dist[start] = 0;
        pq.offer(new Node(start, 0));

        while(!pq.isEmpty()){
            int now = pq.poll().index;
            if(visited[now]) continue;
            visited[now] = true;
            for(int i=1;i<=v;i++){
                if(arr[now][i] == INF) continue;
                if(!visited[i] && dist[i] > dist[now] + arr[now][i]){
                    dist[i] = dist[now] + arr[now][i];
                    pq.offer(new Node(i,dist[i]));
                }
            }
        }
        return dist;
    }
}
```

#### 플로이드-와샬 풀이
``` java
import java.io.*;
import java.util.*;

public class Main {

    static int[][] arr;
    static int INF = 200_000_001;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int N = Integer.parseInt(st.nextToken());
        int E = Integer.parseInt(st.nextToken());

        arr = new int[N+1][N+1];

        for(int i=1;i<=N;i++){
            Arrays.fill(arr[i],INF);
        }

        for(int i=0;i<E;i++){
            String[] s = br.readLine().split(" ");
            int a = Integer.parseInt(s[0]);
            int b = Integer.parseInt(s[1]);
            int c = Integer.parseInt(s[2]);
            arr[a][b] = arr[b][a] = c;
        }

        String[] s = br.readLine().split(" ");
        int v1 = Integer.parseInt(s[0]);
        int v2 = Integer.parseInt(s[1]);
        int[][] d = floyd(N,arr);
        int answer = 0;
        if(d[1][v1] == INF || d[1][v2] == INF || d[v1][v2] == INF || d[v1][N] == INF || d[v2][N] == INF){
            answer = -1;
        }
        else{
            answer = Math.min(d[1][v1]+d[v1][v2]+d[v2][N], d[1][v2]+d[v2][v1]+d[v1][N]);
        }
        bw.write(String.valueOf(answer) + "\n");
        bw.flush();
    }

    private static int[][] floyd(int V, int[][] arr){
        int[][] d = new int[V+1][V+1];

        for(int i=1;i<=V;i++){
            d[i] = Arrays.copyOf(arr[i],V+1);
        }

        for(int i=1;i<=V;i++){
            d[i][i] = 0;
        }

        for(int k=1;k<=V;k++){
            for(int i=1;i<=V;i++){
                for(int j=1;j<=V;j++){
                    if(d[i][j] > d[i][k]+d[k][j]){
                        d[i][j] = d[i][k]+d[k][j];
                    }
                }
            }
        }

        return d;
    }

}
```
### **문제 풀이**
- 최단 거리 알고리즘인 다익스트라와 플로이드-와샬을 활용하는 문제이다.

- 특정 거점을 반드시 지나야하기 때문에 플로이드-와샬을 사용하는 것이 유리할 수 있지만, 문제의 조건에 노드의 갯수가 800이기때문에 여기서는 사용가능하지만 노드의 갯수가 더 많다면 시간초과가 날 수 있다.

- 다익스트라를 사용하면 **시작거점에서 모든 정점에 대한 배열**, **v1에서 모든 정점에 대한 배열**, **v2에서 모든 정점에 대한 배열** 총 거리에 대한 배열 3개를 구해야한다.


### **[출처]**
https://www.acmicpc.net/problem/1504