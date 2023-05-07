## **섬의 개수**


### ***problem***
정사각형으로 이루어져 있는 섬과 바다 지도가 주어진다. 섬의 개수를 세는 프로그램을 작성하시오.

한 정사각형과 가로, 세로 또는 대각선으로 연결되어 있는 사각형은 걸어갈 수 있는 사각형이다. 

두 정사각형이 같은 섬에 있으려면, 한 정사각형에서 다른 정사각형으로 걸어서 갈 수 있는 경로가 있어야 한다. 지도는 바다로 둘러싸여 있으며, 지도 밖으로 나갈 수 없다.

#### **Input**
입력은 여러 개의 테스트 케이스로 이루어져 있다. 각 테스트 케이스의 첫째 줄에는 지도의 너비 w와 높이 h가 주어진다. w와 h는 50보다 작거나 같은 양의 정수이다.

둘째 줄부터 h개 줄에는 지도가 주어진다. 1은 땅, 0은 바다이다.

입력의 마지막 줄에는 0이 두 개 주어진다.

#### **Output**
각 테스트 케이스에 대해서, 섬의 개수를 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    private static class Point{
        int x,y;
        public Point(int x,int y){
            this.x = x;
            this.y = y;
        }
    }

    public static boolean[][] visited;
    public static int[][] arr;
    public static int[] dx = {1,-1,0,0,-1,-1,1,1},dy={0,0,1,-1,-1,1,-1,1};

    public static void bfs(int a,int b, int n,int m) {
        Queue<Point> q = new LinkedList<>();
        q.offer(new Point(a,b));
        visited[a][b] = true;
        while(!q.isEmpty()){
            Point p = q.poll();
            int x = p.x;
            int y = p.y;
            for(int i=0;i<8;i++){
                int nx = x + dx[i];
                int ny = y + dy[i];
                if(nx >= 0 && ny >= 0 && nx < n && ny < m){
                    if(!visited[nx][ny] && arr[nx][ny] == 1){
                        visited[nx][ny] = true;
                        q.offer(new Point(nx,ny));
                    }
                }
            }
        }
    }



    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        while (true){
            String[] s1 = br.readLine().split(" ");
            int m = Integer.parseInt(s1[0]);
            int n = Integer.parseInt(s1[1]);
            if(n ==0 && m == 0) break;

            arr = new int[n][m];
            visited = new boolean[n][m];
            for(int i=0;i<n;i++){
                String[] s2 = br.readLine().split(" ");
                for(int j=0;j<m;j++){
                    arr[i][j] = Integer.parseInt(s2[j]);
                }
            }
            int count = 0;
            for(int i=0;i<n;i++){
                for(int j=0;j<m;j++){
                    if(!visited[i][j] && arr[i][j] == 1){
                        count++;
                        bfs(i,j,n,m);
                    }

                }
            }
            bw.write(count+"\n");
        }

        bw.flush();
    }

}
```
- Point는 위치(x,y)를 가지고 있는 class이다.
- visited는 방문여부를 저장하는 boolean타입의 이차원 배열이다.
- dx,dy는 상,하,좌,우 대각선 총 8개를 탐색할 수 있도록 선언해주었다.

- 이중 반복문을 돌면서 bfs로 섬을 탐색하고 count를 증가시킨다.
    - 연결되어있는 섬은 bfs를 한번돌 때 모두 탐색되어 visited가 true가 되어 bfs를 도는 경우만 count해주면 된다.

### 출처
https://www.acmicpc.net/problem/4963