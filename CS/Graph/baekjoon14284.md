## **간선 이어가기 2**


### **problem**
정점 n개, 0개의 간선으로 이루어진 무방향 그래프가 주어진다. 그리고 m개의 가중치 간선의 정보가 있는 간선리스트가 주어진다. 간선리스트에 있는 간선 하나씩 그래프에 추가해 나갈 것이다. 이때, 특정 정점 s와 t가 연결이 되는 시점에서 간선 추가를 멈출 것이다. 연결이란 두 정점이 간선을 통해 방문 가능한 것을 말한다.

s와 t가 연결이 되는 시점의 간선의 가중치의 합이 최소가 되게 추가하는 간선의 순서를 조정할 때, 그 최솟값을 구하시오.

#### **Input**
첫째 줄에 정점의 개수 n, 간선리스트의 간선 수 m이 주어진다.(2≤n≤5000,1≤m≤100,000)

다음 m줄에는 a,b,c가 주어지는데, 이는 a와 b는 c의 가중치를 가짐을 말한다. (1≤a,b≤n,1≤c≤100,a≠b)

다음 줄에는 두 정점 s,t가 주어진다. (1≤s,t≤n,s≠t)

모든 간선을 연결하면 그래프는 연결 그래프가 됨이 보장된다.

#### **Output**
s와 t가 연결되는 시점의 간선의 가중치 합의 최솟값을 출력하시오,

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
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
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

        st = new StringTokenizer(br.readLine()," ");
        int s = Integer.parseInt(st.nextToken());
        int e = Integer.parseInt(st.nextToken());


        bw.write(String.valueOf(dijkstra(N,s,e)));

        bw.flush();
    }

    private static int dijkstra(int N,int start, int end){
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
                    dist[n.index] = dist[now.index]+n.cost;
                    pq.offer(new Node(n.index, dist[n.index]));
                }
            }

        }

        return dist[end];
    }

}
```
### **문제 풀이**
- 간선을 추가하고 무방향그래프라고 하여, MST문제인가 생각했지만 모든 노드를 연결할 필요 없이 `s`에서 `t`까지의 최단거리를 구하는 문제이기 때문에 최단 거리 알고리즘을 사용하였다.

- 입력 받은대로 간선을 추가하며 최솟값을 찾아야하는 것 처럼 보이지만, 간선을 추가하는 순서는 임의로 변경할 수 있고, `s`에서 `t`로 최단거리가 나오면 종료하기 때문에 그냥 start를 `s`로 하는 다익스트라 알고리즘을 사용하여 문제를 해결하면 된다.




### **[출처]**
https://www.acmicpc.net/problem/14284