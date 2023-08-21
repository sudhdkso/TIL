## **전력난**


### ***problem***
성진이는 한 도시의 시장인데 거지라서 전력난에 끙끙댄다. 그래서 모든 길마다 원래 켜져 있던 가로등 중 일부를 소등하기로 하였다. 길의 가로등을 켜 두면 하루에 길의 미터 수만큼 돈이 들어가는데, 일부를 소등하여 그만큼의 돈을 절약할 수 있다.

그러나 만약 어떤 두 집을 왕래할 때, 불이 켜져 있지 않은 길을 반드시 지나야 한다면 위험하다. 그래서 도시에 있는 모든 두 집 쌍에 대해, 불이 켜진 길만으로 서로를 왕래할 수 있어야 한다.

위 조건을 지키면서 절약할 수 있는 최대 액수를 구하시오.

#### **Input**
입력은 여러 개의 테스트 케이스로 구분되어 있다.

각 테스트 케이스의 첫째 줄에는 집의 수 m과 길의 수 n이 주어진다. (1 ≤ m ≤ 200000, m-1 ≤ n ≤ 200000)

이어서 n개의 줄에 각 길에 대한 정보 x, y, z가 주어지는데, 이는 x번 집과 y번 집 사이에 양방향 도로가 있으며 그 거리가 z미터라는 뜻이다. (0 ≤ x, y < m, x ≠ y)

도시는 항상 연결 그래프의 형태이고(즉, 어떤 두 집을 골라도 서로 왕래할 수 있는 경로가 있다), 도시상의 모든 길의 거리 합은 231미터보다 작다.

입력의 끝에서는 첫 줄에 0이 2개 주어진다.

#### **Output**
각 테스트 케이스마다 한 줄에 걸쳐 절약할 수 있는 최대 비용을 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    static List<Node>[] list;
    static int INF = Integer.MAX_VALUE;

    static class Node implements Comparable<Node>{
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

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        while(true){
            StringTokenizer st = new StringTokenizer(br.readLine()," ");
            int V = Integer.parseInt(st.nextToken());
            int E = Integer.parseInt(st.nextToken());
            if(V == 0 && E == 0){
                break;
            }
            int total = 0;
            init(V);
            for(int i=0;i<E;i++){
                st = new StringTokenizer(br.readLine()," ");
                int a = Integer.parseInt(st.nextToken());
                int b = Integer.parseInt(st.nextToken());
                int c = Integer.parseInt(st.nextToken());
                list[a].add(new Node(b,c));
                list[b].add(new Node(a,c));
                total += c;
            }
            int answer = total - prim(V);
            bw.write(String.valueOf(answer)+"\n");
        }
        bw.flush();
    }

    private static void init(int N){
        list = new List[N];

        for(int i=0;i<N;i++){
            list[i] = new ArrayList<>();
        }
    }
    private static int prim(int v){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[v];
        boolean[] visited = new boolean[v];

        Arrays.fill(dist, INF);
        dist[0] = 0;
        pq.offer(new Node(0,0));
        int min = 0, count = 0;

        while(!pq.isEmpty()){
            Node now = pq.poll();
            if(visited[now.index]) continue;
            visited[now.index] = true;

            min += now.cost;
            count++;

            if(count == v){
                break;
            }

            for(Node n : list[now.index]){
                if(!visited[n.index] && dist[n.index] > n.cost){
                    dist[n.index] = n.cost;
                    pq.offer(new Node(n.index, n.cost));
                }
            }
        }
        return min;
    }
}
```
### **문제 풀이**
- 최소신장트리를 구하고, 모든 마을을 연결하는 비용에서 최대 얼마를 절약할 수 있는지 구하는 문제이다.

- 문제는 여러 테스트케이스로 주어지기 때문에, 각 케이스 별로 인접리스트와 정답을 초기화해주면서 문제의 정답을 구해야한다.

### **[출처]**
https://www.acmicpc.net/problem/6497