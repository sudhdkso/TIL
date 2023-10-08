## **인터넷 설치**


### ***problem***
오늘 팀전을 다들 열심히 하시는 것을 보고 원장선생님은 세미나 실에 인터넷을 설치해 주기로 마음을 먹으셨다. 하지만 비 협조적인 코레스코 콘도는 원장님께서 학생들에게 인터넷 선을 연결하는 것에 대해서 일부에 대해 돈을 요구하였다.

각각의 학생들의 번호가 1부터 N까지 붙여져 있다고 하면 아무나 서로 인터넷 선이 연결되어 있지 않다. P(P<=10,000)개의 쌍만이 서로 이어 질수 있으며 서로 선을 연결하는데 가격이 다르다.

1번은 다행히 인터넷 서버와 바로 연결되어 있어 인터넷이 가능하다. 우리의 목표는 N번 컴퓨터가 인터넷에 연결하는 것이다. 나머지 컴퓨터는 연결 되어 있거나 연결 안되어 있어도 무방하다.

하지만 코레스코에서는 K개의 인터넷 선에 대해서는 공짜로 연결해주기로 하였다. 그리고 나머지 인터넷 선에 대해서는 남은 것 중 제일 가격이 비싼 것에 대해서만 가격을 받기로 하였다. 이때 원장선생님이 내게 되는 최소의 값을 구하여라.

#### **Input**
첫 번째 줄에 N(1 ≤ N ≤ 1,000), 케이블선의 개수 P(1 ≤ P ≤ 10,000), 공짜로 제공하는 케이블선의 개수 K(0 ≤ K < N)이 주어진다. 다음 P개의 줄에는 케이블이 연결하는 두 컴퓨터 번호와 그 가격이 차례로 들어온다. 가격은 1 이상 1,000,000 이하다.

#### **Output**
첫째 줄에 원장선생님이 내게 되는 최소의 돈을 출력한다. 만약 1번과 N번 컴퓨터를 잇는 것이 불가능 하다면 -1을 출력한다.
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
    static List<Node>[] list;
    static  int N;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());

        list = new List[N+1];

        for(int i=1;i<=N;i++){
            list[i] = new ArrayList<>();
        }
        
        int low = 0, high = 0;
        
        while (M-- > 0) {
            st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = (Integer.parseInt(st.nextToken()));

            list[a].add(new Node(b,c));
            list[b].add(new Node(a,c));
            high = Math.max(high, c);
        }

        int answer = INF;

        while(low <= high){
            int mid = (low+high)/2;
            if(dijkstra(K, mid)){
                answer = mid;
                high = mid-1;
            }
            else{
                low = mid+1;
            }
        }

        bw.write((answer == INF ? -1 : answer)+"\n");
        bw.flush();
    }

    private static boolean dijkstra(int k, int threshold){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[N+1];

        Arrays.fill(dist, INF);

        dist[1] = 0;
        pq.offer(new Node(1, 0));

        while(!pq.isEmpty()){
            Node now = pq.poll();
            int index = now.index;
            int cost = now.cost;

            if(cost > dist[index]) continue;

            for(Node n : list[index]){
                int nc = n.cost > threshold ? now.cost+1 : now.cost;

                if(dist[n.index] > nc){
                    dist[n.index] = nc;
                    pq.offer(new Node(n.index, nc));
                }

            }
        }
        return k >= dist[N];
    }
}
```
### **문제 풀이**
- 이 문제는 최단거리와 이분탐색을 함께 사용하는 문제이다.
- 문제에서 최종적으로 구해야하는 것은, 1부터 N까지 연결하는 최소 비용이 아닌, 1부터 N까지 인터넷을 연결할 때 k개의 케이블을 제외하고 가자 비싼 연결에 대한 최소 비용을 구하는 문제이다.

- 처음에는 단순히 1부터 N까지 최소 비용으로 연결하고, 각 연결에 대한 비용을 내림차순으로 정렬한 후 k+1번째 값을 출력하는 방식으로 문제를 해결하려고 하였다. 하지만 최단 비용으로 연결한다고 하더라도 k+1번째 값이 정답임을 보장할 수 없다.
    - 만약에 연결하는데 1개의 케이블을 무료로 제공한다고 가정하자.
        - k = 1
    1. 1부터 N까지 연결하는 최단 거리로 연결할 때 각각의 비용이 `[1,2,3,4]`한다면 가장 비싼 가중치 **4**인 케이블을 무료로 제공받고, 원장선생님이 내야하는 비용은 **3**이라고 할 수 있다.
    2. 1부터 N까지 최단 거리로 연결하지 않는 경우 각각의 비용이 `[1,1,9]`이라고 한다면, 가장 비싼 가중치 **9**인 케이블을 제외하고 원장선생님이 내야하는 비용은 **1**이 된다.
    - 위의 두가지 케이스를 통해서 이 문제는 최단 거리로 연결이 된다고 하더라도 꼭 정답이 아님을 알 수 있다. 1번은 총 거리가 10이고 2번은 총 거리가 11이지만, 최종적으로 내야하는 비용은 1번보다 2번이 더 작음을 알 수 있다. 
    - 그래서 이 문제는 최단거리를 구한 후 k개를 제하는 방식이 아닌 처음부터 k개의 케이블을 무료로 받는 다고 가정하고 최솟값을 구해야한다.

- 문제에서는 이분탐색을 통해서 각 임계값을 움직이면서 k개를 무료로 제공받는 최소 값을 구한다.
    - 다익스트라 알고리즘은 원래 최단 거리 알고리즘을 사용할때와 거의 비슷하지만, dist의 의미가 조금 다르다. 여기서 dist의 의미는 최단거리 비용이 아닌, 임계값을 넘어가는 케이블의 갯수를 의미한다.
    - 임계값을 넘어가는 케이블들의 갯수를 확인하여 마지막에 dist[N]이 k보다 작거나 같으면 임계값을 넘어가는 케이블들을 모두 무료로 제공받을 수 있다. 하지만 dist[N]이 k보다 크다면 임계값을 넘어가는 케이블들을 무료로 모두 제공받을 수 없기 때문에 임계값이 최소비용이 아님을 의미한다.
    
- 이분탐색과 다익스트라 알고리즘을 통해서 가장 최소 비용을 구하여 출력하며 된다.

### **[출처]**
https://www.acmicpc.net/problem/1800