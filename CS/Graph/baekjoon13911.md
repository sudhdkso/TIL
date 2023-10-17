## **도로포장**


### ***problem***
안양에 사는 상혁이는 4년간의 통학에 지쳐 서울에 집을 구하려고 한다. 상혁이가 원하는 집은 세가지 조건이 있다.

- 맥세권 : 맥세권인 집은 맥도날드와 집 사이의 최단거리가 x이하인 집이다.
- 스세권 : 스세권인 집은 스타벅스와 집 사이의 최단거리가 y이하인 집이다.
- 맥세권과 스세권을 만족하는 집 중 최단거리의 합이 최소인 집

통학 때문에 스트레스를 많이 받은 상혁이는 집을 선택하는 데 어려움을 겪고 있다. 똑똑한 여러분이 상혁이 대신 이 문제를 해결해 주자. 이사 갈 지역의 지도가 그래프로 주어지고 맥도날드와 스타벅스의 위치가 정점 번호로 주어질 때 상혁이가 원하는 집의 최단거리의 합을 출력하는 프로그램을 작성하시오. (맥도날드와 스타벅스가 아닌 정점에는 모두 집이 있다.)

<p align = "center">
    <img src="../images\baekjoon13911.png" width ="500px">
</p>

위의 예제 지도에서 사각형은 맥도날드를, 별은 스타벅스가 위치한 정점을 나타낸다. 각 원은 집이 있는 정점을 낸다. x가 6이고 y가 4일 때 가능한 집의 정점은 6이다. 맥도날드까지의 최단거리가 2, 스타벅스까지의 최단거리가 4로 총 합이 6이 되기 때문이다. 정점 7은 맥세권이면서 스세권이지만 맥도날드까지의 최단거리가 6, 스타벅스까지의 최단거리가 2로 총 합이 8로써 정점 6의 값보다 크므로 답이 아니다. 그 외의 정점 2, 3, 4는 맥세권이면서 스세권인 조건을 충족하지 못하므로 답이 될 수 없다.

#### **Input**
첫줄에는 정점의 개수 V(3 ≤ V ≤ 10,000)와 도로의 개수 E(0 ≤ E ≤ 300,000)가 주어진다. 그 다음 E줄에 걸쳐 각 도로를 나타내는 세 개의 정수 (u,v,w)가 순서대로 주어진다. 이는 u와 v(1 ≤ u,v ≤ V)사이에 가중치가 w(1 ≤ w < 10,000)인 도로가 존재한다는 뜻이다. u와 v는 서로 다르며 다른 두 정점 사이에는 여러 개의 간선이 존재할 수도 있음에 유의한다. E+2번째 줄에는 맥도날드의 수 M(1 ≤ M ≤ V-2) 맥세권일 조건 x(1 ≤ x ≤ 100,000,000)가 주어지고 그 다음 줄에 M개의 맥도날드 정점 번호가 주어진다. E+4번째 줄에는 스타벅스의 수 S(1 ≤ S ≤ V-2)와 스세권일 조건 y(1 ≤ y ≤ 100,000,000)가 주어지고 그 다음 줄에 S개의 스타벅스 정점 번호가 주어진다. 

- 맥도날드나 스타벅스가 위치한 정점에는 집이 없다.
- 한 정점에 맥도날드와 스타벅스가 같이 위치할 수 있다.
- 집이 있는(= 맥도날드나 스타벅스가 위치하지 않은) 정점이 하나 이상 존재한다.

#### **Output**
 상혁이가 원하는 집의 맥도날드까지의 최단거리와 스타벅스까지의 최단거리 합을 출력한다. 만일 원하는 집이 존재하지 않으면 -1을 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    public static class Node implements Comparable<Node>{
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

    static List<Node>[] list;
    static int INF = Integer.MAX_VALUE;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");

        int V = Integer.parseInt(st.nextToken());
        int E = Integer.parseInt(st.nextToken());

        list = new List[V+1];

        for(int i=1;i<=V;i++){
            list[i] = new ArrayList<>();
        }

        while(E-- > 0){
            st = new StringTokenizer(br.readLine()," ");
            int u = Integer.parseInt(st.nextToken());
            int v = Integer.parseInt(st.nextToken());
            int w = Integer.parseInt(st.nextToken());

            list[u].add(new Node(v,w));
            list[v].add(new Node(u,w));
        }

        st = new StringTokenizer(br.readLine()," ");

        int M = Integer.parseInt(st.nextToken());
        int x = Integer.parseInt(st.nextToken());

        st = new StringTokenizer(br.readLine()," ");
        int[] mac = new int[M];
        for(int i=0;i<M;i++){
            mac[i] = Integer.parseInt(st.nextToken());
        }

        int[] result1 = dijkstra(V,mac);
        st = new StringTokenizer(br.readLine()," ");

        int S = Integer.parseInt(st.nextToken());
        int y = Integer.parseInt(st.nextToken());

        st = new StringTokenizer(br.readLine()," ");
        int[] sta = new int[S];
        for(int i=0;i<S;i++){
            sta[i] = Integer.parseInt(st.nextToken());
        }

        int[] result2 = dijkstra(V,sta);
        int answer = INF;
        for(int i=1;i<=V;i++){
            if(result1[i] == 0 || result2[i] == 0) continue;
            if(result1[i] <= x && result2[i] <= y ){
                answer = Math.min(answer, result1[i]+result2[i]);
            }
        }
        
        bw.write((answer == INF ? -1 : answer)+"\n");
        bw.flush();
    }


    private static int[] dijkstra(int N, int[] arr){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[N+1];
        boolean[] visited = new boolean[N+1];
        Arrays.fill(dist, INF);

        for(int i=0;i<arr.length;i++){
            int n = arr[i];
            dist[n] = 0;
            pq.offer(new Node(n,0));
        }

        while(!pq.isEmpty()){
            Node now = pq.poll();
            int index = now.index;

            if(visited[index] || now.cost> dist[index]) continue;
            visited[index] = true;

            for(Node next : list[index]){
                if(!visited[next.index] && dist[next.index] > dist[index] + next.cost){
                    dist[next.index] = dist[index] + next.cost;
                    pq.offer(new Node(next.index, dist[next.index]));
                }
            }
        }

        return dist;
    }
}
```
### **문제 풀이**


### **[출처]**
https://www.acmicpc.net/problem/13911