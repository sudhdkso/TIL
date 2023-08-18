## **도시 분할 계획**


### ***problem***
동물원에서 막 탈출한 원숭이 한 마리가 세상구경을 하고 있다. 그러다가 평화로운 마을에 가게 되었는데, 그곳에서는 알 수 없는 일이 벌어지고 있었다.

마을은 N개의 집과 그 집들을 연결하는 M개의 길로 이루어져 있다. 길은 어느 방향으로든지 다닐 수 있는 편리한 길이다. 그리고 각 길마다 길을 유지하는데 드는 유지비가 있다. 임의의 두 집 사이에 경로가 항상 존재한다.

마을의 이장은 마을을 두 개의 분리된 마을로 분할할 계획을 가지고 있다. 마을이 너무 커서 혼자서는 관리할 수 없기 때문이다. 마을을 분할할 때는 각 분리된 마을 안에 집들이 서로 연결되도록 분할해야 한다. 각 분리된 마을 안에 있는 임의의 두 집 사이에 경로가 항상 존재해야 한다는 뜻이다. 마을에는 집이 하나 이상 있어야 한다.

그렇게 마을의 이장은 계획을 세우다가 마을 안에 길이 너무 많다는 생각을 하게 되었다. 일단 분리된 두 마을 사이에 있는 길들은 필요가 없으므로 없앨 수 있다. 그리고 각 분리된 마을 안에서도 임의의 두 집 사이에 경로가 항상 존재하게 하면서 길을 더 없앨 수 있다. 마을의 이장은 위 조건을 만족하도록 길들을 모두 없애고 나머지 길의 유지비의 합을 최소로 하고 싶다. 이것을 구하는 프로그램을 작성하시오.

#### **Input**
첫째 줄에 집의 개수 N, 길의 개수 M이 주어진다. N은 2이상 100,000이하인 정수이고, M은 1이상 1,000,000이하인 정수이다. 그 다음 줄부터 M줄에 걸쳐 길의 정보가 A B C 세 개의 정수로 주어지는데 A번 집과 B번 집을 연결하는 길의 유지비가 C (1 ≤ C ≤ 1,000)라는 뜻이다.

임의의 두 집 사이에 경로가 항상 존재하는 입력만 주어진다.

#### **Output**
첫째 줄에 없애고 남은 길 유지비의 합의 최솟값을 출력한다.

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

        }
        int answer = prim(N);
        bw.write(String.valueOf(answer));
        bw.flush();
    }

    private static int prim(int v){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[v+1];
        Arrays.fill(dist, INF);
        boolean[] visited = new boolean[v+1];
        int count = 0, sum = 0, max = 0;
        dist[1] = 0;
        pq.offer(new Node(1,0));
        while(true){
            Node now = pq.poll();
            int index = now.index;
            int cost = now.cost;

            if(visited[index]) continue;

            visited[index] = true;
            sum += cost;
            max = Math.max(cost, max);
            count++;

            if(count == v){
                break;
            }

            for(Node n : list[index]){
                if(!visited[n.index] && dist[n.index] > n.cost){
                    dist[n.index] = n.cost;
                    pq.offer(new Node(n.index, n.cost));
                }
            }
        }
        return sum - max;
    }
}

```
### **문제 풀이**
- 최소신장트리를 구하고, 모든 마을을 연결하는 최소비용에서 연결된 간선 중 가장 큰 간선의 가중치를 빼면 문제의 답을 구할 수 있다.
- 최소신장 트리 문제를 해결하는 알고리즘으로는 크루스칼 알고리즘과 프림 알고리즘이 있는데 이 문제는 프림 알고리즘을 사용하여 문제를 해결하였다.

- 이 문제도 프림 알고리즘과 크루스칼 알고리즘을 모두 사용할 수 있지만, 속도 측면에서 크루스칼 알고리즘이 프림 알고리즘을 사용하는 것 보다 좀 더 빠를 것이라고 생각된다.

### **[출처]**
https://www.acmicpc.net/problem/1647