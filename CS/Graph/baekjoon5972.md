## **택배 배송**


### ***problem***
농부 현서는 농부 찬홍이에게 택배를 배달해줘야 합니다. 그리고 지금, 갈 준비를 하고 있습니다. 평화롭게 가려면 가는 길에 만나는 모든 소들에게 맛있는 여물을 줘야 합니다. 물론 현서는 구두쇠라서 최소한의 소들을 만나면서 지나가고 싶습니다.

농부 현서에게는 지도가 있습니다. N (1 <= N <= 50,000) 개의 헛간과, 소들의 길인 M (1 <= M <= 50,000) 개의 양방향 길이 그려져 있고, 각각의 길은 C_i (0 <= C_i <= 1,000) 마리의 소가 있습니다. 소들의 길은 두 개의 떨어진 헛간인 A_i 와 B_i (1 <= A_i <= N; 1 <= B_i <= N; A_i != B_i)를 잇습니다. 두 개의 헛간은 하나 이상의 길로 연결되어 있을 수도 있습니다. 농부 현서는 헛간 1에 있고 농부 찬홍이는 헛간 N에 있습니다.

다음 지도를 참고하세요.


           [2]---
          / |    \
         /1 |     \ 6
        /   |      \
     [1]   0|    --[3]
        \   |   /     \2
        4\  |  /4      [6]
          \ | /       /1
           [4]-----[5] 
                3  

농부 현서가 선택할 수 있는 최선의 통로는 1 -> 2 -> 4 -> 5 -> 6 입니다. 왜냐하면 여물의 총합이 1 + 0 + 3 + 1 = 5 이기 때문입니다.

농부 현서의 지도가 주어지고, 지나가는 길에 소를 만나면 줘야할 여물의 비용이 주어질 때 최소 여물은 얼마일까요? 농부 현서는 가는 길의 길이는 고려하지 않습니다.

#### **Input**
첫째 줄에 N과 M이 공백을 사이에 두고 주어집니다.

둘째 줄부터 M+1번째 줄까지 세 개의 정수 A_i, B_i, C_i가 주어집니다.

#### **Output**
첫째 줄에 농부 현서가 가져가야 될 최소 여물을 출력합니다.

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

        public int getIndex(){
            return this.index;
        }

        public int getCost(){
            return this.cost;
        }

        @Override
        public int compareTo(Node e){
            return Integer.compare(this.cost, e.cost);
        }
    }
    static int INF =  Integer.MAX_VALUE;
    static List<Node>[] nodes;

    static int N;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        init(N);

        for(int i=0;i<M;i++){
            st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = Integer.parseInt(st.nextToken());

            nodes[a].add(new Node(b,c));
            nodes[b].add(new Node(a,c));
        }

        int result = dijkstra(N);

        bw.write(result+"\n");
        bw.flush();
    }

    private static void init(int N){
        nodes = new List[N+1];

        for(int i=1;i<=N;i++){
            nodes[i] = new ArrayList<>();
        }
    }

    private static int dijkstra(int N){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[N+1];
        boolean[] visited = new boolean[N+1];

        Arrays.fill(dist, INF);
        dist[1] = 0;

        pq.offer(new Node(1,0));

        while(!pq.isEmpty()){
            int index = pq.poll().getIndex();

            if(visited[index]) continue;
            visited[index] = true;

            for(Node n : nodes[index]){
                int nextCost = dist[index] + n.cost;
                if(!visited[n.index] && dist[n.index] > nextCost){
                    dist[n.index] = nextCost;
                    pq.offer(new Node(n.index,dist[n.index]));
                }
            }
        }
        return dist[N];
    }
}
```
### **문제 풀이**
- 기본적인 최단거리 문제이다. 비교하는 값을 거리가 아닌 소가 몇마리인지 계산하면된다.
- start지점에서 end지점까지 최단거리를 구하는 문제이기 때문에 다익스트라 알고리즘을 활용하였다.

### **[출처]**
https://www.acmicpc.net/problem/5972