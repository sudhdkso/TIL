## **우주신과의 교감**


### ***problem***
황선자씨는 우주신과 교감을 할수 있는 채널러 이다. 하지만 우주신은 하나만 있는 것이 아니기때문에 황선자 씨는 매번 여럿의 우주신과 교감하느라 힘이 든다. 이러던 와중에 새로운 우주신들이 황선자씨를 이용하게 되었다.

하지만 위대한 우주신들은 바로 황선자씨와 연결될 필요가 없다. 이미 황선자씨와 혹은 이미 우주신끼리 교감할 수 있는 우주신들이 있기 때문에 새로운 우주신들은 그 우주신들을 거쳐서 황선자 씨와 교감을 할 수 있다.

우주신들과의 교감은 우주신들과 황선자씨 혹은 우주신들 끼리 이어진 정신적인 통로를 통해 이루어 진다. 하지만 우주신들과 교감하는 것은 힘든 일이기 때문에 황선자씨는 이런 통로들이 긴 것을 좋아하지 않는다. 왜냐하면 통로들이 길 수록 더 힘이 들기 때문이다.

또한 우리들은 3차원 좌표계로 나타낼 수 있는 세상에 살고 있지만 우주신들과 황선자씨는 2차원 좌표계로 나타낼 수 있는 세상에 살고 있다. 통로들의 길이는 2차원 좌표계상의 거리와 같다.

이미 황선자씨와 연결된, 혹은 우주신들과 연결된 통로들이 존재한다. 우리는 황선자 씨를 도와 아직 연결이 되지 않은 우주신들을 연결해 드려야 한다. 새로 만들어야 할 정신적인 통로의 길이들이 합이 최소가 되게 통로를 만들어 “빵상”을 외칠수 있게 도와주자.

#### **Input**
첫째 줄에 우주신들의 개수 
$N$ (
$1 \le 1\,000$) 이미 연결된 신들과의 통로의 개수
$M$ (
$1 \le M \le 1\,000$)가 주어진다.

두 번째 줄부터 
$N$개의 줄에는 황선자를 포함하여 우주신들의 좌표가 
$X$, 
$Y$ (
$0 \le X, Y \le 1\,000\,000$)가 주어진다. 그 밑으로 
$M$개의 줄에는 이미 연결된 통로가 주어진다. 번호는 위의 입력받은 좌표들의 순서라고 생각하면 된다. 좌표는 정수이다.

#### **Output**
첫째 줄에 만들어야 할 최소의 통로 길이를 소수점 둘째 자리까지 반올림하여 출력하라.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    private static class Edge implements Comparable<Edge>{
        int nodeA;
        int nodeB;
        double cost;

        public Edge(int nodeA, int nodeB, double cost){
            this.nodeA = nodeA;
            this.nodeB = nodeB;
            this.cost = cost;
        }

        @Override
        public int compareTo(Edge e){
            return Double.compare(this.cost, e.cost);
        }
    }
    private static ArrayList<Edge> edges = new ArrayList<>();
    public static int[] parent = new int[1001];

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine()," ");

        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        int[][] coordi = new int[N][2];
        for(int i=0;i<N;i++){
            st = new StringTokenizer(br.readLine()," ");
            coordi[i][0] = Integer.parseInt(st.nextToken());
            coordi[i][1] = Integer.parseInt(st.nextToken());
        }

        for (int i = 0; i < N; i++) {
            parent[i] = i;
        }

        for(int i=0;i<N;i++){
            for(int j=i+1;j<N;j++){
                double dx = Math.pow(coordi[i][0]-coordi[j][0],2);
                double dy = Math.pow(coordi[i][1]-coordi[j][1],2);
                edges.add(new Edge(i,j,Math.sqrt(dx+dy)));
            }
        }

        for(int i=0;i<M;i++){
            st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken())-1;
            int b = Integer.parseInt(st.nextToken())-1;
            edges.add(new Edge(a,b,0.0));
        }
        Collections.sort(edges);

        double min = 0.0;
        for(Edge e : edges){
            double cost = e.cost;
            int a = e.nodeA;
            int b = e.nodeB;
            if(findParent(a) != findParent(b)){
                union(a,b);
                min += cost;
            }
        }

        bw.write(String.format("%.2f", min));
        bw.flush();

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
https://www.acmicpc.net/problem/1774