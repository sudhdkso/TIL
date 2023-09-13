## **최소비용 구하기 2**


### ***problem***
n(1≤n≤1,000)개의 도시가 있다. 그리고 한 도시에서 출발하여 다른 도시에 도착하는 m(1≤m≤100,000)개의 버스가 있다. 우리는 A번째 도시에서 B번째 도시까지 가는데 드는 버스 비용을 최소화 시키려고 한다. 그러면 A번째 도시에서 B번째 도시 까지 가는데 드는 최소비용과 경로를 출력하여라. 항상 시작점에서 도착점으로의 경로가 존재한다.

#### **Input**
첫째 줄에 도시의 개수 n(1≤n≤1,000)이 주어지고 둘째 줄에는 버스의 개수 m(1≤m≤100,000)이 주어진다. 그리고 셋째 줄부터 m+2줄까지 다음과 같은 버스의 정보가 주어진다. 먼저 처음에는 그 버스의 출발 도시의 번호가 주어진다. 그리고 그 다음에는 도착지의 도시 번호가 주어지고 또 그 버스 비용이 주어진다. 버스 비용은 0보다 크거나 같고, 100,000보다 작은 정수이다.

그리고 m+3째 줄에는 우리가 구하고자 하는 구간 출발점의 도시번호와 도착점의 도시번호가 주어진다.
#### **Output**
첫째 줄에 출발 도시에서 도착 도시까지 가는데 드는 최소 비용을 출력한다.

둘째 줄에는 그러한 최소 비용을 갖는 경로에 포함되어있는 도시의 개수를 출력한다. 출발 도시와 도착 도시도 포함한다.

셋째 줄에는 최소 비용을 갖는 경로를 방문하는 도시 순서대로 출력한다.

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
    static int INF = Integer.MAX_VALUE;
    static List<Node>[] list;
    public static void main(String[] args) throws IOException {
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.parseInt(br.readLine());
        int M = Integer.parseInt(br.readLine());

        list = new List[N+1];

        for(int i=1;i<=N;i++){
            list[i] = new ArrayList<>();
        }

        for(int i=0;i<M;i++){
            StringTokenizer st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = Integer.parseInt(st.nextToken());

            list[a].add(new Node(b,c));
        }
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int start = Integer.parseInt(st.nextToken());
        int end = Integer.parseInt(st.nextToken());
        int min = dijkstra(N, start , end);
        int count = 2;
        StringBuilder sb = new StringBuilder();
        int index = path[end];
        sb.append(end+" ");
        while(path[index] != 0){
            sb.append(index+" ");
            count++;
            index = path[index];
        }
        sb.append(start+" ");
        bw.write(String.valueOf(min)+"\n");
        bw.write(String.valueOf(count)+"\n");
        bw.write(reverse(sb.toString())+"\n");

        bw.flush();
    }

    private static String reverse(String str){
        String[] strings = str.split(" ");
        StringBuilder sb = new StringBuilder();
        for(int i=strings.length-1;i>=0;i--){
            sb.append(strings[i]+" ");
        }
        return sb.toString();
    }
    static int[] path = new int[1001];
    private static int dijkstra(int N, int start, int end){

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


            if(now.index == end){
                return dist[now.index];
            }

            for(Node n : list[now.index]){
                if(!visited[n.index] && dist[n.index] > dist[now.index]+n.cost){
                    dist[n.index] = dist[now.index]+n.cost;
                    path[n.index] = now.index;
                    pq.offer(new Node(n.index, dist[n.index]));
                }
            }
        }
        return dist[end];
    }

}
```
### **문제 풀이**
- A도시 부터 B도시까지의 최단 거리와 그 루트를 구하는 최단 거리 알고리즘 다익스트라를 사용하는 문제이다.



### **[출처]**
https://www.acmicpc.net/problem/11779