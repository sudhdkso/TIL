## **서강그라운드**


### ***problem***
예은이는 요즘 가장 인기가 있는 게임 서강그라운드를 즐기고 있다. 서강그라운드는 여러 지역중 하나의 지역에 낙하산을 타고 낙하하여, 그 지역에 떨어져 있는 아이템들을 이용해 서바이벌을 하는 게임이다. 서강그라운드에서 1등을 하면 보상으로 치킨을 주는데, 예은이는 단 한번도 치킨을 먹을 수가 없었다. 자신이 치킨을 못 먹는 이유는 실력 때문이 아니라 아이템 운이 없어서라고 생각한 예은이는 낙하산에서 떨어질 때 각 지역에 아이템 들이 몇 개 있는지 알려주는 프로그램을 개발을 하였지만 어디로 낙하해야 자신의 수색 범위 내에서 가장 많은 아이템을 얻을 수 있는지 알 수 없었다.

각 지역은 일정한 길이 l (1 ≤ l ≤ 15)의 길로 다른 지역과 연결되어 있고 이 길은 양방향 통행이 가능하다. 예은이는 낙하한 지역을 중심으로 거리가 수색 범위 m (1 ≤ m ≤ 15) 이내의 모든 지역의 아이템을 습득 가능하다고 할 때, 예은이가 얻을 수 있는 아이템의 최대 개수를 알려주자.
<p align="center">
<img src="https://upload.acmicpc.net/ef3a5124-833a-42ef-a092-fd658bc8e662/-/preview/" width="300px">
</p>

주어진 필드가 위의 그림과 같고, 예은이의 수색범위가 4라고 하자. ( 원 밖의 숫자는 지역 번호, 안의 숫자는 아이템 수, 선 위의 숫자는 거리를 의미한다) 예은이가 2번 지역에 떨어지게 되면 1번,2번(자기 지역), 3번, 5번 지역에 도달할 수 있다. (4번 지역의 경우 가는 거리가 3 + 5 = 8 > 4(수색범위) 이므로 4번 지역의 아이템을 얻을 수 없다.) 이렇게 되면 예은이는 23개의 아이템을 얻을 수 있고, 이는 위의 필드에서 예은이가 얻을 수 있는 아이템의 최대 개수이다.

#### **Input**
첫째 줄에는 지역의 개수 n (1 ≤ n ≤ 100)과 예은이의 수색범위 m (1 ≤ m ≤ 15), 길의 개수 r (1 ≤ r ≤ 100)이 주어진다.

둘째 줄에는 n개의 숫자가 차례대로 각 구역에 있는 아이템의 수 t (1 ≤ t ≤ 30)를 알려준다.

세 번째 줄부터 r+2번째 줄 까지 길 양 끝에 존재하는 지역의 번호 a, b, 그리고 길의 길이 l (1 ≤ l ≤ 15)가 주어진다.

지역의 번호는 1이상 n이하의 정수이다. 두 지역의 번호가 같은 경우는 없다.

#### **Output**
예은이가 얻을 수 있는 최대 아이템 개수를 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    static class Node implements Comparable<Node>{
        int index,cost;
        public Node(int index, int cost){
            this.index = index;
            this.cost = cost;
        }

        @Override
        public int compareTo(Node e){
            return Integer.compare(cost,e.cost);
        }
    }
    static int INF = Integer.MAX_VALUE;
    static int[][] list;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int N = Integer.parseInt(st.nextToken()); // 노드의 갯수
        int M = Integer.parseInt(st.nextToken()); // 수색 범위
        int R = Integer.parseInt(st.nextToken()); //길의 갯수
        int[] item = new int[N];

        String[] items = br.readLine().split(" ");

        for(int i=0;i<N;i++){
            item[i] = Integer.parseInt(items[i]);
        }

        list = new int[N+1][N+1];

        for(int i=0;i<R;i++){
            String[] s = br.readLine().split(" ");
            int a = Integer.parseInt(s[0]);
            int b = Integer.parseInt(s[1]);
            int c = Integer.parseInt(s[2]);
            list[a][b] = list[b][a] = c;

        }
        int max = 0;
        for(int i=1;i<=N;i++){
            int[] result = dijkstra(i,N);
            int total = 0;
            for(int j=0;j<result.length;j++){
                if(result[j] <= M){
                    total += item[j-1];
                }
            }

            max = Math.max(max, total);
        }
        bw.write(String.valueOf(max)+"\n");
        bw.flush();
    }

    private static int[] dijkstra(int start, int v){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[v+1];
        boolean[] visited = new boolean[v+1];
        Arrays.fill(dist, INF);
        dist[start] = 0;
        pq.offer(new Node(start, 0));
        while(!pq.isEmpty()){
            int now = pq.poll().index;
            if(visited[now]) continue;
            visited[now] = true;
            for(int i=1;i<=v;i++){
                if(!visited[i] && list[now][i] > 0 && dist[i] > dist[now] + list[now][i]){
                    dist[i] = dist[now] + list[now][i];
                    pq.offer(new Node(i, dist[i]));
                }
            }
        }

        return dist;
    }

}



```
### **문제 풀이**



### **[출처]**
https://www.acmicpc.net/problem/14938