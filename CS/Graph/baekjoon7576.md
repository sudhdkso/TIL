## **안전 영역**


### ***problem***
철수의 토마토 농장에서는 토마토를 보관하는 큰 창고를 가지고 있다. 토마토는 아래의 그림과 같이 격자 모양 상자의 칸에 하나씩 넣어서 창고에 보관한다. 


창고에 보관되는 토마토들 중에는 잘 익은 것도 있지만, 아직 익지 않은 토마토들도 있을 수 있다. 보관 후 하루가 지나면, 익은 토마토들의 인접한 곳에 있는 익지 않은 토마토들은 익은 토마토의 영향을 받아 익게 된다. 하나의 토마토의 인접한 곳은 왼쪽, 오른쪽, 앞, 뒤 네 방향에 있는 토마토를 의미한다. 대각선 방향에 있는 토마토들에게는 영향을 주지 못하며, 토마토가 혼자 저절로 익는 경우는 없다고 가정한다. 철수는 창고에 보관된 토마토들이 며칠이 지나면 다 익게 되는지, 그 최소 일수를 알고 싶어 한다.

토마토를 창고에 보관하는 격자모양의 상자들의 크기와 익은 토마토들과 익지 않은 토마토들의 정보가 주어졌을 때, 며칠이 지나면 토마토들이 모두 익는지, 그 최소 일수를 구하는 프로그램을 작성하라. 단, 상자의 일부 칸에는 토마토가 들어있지 않을 수도 있다.

#### Input
첫 줄에는 상자의 크기를 나타내는 두 정수 M,N이 주어진다. M은 상자의 가로 칸의 수, N은 상자의 세로 칸의 수를 나타낸다. 단, 2 ≤ M,N ≤ 1,000 이다. 둘째 줄부터는 하나의 상자에 저장된 토마토들의 정보가 주어진다. 즉, 둘째 줄부터 N개의 줄에는 상자에 담긴 토마토의 정보가 주어진다. 하나의 줄에는 상자 가로줄에 들어있는 토마토의 상태가 M개의 정수로 주어진다. 정수 1은 익은 토마토, 정수 0은 익지 않은 토마토, 정수 -1은 토마토가 들어있지 않은 칸을 나타낸다.

토마토가 하나 이상 있는 경우만 입력으로 주어진다.

#### Output
여러분은 토마토가 모두 익을 때까지의 최소 날짜를 출력해야 한다. 만약, 저장될 때부터 모든 토마토가 익어있는 상태이면 0을 출력해야 하고, 토마토가 모두 익지는 못하는 상황이면 -1을 출력해야 한다.

### ***Solution***
``` java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.*;

public class Main {
    private static class Point {
        int x,y,depth;
        public Point(int x,int y, int depth){
            this.x = x;
            this.y = y;
            this.depth = depth;
        }
    }

    private static int[] dx = {1,-1,0,0}, dy = {0,0,1,-1};
    private static boolean[][] visited;
    private static int[][] arr,depth;
    private static Queue<Point> q = new LinkedList<>();

    private static void bfs(int n, int m){

        while(!q.isEmpty()){
            Point p = q.poll();
            int x = p.x;
            int y = p.y;

            for(int i=0;i<4;i++){
                int nx = x + dx[i];
                int ny = y + dy[i];
                if(nx>=0 && ny >=0 && nx<n && ny < m){
                    if(!visited[nx][ny] && arr[nx][ny] == 0){
                        visited[nx][ny] = true;
                        q.add(new Point(nx,ny,p.depth+1));
                        depth[nx][ny] = p.depth+1;
                    }
                }
            }
        }
    }

    public static void main( String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        String[] NM = br.readLine().split(" ");
        int m = Integer.parseInt(NM[0]);
        int n = Integer.parseInt(NM[1]);

        arr = new int[n][m];
        depth = new int[n][m];
        for(int i=0;i<n;i++){
            Arrays.fill(depth[i], -1);
        }
        visited = new boolean[n][m];
        int wall = 0;
        for(int i=0;i<n;i++){
            String[] s = br.readLine().split(" ");
            for(int j=0;j<m;j++){
                arr[i][j] = Integer.parseInt(s[j]);
                if(arr[i][j] == 1){
                    q.add(new Point(i,j,0));
                    visited[i][j] = true;
                    depth[i][j] = 0;
                }
                else if(arr[i][j] == -1){
                    wall++;
                }
            }
        }

        bfs(n,m);
        
        int max = 0, count = 0;
        
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                max = Math.max(max, depth[i][j]);
                if(depth[i][j] == -1){
                    count++;
                }
            }
        }
        
        if(count > wall){
            bw.write(-1+"\n");
        }
        else {
            bw.write(max + "\n");
        }
        
        bw.flush();
    }

}
```
- Point는 위치(x,y)와 depth를 가지고 있는 class이다.
- visited는 방문여부를 저장하는 boolean타입의 이차원 배열이다.
- depth는 주어진 지점으로 부터 (i,j)위치의 토마토가 변화되는 날짜(?)를 담고 있는 int타입의 이차원 배열이다.
- 주어진 토마토 창고의 상태를 배열에 저장을 한다.
    - 익은 토마토가 있는 경우
        - 토마토가 있는 (i,j)위치와 depth를 0으로 전역적으로 선언되어있는 Queue에 추가한다.
        - 토마토가 있는 위치를 visited[i][j]를 true로 한다.
        - depth[i][j]에는 0을 저장한다.

    - 토마토가 들어있지 않은 경우
        - 들어이지 않은 칸의 갯수를 wall에 저장한다.
- Queue를 bfs한다.
    - 이때 방문 가능하며 익지 않은 토마토는 Queue에 추가한다. 
        - 단, (nx,ny)위치와 그 전 p.depth+1을 추가한다.
- Queue에 대해서 bfs를 완료한 후의 depth는 익은 토마토들이 몇일째에 익었는지의 상태를 저장하고 있다.
- depth의 -1의 갯수와 그전에 체크하였던 wall의 갯수를 비교한다.
    -depth의 -1의 갯수가 wall보다 크면 모든 토마토가 다 익지 않은 상태라는 의미이기 때문에 -1을 출력한다.
    - 그게 아니면 max값을 찾아 max를 출력한다.

### 출처
https://www.acmicpc.net/problem/7576