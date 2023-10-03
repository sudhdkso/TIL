## **안정적인 네트워크**


### ***problem***
한 회사는 본사와 지사의 컴퓨터들을 연결하는 네트워크 시설을 보유하고 있다. 각 지사에는 네트워크용 컴퓨터가 한 대씩 있으며, 이들은 본사의 메인 컴퓨터와 직접 연결되어 있다. 몇몇 지사들끼리 연결되어 있는 경우도 있다.

네트워크 시설에서는 두 컴퓨터가 직접 연결되어 있지 않더라도 다른 컴퓨터들을 경유하여 연결될 수 있다. 예를 들어 1, 2번 컴퓨터가 직접 연결되어 있고, 1, 3번 컴퓨터가 직접 연결되어 있다면, 이것은 2, 3번 컴퓨터가 연결되어 있는 효과도 발휘한다는 것이다.

회사 측에서는 네트워크에 고장이 발생하더라도 컴퓨터들이 연결되어 있도록 안정적인 네트워크를 구축하고자 한다. 네트워크에 고장이 발생하는 경우는 두 가지가 있다. 첫 번째는 직접 연결되어 있는 두 컴퓨터의 연결이 끊어지는 경우이다. 회사 측은 이런 경우에도 이 두 컴퓨터가 다른 컴퓨터들을 경유하여 연결되어 있기를 원한다. 두 번째는 컴퓨터가 고장 나는 경우이다. 회사 측은 이런 경우에는 고장 나지 않은 컴퓨터들끼리 연결되어 있기를 원한다.

예제로 주어진 입력에서 1, 2번 컴퓨터의 연결이 끊어지더라도, 이 두 컴퓨터는 3번 컴퓨터를 통해서 연결되게 된다. 하지만 1번 컴퓨터가 고장 나는 경우에는 5번 컴퓨터가 다른 컴퓨터들과 연결되어 있지 못하게 된다. 따라서 5번 컴퓨터를 다른 컴퓨터와 직접 연결해 주어야 한다.

두 컴퓨터를 연결하는 데 소요되는 비용은 일정하지 않다. 당신은 네트워크의 연결 상태를 입력받아 이 네트워크가 안정적인 네트워크인지 판별하고, 만약 아닐 경우에는 최소 비용으로 회사의 네트워크가 안정적이 되도록 하여야 한다.

#### **Input**
첫째 줄에 두 정수 n(1 ≤ n ≤ 1,000), m(0 ≤ m ≤ 10,000)이 주어진다. n은 컴퓨터의 개수이며, m은 연결되어 있는 지사 컴퓨터들의 쌍의 개수이다. 다음 m개의 줄에는 두 정수 x, y가 주어진다. 이는 서로 다른 두 컴퓨터, x번 컴퓨터와 y번 컴퓨터가 직접 연결되어 있음을 의미한다. 다음 n개의 줄에는 인접 행렬의 형태로 두 컴퓨터를 연결할 때 드는 비용이 주어진다. 이 값은 1 이상 30,000이하의 정수이며, i번 컴퓨터와 j번 컴퓨터를 연결할 때 드는 비용은 j번 컴퓨터와 i번 컴퓨터를 연결할 때 드는 비용과 같다. 본사의 컴퓨터는 항상 1번 컴퓨터이다.

#### **Output**
첫째 줄에 최소 비용 X와 연결할 컴퓨터들의 쌍의 개수 K를 출력한다. 다음 K개의 줄에는 두 정수로 연결할 컴퓨터들의 번호를 출력한다. 만약 주어진 입력이 안정적인 네트워크라면 X=0이고 K=0이 된다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    static class Edge implements Comparable<Edge> {
        int nodeA,nodeB, cost;

        public Edge(int nodeA, int nodeB, int cost) {
            this.nodeA = nodeA;
            this.nodeB = nodeB;
            this.cost = cost;
        }

        @Override
        public int compareTo(Edge e) {
            return Integer.compare(this.cost, e.cost);
        }
    }

    static int INF = Integer.MAX_VALUE;
    static List<Edge> edges = new ArrayList<>();
    static int N;

    static int[] parent = new int[1001];
    static StringBuilder sb = new StringBuilder();
    static int sum = 0, count = 0;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        for(int i=0;i<M;i++){
            st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());

            edges.add(new Edge(a,b,0));
        }

        for(int i=0;i<N;i++){
            String[] s = br.readLine().split(" ");
            for(int j=i+1;j<N;j++){
                int c = Integer.parseInt(s[j]);

                edges.add(new Edge(i+1,j+1,c));
            }
        }

        for(int i=1;i<=N;i++){
            parent[i] = i;
        }
        kruskal();
        bw.write(sum+" "+count+"\n");
        bw.write(sb.toString());

        bw.flush();
    }
    private static int findParent(int x){
        if( x == parent[x]){
            return x;
        }
        return parent[x] = findParent(parent[x]);
    }
    private static void union(int a, int b){
        a = findParent(a);
        b = findParent(b);
        if(b > a){
            parent[b] = a;
        }
        else{
            parent[a] = b;
        }
    }

    private static void kruskal(){

        Collections.sort(edges);

        for(Edge e : edges){
            int a = e.nodeA;
            int b = e.nodeB;
            int c = e.cost;
            if(a == 1 || b == 1) continue;
            if(findParent(a) != findParent(b)){
                union(a,b);
                sum += c;
                if(c > 0){
                    count++;
                    sb.append(a+" "+b).append("\n");
                }
            }
        }
    }
}
```
### **문제 풀이**
- 이 문제는 모든 네트워크를 연결하는 최소 비용을 구하는 최소 스패닝 트리 문제이다.

- 문제에서는 이미 연결되어 있는 네트워크들과, 각 네트워크 간 연결하는 비용에 대한 행렬이 차례로 주어진다.
    - 이때 이미 연결되어있는 네트워크의 경우 비용을 0으로 edges에 추가하고, 입력받은 행렬의 경우 `(i,j)`에 맞게 비용 c로 edges에 추가한다.

- 그리고 이 edges를 이용하여 크루스칼 알고리즘을 실행시킨다.
    - 이때 문제에서 모든 네트워크를 연결하는 최소비용과 연결해야하는 네트워크의 갯수, 그리고 연결하는 네트워크에 대한 정보들을 출력해야한다.
    - 그래서 일반 크루스칼 알고리즘과 동일하게 최소비용은 계산한다.
    - 그리고 연결하는 네트워크의 갯수와 네트워크 정보의 경우 edges에 비용이 0인 간선들로 연결되지 않고 존재하기 때문에 이 간선들이 연결되는 경우는 연결하는 네트워크의 갯수와 정보에 추가해서는 안되기 때문에 따로 처리를 해줘야한다.

- 그리고 이렇게 크루스칼 알고리즘을 적용한 이후에 나온 최소 비용, 연결해야하는 네트워크의 갯수, 정보를 차례대로 출력한다.
 
### **[출처]**
https://www.acmicpc.net/problem/2406