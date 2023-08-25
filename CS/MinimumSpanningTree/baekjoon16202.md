## **MST 게임**


### ***problem***
N개의 정점과 M개의 양방향간선으로 이루어진 단순 그래프 G가 있다. 단순 그래프란, self-loop 간선(한 정점에서 자기 자신을 이어주는 간선)이 없고, 임의의 두 정점 사이 최대 한 개의 간선이 있는 그래프를 말한다. 단순 그래프의 스패닝 트리(Spanning Tree)는 다음 조건을 만족하는 간선의 집합이다.

1. 스패닝 트리를 구성하는 간선의 개수는 N-1개이다.
2. 그래프의 임의의 두 정점을 골랐을 때, 스패닝 트리에 속하는 간선만 이용해서 두 정점을 연결하는 경로를 구성할 수 있어야 한다.

스패닝 트리 중에서 간선의 가중치의 합이 최소인 것을 최소 스패닝 트리(Minimum Spanning Tree, MST)라고 부른다.

이제 그래프에서 MST 게임을 하려고 한다.

- MST 게임은 그래프에서 간선을 하나씩 제거하면서 MST의 비용을 구하는 게임이다. MST의 비용이란 MST를 이루고 있는 가중치의 합을 의미한다. 각 턴의 점수는 해당 턴에서 찾은 MST의 비용이 된다. 
- 이 과정은 K턴에 걸쳐서 진행되며, 첫 턴에는 입력으로 주어진 그래프의 MST 비용을 구해야 한다.
- 각 턴이 종료된 후에는 그 턴에서 구한 MST에서 가장 가중치가 작은 간선 하나를 제거한다.
- 한 번 제거된 간선은 이후의 턴에서 사용할 수 없다.
- 어떤 턴에서 MST를 만들 수 없다면, 그 턴의 점수는 0이다. 당연히 이후 모든 턴의 점수도 0점이다. 첫 턴에 MST를 만들 수 없는 경우도 있다.

양방향 간선으로 이루어진 단순 그래프와 K가 주어졌을 때, 각 턴의 점수가 몇 점인지 구하는 프로그램을 작성하시오.

#### **Input**
첫째 줄에 그래프 정점의 개수 N, 그래프 간선의 개수 M, 턴의 수 K가 주어진다. (2 ≤ N ≤ 1,000, 1 ≤ M ≤ min(10,000, N×(N-1)/2), 1 < K ≤ 100)

그 후 M개의 줄에 간선의 정보가 주어진다. 간선의 정보는 간선을 연결하는 두 정점의 번호 x, y로 이루어져 있다. 같은 간선이 여러 번 주어지는 경우는 없다. 간선의 가중치는 주어지는 순서대로 1, 2, ..., M이다.

정점의 번호는 1부터 N까지로 이루어져 있다.

#### **Output**
한 줄에 공백으로 구분하여 K개의 정수를 출력해야 한다. K개의 정수는 각 턴에 얻는 점수를 나타낸다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    static int[][] arr;
    static int INF = Integer.MAX_VALUE;

    static class Node implements Comparable<Node>{
        int prv, index;
        int cost;
        public Node(int index,int prv, int cost){
            this.index = index;
            this.prv = prv;
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
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());

        arr = new int[N+1][N+1];

        for(int i=0;i<M;i++){
            st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            arr[a][b] = arr[b][a] = i+1;
        }

        StringBuilder sb = new StringBuilder();
        for(int i=0;i<K;i++){
            sb.append(prim(N)).append(" ");
        }
        sb.append("\n");
        bw.write(sb.toString());
        bw.flush();
    }

    private static int prim(int v){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dist = new int[v+1];
        boolean[] visited = new boolean[v+1];
        Arrays.fill(dist, INF);
        dist[1] = 0;
        pq.offer(new Node(1,1,0));
        int min = Integer.MAX_VALUE;
        int x = 0, y = 0 , sum = 0, count = 0;
        while(!pq.isEmpty()){
            Node now = pq.poll();
            if(visited[now.index]) continue;
            visited[now.index] = true;

            sum += now.cost;
            count++;
            if(min > now.cost && now.index != 1){
                min = now.cost;
                x = now.index;
                y = now.prv;
            }
            if(count == v){
                arr[x][y] = arr[y][x] = 0;
                return sum;
            }
            for(int i=1;i<=v;i++){
                if(arr[now.index][i] <= 0) continue;
                if(!visited[i] && dist[i] > arr[now.index][i]){
                    dist[i] = arr[now.index][i];
                    pq.offer(new Node(i, now.index,dist[i]));
                }
            }
        }

        return 0;
    }
}
```
### **문제 풀이**


### **[출처]**
https://www.acmicpc.net/problem/16202