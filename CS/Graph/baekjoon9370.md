## **미확인 도착지**


### ***problem***
(취익)B100 요원, 요란한 옷차림을 한 서커스 예술가 한 쌍이 한 도시의 거리들을 이동하고 있다. 너의 임무는 그들이 어디로 가고 있는지 알아내는 것이다. 우리가 알아낸 것은 그들이 s지점에서 출발했다는 것, 그리고 목적지 후보들 중 하나가 그들의 목적지라는 것이다. 그들이 급한 상황이기 때문에 목적지까지 우회하지 않고 최단거리로 갈 것이라 확신한다. 이상이다. (취익)

어휴! (요란한 옷차림을 했을지도 모를) 듀오가 어디에도 보이지 않는다. 다행히도 당신은 후각이 개만큼 뛰어나다. 이 후각으로 그들이 g와 h 교차로 사이에 있는 도로를 지나갔다는 것을 알아냈다.

이 듀오는 대체 어디로 가고 있는 것일까?

<p style="text-align: center;"><img alt="" src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images/destination.png" style="font-size:medium;  width:236px"></p>

예제 입력의 두 번째 케이스를 시각화한 것이다. 이 듀오는 회색 원에서 두 검은 원 중 하나로 가고 있고 점선으로 표시된 도로에서 냄새를 맡았다. 따라서 그들은 6으로 향하고 있다.

#### **Input**
첫 번째 줄에는 테스트 케이스의 T(1 ≤ T ≤ 100)가 주어진다. 각 테스트 케이스마다

- 첫 번째 줄에 3개의 정수 n, m, t (2 ≤ n ≤ 2 000, 1 ≤ m ≤ 50 000 and 1 ≤ t ≤ 100)가 주어진다. 각각 교차로, 도로, 목적지 후보의 개수이다.
- 두 번째 줄에 3개의 정수 s, g, h (1 ≤ s, g, h ≤ n)가 주어진다. s는 예술가들의 출발지이고, g, h는 문제 설명에 나와 있다. (g ≠ h)
- 그 다음 m개의 각 줄마다 3개의 정수 a, b, d (1 ≤ a < b ≤ n and 1 ≤ d ≤ 1 000)가 주어진다. a와 b 사이에 길이 d의 양방향 도로가 있다는 뜻이다.

그 다음 t개의 각 줄마다 정수 x가 주어지는데, t개의 목적지 후보들을 의미한다. 이 t개의 지점들은 서로 다른 위치이며 모두 s와 같지 않다.
교차로 사이에는 도로가 많아봐야 1개이다. m개의 줄 중에서 g와 h 사이의 도로를 나타낸 것이 존재한다. 또한 이 도로는 목적지 후보들 중 적어도 1개로 향하는 최단 경로의 일부이다.

#### **Output**
테스트 케이스마다

- 입력에서 주어진 목적지 후보들 중 불가능한 경우들을 제외한 목적지들을 공백으로 분리시킨 오름차순의 정수들로 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
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
    static int INF =  Integer.MAX_VALUE;
    static List<Node>[] nodes;

    static int N;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int T = Integer.parseInt(br.readLine());

        while(T-- > 0){
            StringTokenizer st = new StringTokenizer(br.readLine()," ");
            N = Integer.parseInt(st.nextToken()); // 노드의 갯수
            int M = Integer.parseInt(st.nextToken()); // 간선의 갯수
            int t = Integer.parseInt((st.nextToken())); //후보지 갯수

            init(N);

            st = new StringTokenizer(br.readLine()," ");
            int s = Integer.parseInt(st.nextToken()); // 화가의 출발지
            int g = Integer.parseInt(st.nextToken()); //
            int h = Integer.parseInt(st.nextToken()); //

            for(int i=0;i<M;i++){
                st = new StringTokenizer(br.readLine()," ");
                int a = Integer.parseInt(st.nextToken());
                int b = Integer.parseInt(st.nextToken());
                int d = Integer.parseInt(st.nextToken());

                nodes[a].add(new Node(b,d));
                nodes[b].add(new Node(a,d));
            }
            List<Integer> candidates = new ArrayList<>();
            for(int i=0;i<t;i++){
                int e = Integer.parseInt(br.readLine());
                int way1 = dijkstra(s, g) + dijkstra(g, h) + dijkstra(h, e);
                int way2 = dijkstra(s, h) + dijkstra(h, g) + dijkstra(g, e);
                int result = dijkstra(s, e);

                if(Math.min(way1,way2) == result){
                    candidates.add(e);
                }
            }

            Collections.sort(candidates);

            for(int candi : candidates){
                bw.write(candi+" ");
            }
            bw.write("\n");

        }


        bw.flush();
    }

    private static void init(int N){
        nodes = new List[N+1];

        for(int i=1;i<=N;i++){
            nodes[i] = new ArrayList<>();
        }


    }
    private static int dijkstra(int start, int end){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[N+1];
        boolean[] visited = new boolean[N+1];

        Arrays.fill(dist, INF);

        dist[start] = 0;
        pq.offer(new Node(start, 0));

        while(!pq.isEmpty()){
            Node now  = pq.poll();
            int index = now.index;

            if(visited[index] || dist[now.index] < now.cost) continue;
            visited[index] = true;

            for(Node n : nodes[index]){
                if(!visited[n.index] && dist[n.index] > dist[index] + n.cost){
                    dist[n.index] = dist[index] + n.cost;
                    Node next = new Node(n.index, dist[n.index]);

                    pq.offer(next);
                }
            }
        }
        return dist[end];
    }
}
```
### **문제 풀이**


### **[출처]**
https://www.acmicpc.net/problem/9370