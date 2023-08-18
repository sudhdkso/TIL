## **행성 연결**


### ***problem***
시간 제한	메모리 제한	제출	정답	맞힌 사람	정답 비율
1 초	256 MB	6027	2817	2053	43.709%
문제
홍익 제국의 중심은 행성 T이다. 제국의 황제 윤석이는 행성 T에서 제국을 효과적으로 통치하기 위해서, N개의 행성 간에 플로우를 설치하려고 한다.

두 행성 간에 플로우를 설치하면 제국의 함선과 무역선들은 한 행성에서 다른 행성으로 무시할 수 있을 만큼 짧은 시간만에 이동할 수 있다. 하지만, 치안을 유지하기 위해서 플로우 내에 제국군을 주둔시켜야 한다.

모든 행성 간에 플로우를 설치하고 플로우 내에 제국군을 주둔하면, 제국의 제정이 악화되기 때문에 황제 윤석이는 제국의 모든 행성을 연결하면서 플로우 관리 비용을 최소한으로 하려 한다.

N개의 행성은 정수 1,…,N으로 표시하고, 행성 i와 행성 j사이의 플로우 관리비용은 Cij이며, i = j인 경우 항상 0이다.

제국의 참모인 당신은 제국의 황제 윤석이를 도와 제국 내 모든 행성을 연결하고, 그 유지비용을 최소화하자. 이때 플로우의 설치비용은 무시하기로 한다.

#### **Input**
입력으로 첫 줄에 행성의 수 N (1 ≤ N ≤ 1000)이 주어진다.

두 번째 줄부터 N+1줄까지 각 행성간의 플로우 관리 비용이 N x N 행렬 (Cij), (1 ≤ i, j ≤ N, 1 ≤ Cij ≤ 100,000,000, Cij = Cji, Cii = 0) 로 주어진다.

#### **Output**
모든 행성을 연결했을 때, 최소 플로우의 관리비용을 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    static int INF =  Integer.MAX_VALUE;
    static int[][] list;

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

        int N = Integer.parseInt(br.readLine()); // 노드의 갯수

        list = new int[N][N];

        for(int i=0;i<N;i++){
            String[] s = br.readLine().split(" ");
            for(int j=0;j<N;j++){
                list[i][j] = Integer.parseInt(s[j]);
            }
        }
        long answer = prim(N);
        bw.write(String.valueOf(answer));
        bw.flush();
    }

    private static long prim(int v){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[v];
        boolean[] visited = new boolean[v];

        Arrays.fill(dist, INF);
        dist[0] = 0;

        int count = 0;
        long sum = 0;

        pq.offer(new Node(0,0));

        while(true){
            Node now = pq.poll();
            int index = now.index;
            int cost = now.cost;

            if(visited[index]) continue;

            visited[index] = true;
            sum += cost;
            count++;

            if(count == v){
                break;
            }

            for(int i=0;i<v;i++){
                if(!visited[i] && dist[i] > list[index][i]){
                    dist[i] = list[index][i];
                    pq.offer(new Node(i,dist[i]));
                }
            }
        }
        return sum;
    }
}
```
### **문제 풀이**
- 모든 행성을 연결하고 관리하는 최소비용을 구하는 최소신장트리 문제이다.


### **[출처]**
https://www.acmicpc.net/problem/16398