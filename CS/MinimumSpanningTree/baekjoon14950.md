## **정복자**


### ***problem***
서강 나라는 N개의 도시와 M개의 도로로 이루어졌다. 모든 도시의 쌍에는 그 도시를 연결하는 도로로 구성된 경로가 있다. 각 도로는 양방향 도로이며, 각 도로는 사용하는데 필요한 비용이 존재한다. 각각 도시는 1번부터 N번까지 번호가 붙여져 있다. 그 중에서 1번 도시의 군주 박건은 모든 도시를 정복하고 싶어한다.

처음 점거하고 있는 도시는 1번 도시 뿐이다. 만약 특정 도시 B를 정복하고 싶다면, B와 도로로 연결된 도시들 중에서 적어도 하나를 정복하고 있어야 한다. 조건을 만족하는 도시 중에서 하나인 A를 선택하면, B를 정복하는 과정에서 A와 B를 연결하는 도로의 비용이 소모된다. 박건은 한번에 하나의 도시만 정복을 시도하고 언제나 성공한다. 한 번 도시가 정복되면, 모든 도시는 경계를 하게 되기 때문에 모든 도로의 비용이 t만큼 증가하게 된다. 한 번 정복한 도시는 다시 정복하지 않는다.

이때 박건이 모든 도시를 정복하는데 사용되는 최소 비용을 구하시오.

#### **Input**
첫째 줄에 도시의 개수 N과 도로의 개수 M과 한번 정복할 때마다 증가하는 도로의 비용 t가 주어진다. N은 10000보다 작거나 같은 자연수이고, M은 30000보다 작거나 같은 자연수이다. t는 10이하의 자연수이다.

M개의 줄에는 도로를 나타내는 세 자연수 A, B, C가 주어진다. A와 B사이에 비용이 C인 도로가 있다는 뜻이다. A와 B는 N이하의 서로 다른 자연수이다. C는 10000 이하의 자연수이다.

#### **Output**
모든 도시를 정복하는데 사용되는 최소 비용을 출력하시오.

### ***Solution***
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
    private static List<Node>[] list;
    private static int INF = Integer.MAX_VALUE;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
        int C = Integer.parseInt(st.nextToken());

        list = new List[N+1];

        for(int i=1;i<=N;i++){
            list[i] = new ArrayList<>();
        }


        for(int i=0;i<M;i++){
            st = new StringTokenizer(br.readLine()," ");

            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = Integer.parseInt(st.nextToken());

            list[a].add(new Node(b,c));
            list[b].add(new Node(a,c));

        }

        int answer = prim(1, N, C);
        bw.write(String.valueOf(answer));
        bw.flush();

    }

    private static int prim(int start, int N, int incost){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[N+1];
        boolean[] visited = new boolean[N+1];

        Arrays.fill(dist, INF);
        dist[start] = 0;
        pq.offer(new Node(start, 0));

        int sum = 0 , count = 0;

        while(!pq.isEmpty()){
            Node now = pq.poll();
            if(visited[now.index]) continue;
            visited[now.index] = true;

            if(count > 0){
                sum+= now.cost + (incost*(count-1));
            }
            count++;

            if(count == N){
                break;
            }

            for(Node n : list[now.index]){
                if(!visited[n.index] && dist[n.index] > n.cost){
                    dist[n.index] = n.cost;
                    pq.offer(new Node(n.index, n.cost));
                }
            }
            
        }
        return sum;
    }

}
```
### **문제 풀이**
- 모든 도시를 정복하는 최소비용을 구하는 문제이기 때문에 MST를 구하는 알고리즘을 사용하여 문제를 해결할 수 있다.

- 처음 시작노드가 1번이기 때문에 한 노드씩 MST에 추가시키는 프림 알고리즘을 사용하였다.

- 정복한 도시의 갯수에 따라서 비용이 추가적으로 들지만, 각 비용은 합할 때만 계산하고 각 노드에 대한 최소 비용을 저장하는 dist에는 반영시킬 필요가 없다.

### **[출처]**
https://www.acmicpc.net/problem/14950