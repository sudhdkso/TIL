## **세금**


### ***problem***
주언이는 경제학을 배워 행상인이 되었다. 두 도시를 오가며 장사를 하는데, 통행료의 합이 가장 적은 경로로 이동하려 한다. 도시들은 양방향 도로로 연결되어있으며, 도로마다 통행료가 존재한다.

그런데 정부는 세금 인상안을 발표하였다. 세금을 한 번에 올리면 문제가 발생할 수 있으므로 여러 단계에 걸쳐서 올린다고 한다. 세금이 A만큼 오르면 모든 도로의 통행료가 각각 A만큼 오르게 된다. 세금이 오르게 되면 주언이가 내야 하는 통행료가 변할 수 있다.

주언이를 도와 초기의 최소 통행료와 세금이 오를 때마다의 최소 통행료를 구하시오.

#### **Input**
첫 번째 줄에 세 정수 N (2 ≤ N ≤ 1,000), M (1 ≤ M ≤ 30,000), K (0 ≤ K ≤ 30,000)가 주어진다. 각각 도시의 수, 도로의 수, 세금 인상 횟수를 의미한다.

두 번째 줄에는 두 정수 S와 D (1 ≤ S, D ≤ N, S ≠ D)가 주어진다. 각각 출발 도시와 도착 도시 번호를 의미한다. 도시 번호는 1부터 시작한다.

다음 M개의 줄에는 각각 도로 정보를 나타내는 세 정수 a, b (1 ≤ a < b ≤ N), w (1 ≤ w ≤ 1,000)가 주어진다. 도시 a와 도시 b가 통행료 w인 도로로 연결되어 있다는 것을 의미한다.

다음 총 K개의 줄에는 각각 정수 p (1 ≤ p ≤ 10)가 주어진다. 각각 첫 번째, 두 번째, …, K 번째에 인상되는 세금을 의미한다.

S에서 D로 이동할 수 없는 경우는 주어지지 않는다.

#### **Output**
첫 번째 줄에 세금이 오르기 전의 최소 통행료를 출력한다.

두 번째 줄부터 K개의 줄에 각각 첫 번째, 두 번째, …, K 번째 세금이 올랐을 때의 최소 통행료를 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    static class Node implements Comparable<Node>{
        int index,cost, count;
        public Node(int index, int cost, int count){
            this.index = index;
            this.cost = cost;
            this.count = count;
        }

        public Node(int index, int cost){
            this(index, cost, 0);
        }

        @Override
        public int compareTo(Node e){
            return Integer.compare(this.cost, e.cost);
        }
    }
    static int INF =  Integer.MAX_VALUE;
    static List<Node>[] list;
    static  int N;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());

        list = new List[N+1];

        for(int i=1;i<=N;i++){
            list[i] = new ArrayList<>();
        }

        st = new StringTokenizer(br.readLine()," ");
        int s = Integer.parseInt(st.nextToken());
        int d = Integer.parseInt(st.nextToken());

        while(M-- > 0){
            st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = Integer.parseInt(st.nextToken());
            list[a].add(new Node(b,c));
            list[b].add(new Node(a,c));
        }
        
        int[][] result = dijkstra(s,d);
        int min = INF, index = 0;
        
        for(int i=N;i>0;i--){
            int num = result[d][i];
            if(min > num){
                min = num;
                index = i;
            }
        }
        bw.write(min+"\n");

        while(K-- > 0){
            int p = Integer.parseInt(br.readLine());
            min = INF;
            for(int i=index;i>0;i--){
                if(result[d][i] >= INF) continue;
                result[d][i] += (i*p);
                min = Math.min(min, result[d][i]);
            }
            bw.write(min +"\n");
        }

        bw.flush();
    }

    private static int[][] dijkstra(int s, int d){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[][] dist = new int[N+1][N+1];
        boolean[] visited = new boolean[N+1];

        for(int i=1;i<= N;i++){
            Arrays.fill(dist[i], INF);
        }

        dist[s][0] = 0;

        pq.offer(new Node(s,0));

        while(!pq.isEmpty()){
            Node now = pq.poll();
            int index = now.index;
            int count = now.count;
            if( count >= N || now.cost > dist[index][count]) continue;

            for(Node n : list[index]){
                int next = dist[index][count] + n.cost;
                if(dist[n.index][count+1] > next){
                    dist[n.index][count+1] = next;
                    pq.offer(new Node(n.index, dist[n.index][count+1],count+1));
                }
            }
        }

        return dist;
    }
}
```
### **문제 풀이**
- 이 문제는 s부터 d까지의 최단 요금을 구하는 최단 거리 문제이다.

- 문제에서 k횟수 동안 각 거리에 대한 모든 요금이 p만큼 인상이 된다. 매번 세금이 인상되는 것을 기준으로 최단 요금을 구하면 시간초과가 발생한다. 

- 따라서 이 문제는 한번의 다익스트라 알고리즘을 사용해 각 노드에 대한 최단 거리 비용이 몇개의 노드를 거쳐서 나오는 비용인지 함께 구한 후,`dist[d][1] ~ dist[d][N]`를 이용하여 세금이 오를 때마다 d를 방문할 수 있는 최소 비용이 얼마인지 구할 수 있다.
    - dist[d][i]은 s에서 d를 방문할 때 `i`개의 노드를 거쳐서 방문한 최단 거리를 의미한다.


 
### **[출처]**
https://www.acmicpc.net/problem/13907