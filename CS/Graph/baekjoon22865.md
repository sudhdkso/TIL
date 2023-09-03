## *가장 먼 곳**


### ***problem***
 
$N$개의 땅 중에서 한 곳에 자취를 하려고 집을 알아보고 있다. 세 명의 친구 
$A$, 
$B$, 
$C$가 있는데 이 친구들이 살고 있는 집으로부터 가장 먼 곳에 집을 구하려고 한다.

이때, 가장 먼 곳은 선택할 집에서 거리가 가장 가까운 친구의 집까지의 거리를 기준으로 거리가 가장 먼 곳을 의미한다.

예를 들어, 
$X$ 위치에 있는 집에서 친구 
$A$, 
$B$, 
$C$의 집까지의 거리가 각각 3, 5, 4이라 가정하고 
$Y$ 위치에 있는 집에서 친구 
$A$, 
$B$, 
$C$의 집까지의 거리가 각각 5, 7, 2라고 하자.

이때, 친구들의 집으로부터 땅 
$X$와 땅 
$Y$ 중 더 먼 곳은 땅 
$X$이다. 왜냐하면 
$X$에서 가장 가까운 친구의 집까지의 거리는 3이고, 
$Y$에서는 
$2$이기 때문이다.

친구들이 살고 있는 집으로부터 가장 먼 곳을 구해보자.

#### **Input**
첫번째 줄에 자취할 땅 후보의 개수 
$N$이 주어진다.

2번째 줄에는 친구 
$A$, 
$B$, 
$C$가 사는 위치가 공백으로 구분되어 주어진다. 이때 친구들은 
$N$개의 땅 중 하나에 사는 것이 보장된다. (같은 위치에서 살 수 있다.)

3번째 줄에는 땅과 땅 사이를 잇는 도로의 개수 
$M$이 주어진다.

그 다음줄부터 
$M + 3$번째 줄까지 땅 
$D$, 땅 
$E$, 땅 
$D$와 땅 
$E$와 사이를 연결하는 도로의 길이 
$L$이 공백으로 구분되어 주어진다. 이 도로는 양뱡항 통행이 가능하다.

#### **Output**
친구들이 살고 있는 집으로부터 가장 먼 곳의 땅 번호를 출력한다. 만약 가장 먼 곳이 여러 곳이라면 번호가 가장 작은 땅의 번호를 출력한다.

#### 제한

 
- $ 1 ≤ N ≤ 100,000$ 
 
- $N - 1 ≤ M ≤ 500,000$ 
 
- $1 ≤ A, B, C, D, E ≤ N$ 
 
- $1 ≤ L ≤ 10,000$ 
 
- $L$은 정수
- 땅의 번호는 
$1$부터 
$L$까지 하나씩 붙어 있다.
- 임의의 두 땅 사이를 도로를 통해서 이동할 수 있다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

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
    static List<Node>[] nodes;

    static int INF = Integer.MAX_VALUE;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int N = Integer.parseInt(br.readLine());

        nodes = new List[N+1];

        for(int i=1;i<=N;i++){
            nodes[i] = new ArrayList<>();
        }

        StringTokenizer st = new StringTokenizer(br.readLine()," ");

        int[] friend = new int[3];

        for(int i=0;i<3;i++){
            friend[i] = Integer.parseInt(st.nextToken());
        }

        int M = Integer.parseInt(br.readLine());

        for(int i=0;i<M;i++){
            st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = Integer.parseInt(st.nextToken());
            nodes[a].add(new Node(b,c));
            nodes[b].add(new Node(a,c));
        }

        int[][] dist = new int[3][N+1];
        for(int i=0;i<3;i++){
            dist[i] = dijkstra(friend[i], N);
        }

        int max = 0, index = 0;
        for(int i=1;i<=N;i++){
            int ad = dist[0][i];
            int bd = dist[1][i];
            int cd = dist[2][i];

            int min = Math.min(ad,Math.min(bd,cd));

            if(min > max){
                max = min;
                index = i;
            }
        }

        bw.write(String.valueOf(index));
        bw.flush();
    }

    private static int[] dijkstra(int start, int N){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[N+1];
        boolean[] visited = new boolean[N+1];
        Arrays.fill(dist, INF);

        dist[start] = 0;
        pq.offer(new Node(start, 0));

        while(!pq.isEmpty()){
            Node now = pq.poll();
            if(visited[now.index]) continue;
            visited[now.index] = true;

            for(Node n : nodes[now.index]){
                if(!visited[n.index] && dist[n.index] > dist[now.index] + n.cost){
                    dist[n.index] = dist[now.index] + n.cost;
                    pq.offer(new Node(n.index, dist[n.index]));
                }
            }

        }

        return dist;
    }

}
```
### **문제 풀이**



### **[출처]**
https://www.acmicpc.net/problem/22865