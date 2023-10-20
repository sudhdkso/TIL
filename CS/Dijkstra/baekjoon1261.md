## **알고스팟**


### ***problem***
알고스팟 운영진이 모두 미로에 갇혔다. 미로는 N*M 크기이며, 총 1*1크기의 방으로 이루어져 있다. 미로는 빈 방 또는 벽으로 이루어져 있고, 빈 방은 자유롭게 다닐 수 있지만, 벽은 부수지 않으면 이동할 수 없다.

알고스팟 운영진은 여러명이지만, 항상 모두 같은 방에 있어야 한다. 즉, 여러 명이 다른 방에 있을 수는 없다. 어떤 방에서 이동할 수 있는 방은 상하좌우로 인접한 빈 방이다. 즉, 현재 운영진이 (x, y)에 있을 때, 이동할 수 있는 방은 (x+1, y), (x, y+1), (x-1, y), (x, y-1) 이다. 단, 미로의 밖으로 이동 할 수는 없다.

벽은 평소에는 이동할 수 없지만, 알고스팟의 무기 AOJ를 이용해 벽을 부수어 버릴 수 있다. 벽을 부수면, 빈 방과 동일한 방으로 변한다.

만약 이 문제가 알고스팟에 있다면, 운영진들은 궁극의 무기 sudo를 이용해 벽을 한 번에 다 없애버릴 수 있지만, 안타깝게도 이 문제는 Baekjoon Online Judge에 수록되어 있기 때문에, sudo를 사용할 수 없다.

현재 (1, 1)에 있는 알고스팟 운영진이 (N, M)으로 이동하려면 벽을 최소 몇 개 부수어야 하는지 구하는 프로그램을 작성하시오.

#### **Input**
첫째 줄에 미로의 크기를 나타내는 가로 크기 M, 세로 크기 N (1 ≤ N, M ≤ 100)이 주어진다. 다음 N개의 줄에는 미로의 상태를 나타내는 숫자 0과 1이 주어진다. 0은 빈 방을 의미하고, 1은 벽을 의미한다.

(1, 1)과 (N, M)은 항상 뚫려있다.

#### **Output**
첫째 줄에 알고스팟 운영진이 (N, M)으로 이동하기 위해 벽을 최소 몇 개 부수어야 하는지 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    private static class Node implements Comparable<Node>{
        int x,y,count;
        public Node(int x, int y, int count){
            this.x = x;
            this.y = y;
            this.count = count;
        }

        @Override
        public int compareTo(Node e){
            return Integer.compare(this.count, e.count);
        }

    }
    static int INF = Integer.MAX_VALUE;
    static int[][] arr;
    public static void main(String[] args) throws IOException {
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        int M = Integer.parseInt(st.nextToken());
        int N = Integer.parseInt(st.nextToken());

        arr = new int[N][M];
        for(int i=0;i<N;i++){
            String[] s = br.readLine().split("");
            for(int j=0;j<M;j++){
                arr[i][j] = Integer.parseInt(s[j]);
            }
        }
        bw.write(String.valueOf(dijkstra(N,M)));
        bw.flush();
    }
    static int[] dx = {1,0,-1,0} , dy = {0,1,0,-1};
    private static int dijkstra(int n,int m){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[][] dist = new int[n][m];
        boolean[][] visited = new boolean[n][m];
        
        for(int i=0;i<n;i++){
            Arrays.fill(dist[i],INF);
        }

        dist[0][0] = 0;
        pq.offer(new Node(0,0,0));

        while(!pq.isEmpty()){
            Node now = pq.poll();
            int x = now.x;
            int y = now.y;
            if(visited[x][y]) continue;
            visited[x][y] = true;

            for(int i=0;i<4;i++){
                int nx = x + dx[i];
                int ny = y + dy[i];
                if(nx >= 0 && ny >= 0 && nx < n && ny < m){
                    if(!visited[nx][ny] && dist[nx][ny] > dist[x][y] + arr[nx][ny]){
                        dist[nx][ny] = dist[x][y] + arr[nx][ny];
                        pq.offer(new Node(nx,ny,dist[nx][ny]));
                    }
                }
            }
        }
        return dist[n-1][m-1];
    }

}
```
### **문제 풀이**
- 이 문제는 bfs 문제로도 풀 수 있어보이지만, 문제에서 원래 방문했던 곳도 돌아갈 수 있다고 명시되어있다. bfs 모든 곳을 방문하며 방문했던 곳은 돌아가지 않기 때문에 이 문제는 다익스트라 알고리즘으로 해결할 수 있다.

- 문제에서 방의 종류는 0,1 두가지로만 주어지고 0일 때는 부수는 벽의 갯수에 더하지 않고, 1일 때는 부수는 벽의 갯수를 1개 증가시켜야한다. 그렇기 때문에 다익스트라에서 dist에 최단거리를 업데이트할 때 0,1 여부에 상관없이 그냥 더하면 간편하게 답을 구할 수 있다.

### **[출처]**
https://www.acmicpc.net/problem/1261