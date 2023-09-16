## **미로만들기**


### ***problem***
n×n 바둑판 모양으로 총 n2개의 방이 있다. 일부분은 검은 방이고 나머지는 모두 흰 방이다. 검은 방은 사면이 벽으로 싸여 있어 들어갈 수 없다. 서로 붙어 있는 두 개의 흰 방 사이에는 문이 있어서 지나다닐 수 있다. 윗줄 맨 왼쪽 방은 시작방으로서 항상 흰 방이고, 아랫줄 맨 오른쪽 방은 끝방으로서 역시 흰 방이다.

시작방에서 출발하여 길을 찾아서 끝방으로 가는 것이 목적인데, 아래 그림의 경우에는 시작방에서 끝 방으로 갈 수가 없다. 부득이 검은 방 몇 개를 흰 방으로 바꾸어야 하는데 되도록 적은 수의 방의 색을 바꾸고 싶다.

아래 그림은 n=8인 경우의 한 예이다.

<p style="text-align: center;"><img alt="" src="https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/upload/images/MW747ysuRPRpii4KaUvptRDAx46g.png" style="width: 263px; height: 207px; "></p>

위 그림에서는 두 개의 검은 방(예를 들어 (4,4)의 방과 (7,8)의 방)을 흰 방으로 바꾸면, 시작방에서 끝방으로 갈 수 있지만, 어느 검은 방 하나만을 흰 방으로 바꾸어서는 불가능하다. 검은 방에서 흰 방으로 바꾸어야 할 최소의 수를 구하는 프로그램을 작성하시오.

단, 검은 방을 하나도 흰방으로 바꾸지 않아도 되는 경우는 0이 답이다.

#### **Input**
첫 줄에는 한 줄에 들어가는 방의 수 n(1 ≤ n ≤ 50)이 주어지고, 다음 n개의 줄의 각 줄마다 0과 1이 이루어진 길이가 n인 수열이 주어진다. 0은 검은 방, 1은 흰 방을 나타낸다.

#### **Output**
첫 줄에 흰 방으로 바꾸어야 할 최소의 검은 방의 수를 출력한다.

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
    static int[][] arr;
    static int INF = Integer.MAX_VALUE;
    static int[] dx = {1,0,-1,0} , dy = {0,1,0,-1};

    public static void main(String[] args) throws IOException {
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());

        arr = new int[N][N];

        for(int i=0;i<N;i++){
            String[] s = br.readLine().split("");
            for(int j=0;j<N;j++){
                arr[i][j] = Integer.parseInt(s[j]);
            }
        }

        int answer = dijkstra(N);
        bw.write(answer+"\n");
        bw.flush();
    }

    private static int dijkstra(int n){
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[][] dist = new int[n][n];
        boolean[][] visited = new boolean[n][n];

        for(int i=0;i<n;i++){
            Arrays.fill(dist[i], INF);
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
                if(nx >= 0 && ny >= 0 && nx < n && ny < n){
                    int nextCount = arr[nx][ny] == 1 ? 0 : 1;
                    if(!visited[nx][ny] && dist[nx][ny] > dist[x][y] + nextCount){
                        dist[nx][ny] = dist[x][y] + nextCount;
                        pq.offer(new Node(nx,ny,dist[nx][ny]));
                    }
                }
            }
        }
        return dist[n-1][n-1];
    }
}
```
### **문제 풀이**
- 이 문제는 직전에 풀었던 [백준 1261번](https://www.acmicpc.net/problem/1261)문제와 매우 비슷한 문제이다. 둘의 가장 큰 차이는 백준 1261번 문제는 1이 벽, 0이 빈공간이여서 답을 구할 때 그냥 값을 더해주면 되었다. 하지만 이 문제는 1이 빈공간, 0이 벽이기에 더하는 값을 주의해줘야한다.


### **[출처]**
https://www.acmicpc.net/problem/2665