## **도시 건설**


### ***problem***
채완이는 신도시에 건물 사이를 잇는 양방향 도로를 만들려는 공사 계획을 세웠다.

공사 계획을 검토하면서 비용이 생각보다 많이 드는 것을 확인했다.

채완이는 공사하는 데 드는 비용을 아끼려고 한다. 

모든 건물이 도로를 통해 연결되도록 최소한의 도로를 만들려고 한다.

그림에 있는 도로를 다 설치할 때 드는 비용은 62이다. 모든 건물을 연결하는 도로만 만드는 비용은 27로 절약하는 비용은 35이다.

채완이는 도로가 너무 많아 절약되는 금액을 계산하는 데 어려움을 겪고 있다.

채완이를 대신해 얼마나 절약이 되는지 계산해주자.

#### **Input**
첫 번째 줄에 건물의 개수 
$N$ 
$(3 \le N \le 10^5 )$와 도로의 개수 
$M$ 
$(2 \le M \le min( {N(N-1) \over 2}, 5×10^5)) $가 주어진다.

두 번째 줄 부터 
$M + 1$줄까지 건물의 번호 
$a$, 
$b$ 
$(1 \le a, b \le N, a ≠ b)$와 두 건물 사이 도로를 만들 때 드는 비용 
$c (1 \le c \le 10^6)$가 주어진다. 같은 쌍의 건물을 연결하는 두 도로는 주어지지 않는다.

#### **Output**
예산을 얼마나 절약 할 수 있는지 출력한다. 만약 모든 건물이 연결되어 있지 않는다면 -1을 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    static int INF =  Integer.MAX_VALUE;
    static List<Node>[] list;

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
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int N = Integer.parseInt(st.nextToken()); // 노드의 갯수
        int M = Integer.parseInt(st.nextToken());

        list = new List[N+1];
        long total = 0;

        for(int i=1;i<=N;i++){
            list[i] = new ArrayList<>();
        }

        for(int i=0;i<M;i++){
            String[] s = br.readLine().split(" ");
            int a = Integer.parseInt(s[0]);
            int b = Integer.parseInt(s[1]);
            int c = Integer.parseInt(s[2]);
            list[a].add(new Node(b,c));
            list[b].add(new Node(a,c));
            total += c;
        }

        long answer = prim(N);
        if(answer > 0){
            answer = total-answer;
        }

        bw.write(String.valueOf(answer));
        bw.flush();
    }

    private static long prim(int v){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[v+1];
        boolean[] visited = new boolean[v+1];

        Arrays.fill(dist, INF);
        dist[1] = 0;

        pq.offer(new Node(1,0));

        int count = 0;
        long sum = 0;
        while(!pq.isEmpty()){
            Node now = pq.poll();
            if(visited[now.index]) continue;
            visited[now.index] = true;

            sum += now.cost;
            count++;

            if(count == v){
                return sum;
            }

            for(Node n : list[now.index]){
                if(!visited[n.index] && dist[n.index] > n.cost){
                    dist[n.index] = n.cost;
                    pq.offer(n);
                }
            }

        }
        return -1;
    }
}
```
### **문제 풀이**
- 모든 건물을 잇는 도로를 건설하는 최소비용을 구하는 문제이기 때문에 이 문제는 최소신장트리문제이다.

- 이 문제는 인접행렬을 사용하면 메모리초과가 발생하기 때문에 인접리스트를 사용해야한다.

- 그리고 모든 도로를 연결할 때의 비용에서 최소 비용으로 도로를 연결할 떄의 비용의 차를 출력해야하기 때문에 먼저 인접리스트를 만들면서 드는 비용을 변수 total에 모두 더한다.

- 그리고 프림 알고리즘을 통해서 모든 건물을 연결하였을 때의 최소비용을 구하고, total에서 최소비용을 뺀 값을 출력한다.

- 프림알고리즘에서 우선순위큐를 사용하여 시간초과가 발생하지 않을 수 있었다.

### **[출처]**
https://www.acmicpc.net/problem/21924