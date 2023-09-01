## **행성 터널**


### ***problem***
때는 2040년, 이민혁은 우주에 자신만의 왕국을 만들었다. 왕국은 N개의 행성으로 이루어져 있다. 민혁이는 이 행성을 효율적으로 지배하기 위해서 행성을 연결하는 터널을 만들려고 한다.

행성은 3차원 좌표위의 한 점으로 생각하면 된다. 두 행성 A(xA, yA, zA)와 B(xB, yB, zB)를 터널로 연결할 때 드는 비용은 min(|xA-xB|, |yA-yB|, |zA-zB|)이다.

민혁이는 터널을 총 N-1개 건설해서 모든 행성이 서로 연결되게 하려고 한다. 이때, 모든 행성을 터널로 연결하는데 필요한 최소 비용을 구하는 프로그램을 작성하시오.

#### **Input**
첫째 줄에 행성의 개수 N이 주어진다. (1 ≤ N ≤ 100,000) 다음 N개 줄에는 각 행성의 x, y, z좌표가 주어진다. 좌표는 -109보다 크거나 같고, 109보다 작거나 같은 정수이다. 한 위치에 행성이 두 개 이상 있는 경우는 없다. 

#### **Output**
첫째 줄에 모든 행성을 터널로 연결하는데 필요한 최소 비용을 출력한다.

### ***Solution***
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
    static int[] parent = new int[100_001];
    static List<Edge> edges = new ArrayList<>();

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int N = Integer.parseInt(br.readLine());

        List<int[]> point = new ArrayList<>();

        for(int i=0;i<N;i++){
            StringTokenizer st = new StringTokenizer(br.readLine()," ");
            int x = Integer.parseInt(st.nextToken());
            int y = Integer.parseInt(st.nextToken());
            int z = Integer.parseInt(st.nextToken());
            point.add(new int[]{i,x,y,z});
        }

        for(int i=1;i<=3;i++){
            int v = i;
            Collections.sort(point, (o1,o2) -> Integer.compare(o1[v],o2[v]));
            for(int j=0;j<N-1;j++){
                int[] p1 = point.get(j);
                int[] p2 = point.get(j+1);
                edges.add(new Edge(p1[0],p2[0], Math.abs(p1[v]-p2[v])));
            }
        }

        Collections.sort(edges);

        for(int i=0;i<N;i++){
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
- 모든 행성을 연결하는 터널을 짓는 최소비용을 구하는 MST문제이다.

- 지금까지 좌표가 주어지는 경우 이중 반복문을 통해서 2개의 노드간의 거리를 모두 구하는 방식으로 문제를 해결하였는데, 이 문제에서는 이중 반복문을 사용하여 문제를 풀게되면 간선의 수가 너무 많아져 메모리 초과가 발생한다.

- 그래서 각각 x,y,z별로 정렬을 하여 인접한 좌표끼리만 간선을 만들어 MST를 구해야한다.


### **[출처]**
https://www.acmicpc.net/problem/2887