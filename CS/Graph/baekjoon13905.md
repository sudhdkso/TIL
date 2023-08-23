## **세부**


### ***problem***
빼빼로 데이를 맞아 혜빈이와 숭이는 세부에 있는 섬에 놀러 갔다. 섬은 바다 위에 떠다니는 집과 집들을 연결하는 오크나무로 되어있는 다리로 이뤄져 있다. 숭이는 혜빈이에게 깜짝 이벤트를 해주기 위해 섬 관리자에게 혜빈이를 이벤트장소에 머물게 해달라고 하였다. 이벤트 당일 날 숭이는 금으로 된 빼빼로들을 들고 이벤트 장소로 이동하려 했다. 하지만 아뿔싸! 각 다리마다 다리 위를 지날 수 있는 무게 제한이 존재했다. 비싼 금빼빼로를 가면서 버리기 아깝던 숭이는 자신이 머물던 집에서 자신이 혜빈이에게 갈 수 있는 최대한의 금빼빼로만을 들고 가려고 한다. 따라서 숭이는 인공위성조차 해킹할 수 있는 당신에게 혜빈이에게 들고 갈 수 있는 최대한의 금빼빼로 개수를 알려달라고 부탁했다. 당신은 인공위성을 해킹하여 집들의 번호와 다리의 무게제한을 알아내었다. 이 정보를 가지고 빨리 숭이를 도와주자.

(금빼빼로 하나의 무게는 1이고, 숭이의 몸무게는 고려하지 않는다.)

#### **Input**
첫 번째 줄에는 섬에 존재하는 집의 수 N(2≤N≤100,000)와 다리의 수 M(1≤M≤300,000)이 주어진다. 두 번째 줄에는 숭이의 출발 위치(s)와 혜빈이의 위치(e)가 주어진다. (1≤s, e≤N, s≠e). 다음 M개의 줄에는 다리의 정보가 주어진다. 각 줄은 “집의 번호 h1(1≤h1≤N), 집의 번호 h2(1≤h2≤N), 다리의 무게제한 k(1≤k≤1,000,000)” 형식으로 주어진다. (h1집과 h2집은 무게제한이 k인 다리로 연결되어 있다.)

#### **Output**
첫 번째 줄에 N명의 학생들 중 오고 가는데 가장 오래 걸리는 학생의 소요시간을 출력한다.

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
        public String toString(){
            return "[ index : "+this.index+" , cost : "+this.cost+" ]\n";
        }
        @Override
        public int compareTo(Node e){
            return Integer.compare(e.cost, this.cost);
        }
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        st = new StringTokenizer(br.readLine()," ");
        int s = Integer.parseInt(st.nextToken());
        int e = Integer.parseInt(st.nextToken());

        init(N);

        for(int i=0;i<M;i++){
            st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = Integer.parseInt(st.nextToken());
            list[a].add(new Node(b,c));
            list[b].add(new Node(a,c));
        }
        int answer = dijkstra(N,s,e);
        bw.write(String.valueOf(answer));
        bw.flush();
    }

    private static void init(int N){
        list = new List[N+1];

        for(int i=1;i<=N;i++){
            list[i] = new ArrayList<>();
        }
    }
    private static int dijkstra(int v, int start, int end){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[v+1];
        boolean[] visited = new boolean[v+1];

        Arrays.fill(dist, 0);
        dist[start] = INF;
        pq.offer(new Node(start,INF));

        while(!pq.isEmpty()){
            Node now = pq.poll();
            if(visited[now.index] || dist[now.index] > now.cost) continue;
            visited[now.index] = true;

            for(Node n : list[now.index]){
                int min = Math.min(dist[now.index], n.cost);
                if( dist[n.index] < min){
                    dist[n.index] = min;
                    pq.offer(new Node(n.index, dist[n.index]));
                }
            }
        }
        return dist[end];
    }
}
```

### **문제 풀이**


### **[출처]**
https://www.acmicpc.net/problem/13905