### 문자열 집합


#### ***problem***
N×M크기의 배열로 표현되는 미로가 있다.
<table>
  <tr>
    <td>1</td>
    <td>0</td>
    <td>1</td>
    <td>1</td>
    <td>1</td>
    <td>1</td>
  </tr>
  <tr>
    <td>1</td>
    <td>0</td>
    <td>1</td>
    <td>0</td>
    <td>1</td>
    <td>0</td>
  </tr>
  <tr>
    <td>1</td>
    <td>0</td>
    <td>1</td>
    <td>0</td>
    <td>1</td>
    <td>1</td>
  </tr>
  <tr>
    <td>1</td>
    <td>1</td>
    <td>1</td>
    <td>0</td>
    <td>1</td>
    <td>1</td>
  </tr>
</table>
미로에서 1은 이동할 수 있는 칸을 나타내고, 0은 이동할 수 없는 칸을 나타낸다. 이러한 미로가 주어졌을 때, (1, 1)에서 출발하여 (N, M)의 위치로 이동할 때 지나야 하는 최소의 칸 수를 구하는 프로그램을 작성하시오. 한 칸에서 다른 칸으로 이동할 때, 서로 인접한 칸으로만 이동할 수 있다.

위의 예에서는 15칸을 지나야 (N, M)의 위치로 이동할 수 있다. 칸을 셀 때에는 시작 위치와 도착 위치도 포함한다.

#### Input
첫째 줄에 두 정수 N, M(2 ≤ N, M ≤ 100)이 주어진다. 다음 N개의 줄에는 M개의 정수로 미로가 주어진다. 각각의 수들은 **붙어서** 입력으로 주어진다.

#### Output
첫째 줄에 지나야 하는 최소의 칸 수를 출력한다. 항상 도착위치로 이동할 수 있는 경우만 입력으로 주어진다.

### ***Solution***
``` java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.*;

class Point {
    int x;
    int y;
    public Point(int x,int y){
        this.x = x;
        this.y = y;
    }
}

public class Main {
    public static int[][] arr,distance;
    public static boolean[][] visited;
    public static int[] dx = {0, 0, 1, -1}, dy = {1, -1, 0, 0};

    public static void bfs(int N, int M){
        Queue<Point> q = new LinkedList<>();
        q.add(new Point(0,0));
        visited[0][0] = true;
        distance[0][0] = 1;

        while(!q.isEmpty()){
            Point p = q.remove();
            int x = p.x;
            int y = p.y;
            for(int i=0;i<4;i++){
                int nx = x + dx[i];
                int ny = y + dy[i];
                if(nx>= 0 && ny >= 0 && nx < N && ny <M) {
                    if(visited[nx][ny] == false && arr[nx][ny] == 1){
                        q.add(new Point(nx, ny));
                        distance[nx][ny] = distance[x][y]+1;
                        visited[nx][ny] = true;
                    }
                }
            }
        }
    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));


        String[] NM = br.readLine().split(" ");
        int N = Integer.parseInt(NM[0]);
        int M = Integer.parseInt(NM[1]);
        arr = new int[N][M];
        distance = new int[N][M];
        visited = new boolean[N][M];

        for(int i=0; i<N; i++) {
            String s = br.readLine();
            for(int j=0;j<M;j++){
                arr[i][j] = Integer.parseInt(s.substring(j,j+1));
            }
        }

        bfs(N, M);
        bw.write(String.valueOf(distance[N-1][M-1]));
        bw.flush();
    }
}

```

- bfs를 사용한 미로찾기 문제이다.
- distance[i][j]에는 (0,0) ~ (i,j)까지의 거리를 담고 있다.
- visited[i][j]에는 (i,j)의 방문 여부를 boolean타입으로 저장합니다.

### 출처
https://www.acmicpc.net/problem/2178