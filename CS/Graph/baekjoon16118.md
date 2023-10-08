## **달빛 여우**


### ***problem***
관악산 기슭에는 보름달을 기다리는 달빛 여우가 한 마리 살고 있다. 달빛 여우가 보름달의 달빛을 받으면 아름다운 구미호로 변신할 수 있다. 하지만 보름달을 기다리는 건 달빛 여우뿐만이 아니다. 달빛을 받아서 멋진 늑대인간이 되고 싶어 하는 달빛 늑대도 한 마리 살고 있다.

관악산에는 1번부터 N번까지의 번호가 붙은 N개의 나무 그루터기가 있고, 그루터기들 사이에는 M개의 오솔길이 나 있다. 오솔길은 어떤 방향으로든 지나갈 수 있으며, 어떤 두 그루터기 사이에 두 개 이상의 오솔길이 나 있는 경우는 없다. 달빛 여우와 달빛 늑대는 1번 나무 그루터기에서 살고 있다.

보름달이 뜨면 나무 그루터기들 중 하나가 달빛을 받아 밝게 빛나게 된다. 그러면 달빛 여우와 달빛 늑대는 먼저 달빛을 독차지하기 위해 최대한 빨리 오솔길을 따라서 그 그루터기로 달려가야 한다. 이때 달빛 여우는 늘 일정한 속도로 달려가는 반면, 달빛 늑대는 달빛 여우보다 더 빠르게 달릴 수 있지만 체력이 부족하기 때문에 다른 전략을 사용한다. 달빛 늑대는 출발할 때 오솔길 하나를 달빛 여우의 두 배의 속도로 달려가고, 그 뒤로는 오솔길 하나를 달빛 여우의 절반의 속도로 걸어가며 체력을 회복하고 나서 다음 오솔길을 다시 달빛 여우의 두 배의 속도로 달려가는 것을 반복한다. 달빛 여우와 달빛 늑대는 각자 가장 빠르게 달빛이 비치는 그루터기까지 다다를 수 있는 경로로 이동한다. 따라서 둘의 이동 경로가 서로 다를 수도 있다.

출제자는 관악산의 모든 동물을 사랑하지만, 이번에는 달빛 여우를 조금 더 사랑해 주기로 했다. 그래서 달빛 여우가 달빛 늑대보다 먼저 도착할 수 있는 그루터기에 달빛을 비춰 주려고 한다. 이런 그루터기가 몇 개나 있는지 알아보자.

#### **Input**
첫 줄에 나무 그루터기의 개수와 오솔길의 개수를 의미하는 정수 N, M(2 ≤ N ≤ 4,000, 1 ≤ M ≤ 100,000)이 주어진다.

두 번째 줄부터 M개의 줄에 걸쳐 각 줄에 세 개의 정수 a, b, d(1 ≤ a, b ≤ N, a ≠ b, 1 ≤ d ≤ 100,000)가 주어진다. 이는 a번 그루터기와 b번 그루터기 사이에 길이가 d인 오솔길이 나 있음을 의미한다.

#### **Output**
첫 줄에 달빛 여우가 달빛 늑대보다 먼저 도착할 수 있는 나무 그루터기의 개수를 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    static class Node implements Comparable<Node>{
        int index, run;
        long cost;
        public Node(int index, long cost, int run){
            this.index = index;
            this.cost = cost;
            this.run = run;
        }

        public Node(int index, long cost){
            this(index, cost, 0);
        }

        @Override
        public int compareTo(Node e){
            return Long.compare(this.cost, e.cost);
        }
    }
    static long INF =  Long.MAX_VALUE;
    static List<Node>[] list;
    static  int N;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        list = new List[N+1];

        for(int i=1;i<=N;i++){
            list[i] = new ArrayList<>();
        }

        while (M-- > 0) {
            st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = (Integer.parseInt(st.nextToken()))*2;

            list[a].add(new Node(b,c));
            list[b].add(new Node(a,c));
        }

        long[][] wolf = wolfDijkstra();
        long[] fox = foxDijkstra();

        int answer = 0;
        for(int i=2;i<=N; i++){
            //if(fox[i] == INF) continue;
            if(fox[i] < Math.min(wolf[0][i], wolf[1][i])){
                answer++;
            }
        }

        bw.write(answer+"\n");
        bw.flush();
    }

    private static long[][] wolfDijkstra(){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        long[][] dist = new long[2][N+1];

        for(int i=0;i<= 1;i++){
            Arrays.fill(dist[i], INF);
        }
        dist[0][1] = 0;

        pq.offer(new Node(1,0));

        while(!pq.isEmpty()){
            Node now = pq.poll();
            int index = now.index;
            int run = now.run;
            if(now.cost > dist[run][index]) continue;

            for(Node n : list[index]){
                long next = dist[run][index] + (run == 0 ? n.cost/2 : n.cost*2);
                int state = 1-run;
                if(dist[state][n.index] > next){
                    dist[state][n.index] = next;
                    pq.offer(new Node(n.index, next,state));
                }
            }
        }

        return dist;
    }

    private static long[] foxDijkstra(){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        long[] dist = new long[N+1];
        boolean[] visited = new boolean[N+1];
        Arrays.fill(dist, INF);
        dist[1] = 0;

        pq.offer(new Node(1,0));

        while(!pq.isEmpty()){
            Node now = pq.poll();
            int index = now.index;
            if(visited[index] || now.cost > dist[index]) continue;
            visited[index] = true;

            for(Node n : list[index]){
                if(!visited[n.index] && dist[n.index]> dist[index] + n.cost){
                    dist[n.index] = dist[index] + n.cost;
                    pq.offer(new Node(n.index, dist[n.index]));
                }

            }
        }

        return dist;
    }
}
```
### **문제 풀이**
- 이 문제는 최단거리 알고리즘을 사용하는 최단거리 문제이다.
- 문제에서 여우의 경우와 늑대의 경우 두가지가 있는데, 여우는 기본적인 다익스트라 알고리즘의 방식이지만 늑대의 경우는 cost의 값이 계속 바뀌게 되므로, 늑대의 경우와 여우의 경우 다익스트라 알고리즘을 다르게 사용해줘야한다.

- 일단 주어지는 정보들에 대해서 인접리스트를 생성해준다. 이때 만들어지는 인접리스트에서 cost의 값을 저장할 때 원래 비용의 2배를 저장한다.
    - **cost*2**를 저장하는 이유는 늑대 최단 비용을 계산할 때 비용의 절반을 계산해야하는 경우가 존재하는데, cost를 그래도 계산하면 소숫점이하의 값이 누락되는 문제가 존재하기 때문에 처음부터 **cost*2**를 저장한다.
        - ex) 원래 비용이 3이면 절반 값이 1이 되기 때문에 $\frac{1}{2}$가 아닌 $\frac{1}{3}$이 되어버리는 문제가 있다.

- 그리고 여우의 경우는 주어진 인접리스트에 대해서 똑같이 다익스트라 알고리즘을 적용한다.

- 늑대의 경우는 Node에 run이라는 변수를 추가하여 직전에 달렸는지 달리지 않았는지 구분할 수 있도록 만들어 준다.
- 그리고 dist의 이차원 배열을 통해서 달리지 않은 상태의 dist[0][i]와 달린 상태의 dist[1][i]의 경우를 모두 계산한다.
    - run을 통해서 직전에 달렸으면 dist[0][i]에 최단 거리를 계산하여 넣고, 달리지 않았으면 dist[1][i]에 최단 거리 비용을 계산해서 넣는다.

- 위의 방식을 통해서 각각 여우와 늑대에 대해서 최단 거리를 모두 계산하였으면, 각 노드들에 대해서 여우와 늑대의 최소 비용을 비교한다.

- 늑대는 2차원 배열이기 때문에 wolf[0][i]와 wolf[1][i]중 더 작은 값과 fox[i]를 비교하여 wolf의 값이 더 크면 answer를 증가시킨다.
    - 비용이 더 큰 값이 해당 노드에 도착하는 시간이 더 오래걸리는 것이다.

- 그리고 answer에 대해서 출력한다.
 
### **[출처]**
https://www.acmicpc.net/problem/16118