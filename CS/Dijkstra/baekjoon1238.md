## **파티**


### ***problem***
N개의 숫자로 구분된 각각의 마을에 한 명의 학생이 살고 있다.

어느 날 이 N명의 학생이 X (1 ≤ X ≤ N)번 마을에 모여서 파티를 벌이기로 했다. 이 마을 사이에는 총 M개의 단방향 도로들이 있고 i번째 길을 지나는데 Ti(1 ≤ Ti ≤ 100)의 시간을 소비한다.

각각의 학생들은 파티에 참석하기 위해 걸어가서 다시 그들의 마을로 돌아와야 한다. 하지만 이 학생들은 워낙 게을러서 최단 시간에 오고 가기를 원한다.

이 도로들은 단방향이기 때문에 아마 그들이 오고 가는 길이 다를지도 모른다. N명의 학생들 중 오고 가는데 가장 많은 시간을 소비하는 학생은 누구일지 구하여라.

#### **Input**
첫째 줄에 N(1 ≤ N ≤ 1,000), M(1 ≤ M ≤ 10,000), X가 공백으로 구분되어 입력된다. 두 번째 줄부터 M+1번째 줄까지 i번째 도로의 시작점, 끝점, 그리고 이 도로를 지나는데 필요한 소요시간 Ti가 들어온다. 시작점과 끝점이 같은 도로는 없으며, 시작점과 한 도시 A에서 다른 도시 B로 가는 도로의 개수는 최대 1개이다.

모든 학생들은 집에서 X에 갈수 있고, X에서 집으로 돌아올 수 있는 데이터만 입력으로 주어진다.

#### **Output**
첫 번째 줄에 N명의 학생들 중 오고 가는데 가장 오래 걸리는 학생의 소요시간을 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    static int[][] arr, dist;
    static int INF = Integer.MAX_VALUE;

    private static class Node implements Comparable<Node>{
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
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
        int X = Integer.parseInt(st.nextToken());

        arr = new int[N+1][N+1];
        dist = new int[N+1][N+1];

        for(int i=0;i<M;i++){
            String[] s = br.readLine().split(" ");
            int a = Integer.parseInt(s[0]);
            int b = Integer.parseInt(s[1]);
            int c = Integer.parseInt(s[2]);
            arr[a][b] = c;
        }

        for(int i=1;i<=N;i++){
            dijkstra(N, i);
        }
        int answer = 0;
        for(int i=1;i<=N;i++){
            answer = Math.max(dist[i][X]+dist[X][i] , answer);
        }
        bw.write(String.valueOf(answer) + "\n");
        bw.flush();
    }

    private static void dijkstra(int v, int start){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        Arrays.fill(dist[start], INF);
        dist[start][start] = 0;
        boolean[] visited = new boolean[v+1];
        pq.offer(new Node(start, 0));

        while(!pq.isEmpty()){
            int now = pq.poll().index;
            if(visited[now]) continue;
            visited[now] =true;
            for(int i=1;i<=v;i++){
                if(arr[now][i] == 0) continue;
                if(!visited[i] && dist[start][i] > dist[start][now] + arr[now][i]){
                    dist[start][i] = dist[start][now] + arr[now][i];
                    pq.offer(new Node(i, dist[start][i]));
                }
            }
        }
    }

}
```

### **문제 풀이**
- 최단 거리 알고리즘인 다익스트라를 활용하는 문제이다.
- 다익스트라는 한지점에서 모든 지점에 대한 최단거리를 구하는 문제이다. 그래서 이 문제에서는 플로이드-와샬을 활용해야할 것 처럼 보이지만 플로이드-와샬을 사용해서 문제를 해결하면 vertex의 갯수가 1000개이므로 시간 초과가 발생한다.

- 따라서 다익스트라에서 지점에 대한 거리를 나타내는 배열을 이차원 배열로 만들어 `1~N`까지의 모든 지점에서 모든 지점까지의 최단 거리를 구하여 문제를 해결해야한다.
- 2차원 배열을 만드는 이유는 단방향 노선에 대한 정보들이 들어와 (start-dest)에 대한 값과 (dest-start)의 값이 다를 수 있기 때문에 모든 지점을 한번에 구하여 계산을 편리하게 할 수 있도록 하기 위해서 이차원 배열을 활용하였다.


### **[출처]**
https://www.acmicpc.net/problem/1238