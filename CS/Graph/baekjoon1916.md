## **최소비용 구하기**


### ***problem***
N개의 도시가 있다. 그리고 한 도시에서 출발하여 다른 도시에 도착하는 M개의 버스가 있다. 우리는 A번째 도시에서 B번째 도시까지 가는데 드는 버스 비용을 최소화 시키려고 한다. A번째 도시에서 B번째 도시까지 가는데 드는 최소비용을 출력하여라. 도시의 번호는 1부터 N까지이다.

#### **Input**
첫째 줄에 도시의 개수 N(1 ≤ N ≤ 1,000)이 주어지고 둘째 줄에는 버스의 개수 M(1 ≤ M ≤ 100,000)이 주어진다. 그리고 셋째 줄부터 M+2줄까지 다음과 같은 버스의 정보가 주어진다. 먼저 처음에는 그 버스의 출발 도시의 번호가 주어진다. 그리고 그 다음에는 도착지의 도시 번호가 주어지고 또 그 버스 비용이 주어진다. 버스 비용은 0보다 크거나 같고, 100,000보다 작은 정수이다.

그리고 M+3째 줄에는 우리가 구하고자 하는 구간 출발점의 도시번호와 도착점의 도시번호가 주어진다. 출발점에서 도착점을 갈 수 있는 경우만 입력으로 주어진다.

#### **Output**
첫째 줄에 출발 도시에서 도착 도시까지 가는데 드는 최소 비용을 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    static int[][] arr;
    static int INF = 100_000_001;
    static int[] dist;
    static boolean[] visited;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int V = Integer.parseInt(br.readLine());
        int E = Integer.parseInt(br.readLine());

        arr = new int[V+1][V+1];

        for(int i=1;i<=V;i++){
            Arrays.fill(arr[i],INF);
        }

        for(int i=0;i<E;i++){
            String[] s = br.readLine().split(" ");
            int a = Integer.parseInt(s[0]);
            int b = Integer.parseInt(s[1]);
            int c = Integer.parseInt(s[2]);
            arr[a][b] = Math.min(arr[a][b],c);
        }

        String[] s = br.readLine().split(" ");
        int ar = Integer.parseInt(s[0]);
        int dt = Integer.parseInt(s[1]);
        dijkstra(V,ar);

        bw.write(String.valueOf(dist[dt]) + "\n");
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

    public static void dijkstra(int v, int start){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        dist = new int[v+1];
        visited = new boolean[v+1];
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
    }
}

```
### **문제 풀이**
- 최단 거리 알고리즘인 다익스트라를 사용하는 문제이다.

- 버스의 비용이 0원일 수 있기 때문에 버스 비용을 저장하는 이차원 배열 `arr`을 INF로 모두 초기화한다.

- 그리고 입력된 값을 `arr`에 저장할 때 같은 노선의 버스가 여러대 들어올 수 있기에, 최소 비용만 `arr`에 저장한다.


### **[출처]**
https://www.acmicpc.net/problem/1916