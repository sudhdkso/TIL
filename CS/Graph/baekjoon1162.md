## **도로포장**


### ***problem***
준영이는 매일 서울에서 포천까지 출퇴근을 한다. 하지만 잠이 많은 준영이는 늦잠을 자 포천에 늦게 도착하기 일쑤다. 돈이 많은 준영이는 고민 끝에 K개의 도로를 포장하여 서울에서 포천까지 가는 시간을 단축하려 한다.

문제는 N개의 도시가 주어지고 그 사이 도로와 이 도로를 통과할 때 걸리는 시간이 주어졌을 때 최소 시간이 걸리도록 하는 K개의 이하의 도로를 포장하는 것이다. 도로는 이미 있는 도로만 포장할 수 있고, 포장하게 되면 도로를 지나는데 걸리는 시간이 0이 된다. 또한 편의상 서울은 1번 도시, 포천은 N번 도시라 하고 1번에서 N번까지 항상 갈 수 있는 데이터만 주어진다.

#### **Input**
첫 줄에는 도시의 수 N(1 ≤ N ≤ 10,000)과 도로의 수 M(1 ≤ M ≤ 50,000)과 포장할 도로의 수 K(1 ≤ K ≤ 20)가 공백으로 구분되어 주어진다. M개의 줄에 대해 도로가 연결하는 두 도시와 도로를 통과하는데 걸리는 시간이 주어진다. 도로들은 양방향 도로이며, 걸리는 시간은 1,000,000보다 작거나 같은 자연수이다.

#### **Output**
첫 줄에 K개 이하의 도로를 포장하여 얻을 수 있는 최소 시간을 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    static class Node implements Comparable<Node>{
        int index, count;
        long cost;
        
        public Node(int index, long cost, int count){
            this.index = index;
            this.cost = cost;
            this.count = count;
        }

        public Node(int index, int cost){
            this(index,cost, 0);
        }

        @Override
        public int compareTo(Node e){
            return Long.compare(this.cost, e.cost);
        }
    }

    static long INF = Long.MAX_VALUE;
    static List<Node>[] nodes;
    static int N;
    
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());

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
        
        long answer = dijkstra(1,N,K);
        bw.write(answer+"\n");
        bw.flush();
    }

    private static long dijkstra(int start, int end, int k){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        long[][] dist = new long[N+1][k+1];

        for(int i=1;i<=N;i++){
            Arrays.fill(dist[i], INF);
        }

        dist[start][0] = 0;

        pq.offer(new Node(start, 0));

        while(!pq.isEmpty()){
            Node now = pq.poll();
            
            int index = now.index;
            long cost = now.cost;
            int count = now.count;
            
            if(cost > dist[index][count]) continue;

            for(Node n : nodes[index]){
                if(count < k && dist[n.index][count+1] > dist[index][count]){
                    dist[n.index][count+1] = dist[index][count];
                    pq.offer(new Node(n.index, dist[n.index][count+1], count+1));
                }
                if(dist[n.index][count] > dist[index][count] + n.cost){ //도로 포장을 하지 않는 경우
                    dist[n.index][count] = dist[index][count] + n.cost;
                    pq.offer(new Node(n.index, dist[n.index][count], count));

                }
            }
        }

        return Arrays.stream(dist[end]).min().getAsLong();
    }
}
```
### **문제 풀이**


### **[출처]**
https://www.acmicpc.net/problem/1162