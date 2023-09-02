## **물대기**


### ***problem***
선주는 자신이 운영하는 N개의 논에 물을 대려고 한다. 물을 대는 방법은 두 가지가 있는데 하나는 직접 논에 우물을 파는 것이고 다른 하나는 이미 물을 대고 있는 다른 논으로부터 물을 끌어오는 법이다.

각각의 논에 대해 우물을 파는 비용과 논들 사이에 물을 끌어오는 비용들이 주어졌을 때 최소의 비용으로 모든 논에 물을 대는 것이 문제이다.

#### **Input**
첫 줄에는 논의 수 N(1 ≤ N ≤ 300)이 주어진다. 다음 N개의 줄에는 i번째 논에 우물을 팔 때 드는 비용 Wi(1 ≤ Wi ≤ 100,000)가 순서대로 들어온다. 다음 N개의 줄에 대해서는 각 줄에 N개의 수가 들어오는데 이는 i번째 논과 j번째 논을 연결하는데 드는 비용 Pi,j(1 ≤ Pi,j ≤ 100,000, Pi,j = Pj,i, Pi,i = 0)를 의미한다.

#### **Output**
첫 줄에 모든 논에 물을 대는데 필요한 최소비용을 출력한다.

### ***Solution***
#### 프림 알고리즘
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

    static int[] water;

    static int INF = Integer.MAX_VALUE;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int N = Integer.parseInt(br.readLine());
        nodes = new List[N+1];

        for(int i=0;i<=N;i++){
            nodes[i] = new ArrayList<>();
        }

        for(int i=1;i<=N;i++){
            int cost = Integer.parseInt(br.readLine());
            nodes[0].add(new Node(i,cost));
        }

        for(int i=1;i<=N;i++){
            StringTokenizer st = new StringTokenizer(br.readLine()," ");
            for(int j=1;j<=N;j++){
                int cost = Integer.parseInt(st.nextToken());
                nodes[i].add(new Node(j,cost));
            }
        }

        bw.write(String.valueOf(prim(N)));
        bw.flush();
    }

    private static int prim(int v){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[v+1];
        boolean[] visited = new boolean[v+1];

        Arrays.fill(dist, INF);
        dist[0] = 0;
        pq.offer(new Node(0,0));

        int count = 0, sum = 0;
        while(!pq.isEmpty()){
            Node now = pq.poll();
            if(visited[now.index]) continue;
            visited[now.index] = true;

            sum += now.cost;
            count++;
            if(count == v+1){
                break;
            }

            for(Node n : nodes[now.index]){
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
    static int[] parent = new int[301];
    static List<Edge> edges = new ArrayList<>();
    
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int N = Integer.parseInt(br.readLine());

        for(int i=1;i<=N;i++){
            int cost = Integer.parseInt(br.readLine());
            edges.add(new Edge(0, i, cost));
        }

        for(int i=1;i<=N;i++){
            StringTokenizer st = new StringTokenizer(br.readLine()," ");
            for(int j=1;j<=N;j++){
                int cost = Integer.parseInt(st.nextToken());
                edges.add(new Edge(i,j,cost));
            }
        }

        Collections.sort(edges);

        for(int i=1;i<=N;i++){
            parent[i] = i;
        }

        bw.write(String.valueOf(kruskal()));
        bw.flush();
    }

    private static int kruskal(){
        int sum = 0;
        for(Edge e: edges){
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
        if(x == parent[x]){
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
https://www.acmicpc.net/problem/1368