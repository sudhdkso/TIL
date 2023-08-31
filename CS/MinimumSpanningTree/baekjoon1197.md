## **최소 스패닝 트리**


### ***problem***
그래프가 주어졌을 때, 그 그래프의 최소 스패닝 트리를 구하는 프로그램을 작성하시오.

최소 스패닝 트리는, 주어진 그래프의 모든 정점들을 연결하는 부분 그래프 중에서 그 가중치의 합이 최소인 트리를 말한다.

#### **Input**
첫째 줄에 정점의 개수 V(1 ≤ V ≤ 10,000)와 간선의 개수 E(1 ≤ E ≤ 100,000)가 주어진다. 다음 E개의 줄에는 각 간선에 대한 정보를 나타내는 세 정수 A, B, C가 주어진다. 이는 A번 정점과 B번 정점이 가중치 C인 간선으로 연결되어 있다는 의미이다. C는 음수일 수도 있으며, 절댓값이 1,000,000을 넘지 않는다.

그래프의 정점은 1번부터 V번까지 번호가 매겨져 있고, 임의의 두 정점 사이에 경로가 있다. 최소 스패닝 트리의 가중치가 -2,147,483,648보다 크거나 같고, 2,147,483,647보다 작거나 같은 데이터만 입력으로 주어진다.

#### **Output**
첫째 줄에 최소 스패닝 트리의 가중치를 출력한다.

### ***Solution***
#### 프림 알고리즘
``` java
import java.io.*;
import java.util.*;

public class Main {

    private static class Node implements Comparable<Node>{
        int index, cost;
        public Node(int index, int cost){
            this.index = index;
            this.cost = cost;
        }

        @Override
        public int compareTo(Node e){
            return Integer.compare(this.cost, e.cost);
        }
    }
    static int[] dist;
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

        for(int i=0;i<E;i++){
            st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = Integer.parseInt(st.nextToken());
            list[a].add(new Node(b,c));
            list[b].add(new Node(a,c));
        }

        bw.write(String.valueOf(prim(V)));
        bw.flush();
    }

    private static int prim(int v){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[v+1];
        boolean[] visited = new boolean[v+1];

        Arrays.fill(dist, INF);
        dist[1] = 0;
        pq.offer(new Node(1,0));

        int count = 0, sum = 0;
        while(!pq.isEmpty()){
            Node now = pq.poll();
            if(visited[now.index]) continue;
            visited[now.index] = true;

            sum += now.cost;
            count++;
            if(count == v){
                break;
            }

            for(Node n : list[now.index]){
                if(!visited[n.index] && dist[n.index] > n.cost){
                    dist[n.index] = n.cost;
                    pq.offer(new Node(n.index, dist[n.index]));
                }
            }

        }
        return sum;
    }
}
```
#### 크루스칼 알고리즘
``` java
import java.io.*;
import java.util.*;

public class Main {

    private static class Edge implements Comparable<Edge>{
        int nodeA,nodeB,cost;

        public Edge(int nodeA,int nodeB, int cost){
            this.nodeA = nodeA;
            this.nodeB = nodeB;
            this.cost = cost;
        }

        @Override
        public int compareTo(Edge e){
            return Integer.compare(this.cost, e.cost);
        }
    }
    static int[] parent = new int[10001];
    static List<Edge> edges = new ArrayList<>();

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int V = Integer.parseInt(st.nextToken());
        int E = Integer.parseInt(st.nextToken());


        for(int i=0;i<E;i++){
            st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = Integer.parseInt(st.nextToken());
            edges.add(new Edge(a,b,c));
        }

        Collections.sort(edges);

        for(int i=1;i<=V;i++){
            parent[i] = i;
        }

        bw.write(String.valueOf(kruskal(V)));
        bw.flush();
    }

    private static int kruskal(int v){
        int sum = 0;
        for(Edge e : edges){
            int a = e.nodeA;
            int b = e.nodeB;
            int c = e.cost;

            if(findParent(a) != findParent(b)){
                union(a,b);
                sum += c;
            }
        }

        return sum;
    }

    private static int findParent(int x){
        if(parent[x] == x){
            return x;
        }
        return parent[x] = findParent(parent[x]);
    }

    private static void union(int a, int b){
        a = findParent(a);
        b = findParent(b);
        if(a < b){
            parent[b] = a;
        }
        else{
            parent[a] = b;
        }
    }
}
```
### **문제 풀이**


### **[출처]**
https://www.acmicpc.net/problem/1197