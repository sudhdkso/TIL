## **공주님을 구해라!**


### ***problem***
용사는 마왕이 숨겨놓은 공주님을 구하기 위해 (N, M) 크기의 성 입구 (1,1)으로 들어왔다. 마왕은 용사가 공주를 찾지 못하도록 성의 여러 군데 마법 벽을 세워놓았다. 용사는 현재의 가지고 있는 무기로는 마법 벽을 통과할 수 없으며, 마법 벽을 피해 (N, M) 위치에 있는 공주님을 구출해야만 한다.

마왕은 용사를 괴롭히기 위해 공주에게 저주를 걸었다. 저주에 걸린 공주는 T시간 이내로 용사를 만나지 못한다면 영원히 돌로 변하게 된다. 공주님을 구출하고 프러포즈 하고 싶은 용사는 반드시 T시간 내에 공주님이 있는 곳에 도달해야 한다. 용사는 한 칸을 이동하는 데 한 시간이 걸린다. 공주님이 있는 곳에 정확히 T시간만에 도달한 경우에도 구출할 수 있다. 용사는 상하좌우로 이동할 수 있다.


성에는 이전 용사가 사용하던 전설의 명검 "그람"이 숨겨져 있다. 용사가 그람을 구하면 마법의 벽이 있는 칸일지라도, 단숨에 벽을 부수고 그 공간으로 갈 수 있다. "그람"은 성의 어딘가에 반드시 한 개 존재하고, 용사는 그람이 있는 곳에 도착하면 바로 사용할 수 있다. 그람이 부술 수 있는 벽의 개수는 제한이 없다.

우리 모두 용사가 공주님을 안전하게 구출 할 수 있는지, 있다면 얼마나 빨리 구할 수 있는지 알아보자.


#### **Input**
첫 번째 줄에는 성의 크기인 N, M 그리고 공주에게 걸린 저주의 제한 시간인 정수 T가 주어진다. 첫 줄의 세 개의 수는 띄어쓰기로 구분된다. (3 ≤ N, M ≤ 100, 1 ≤ T ≤ 10000)

두 번째 줄부터 N+1번째 줄까지 성의 구조를 나타내는 M개의 수가 띄어쓰기로 구분되어 주어진다. 0은 빈 공간, 1은 마법의 벽, 2는 그람이 놓여있는 공간을 의미한다. (1,1)과 (N,M)은 0이다.

#### **Output**
용사가 제한 시간 T시간 이내에 공주에게 도달할 수 있다면, 공주에게 도달할 수 있는 최단 시간을 출력한다.

만약 용사가 공주를 T시간 이내에 구출할 수 없다면, "Fail"을 출력한다.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {

    public final static int ROAD = 0, WALL = 1, SWORD = 2;
    public static boolean[][][] visited;
    public static int[] dx = {1,-1,0,0}, dy = {0,0,1,-1};
    public static int[][] arr;

    private static class Point{
        int x,y,time;
        boolean sword;
        public Point(int x,int y,int time, boolean sword){
            this.x = x;
            this.y = y;
            this.time = time;
            this.sword = sword;
        }

        public Point(int x,int y){
            this(x,y,0,false);
        }
    }

    public static int bfs(int n,int m){
        Queue<Point> q = new LinkedList<>();
        if(arr[0][0] == SWORD){
            q.add(new Point(0,0,0, true));
            visited[0][0][1] = true;
        }
        else{
            q.add(new Point(0,0, 0,false));
            visited[0][0][0] = true;
        }

        while(!q.isEmpty()){
            Point p = q.poll();
            int x = p.x;
            int y = p.y;
            for(int i=0;i<4;i++){
                int nx = x + dx[i];
                int ny = y + dy[i];
                if(nx == n-1 && ny == m-1) return p.time+1;
                if(nx>= 0 && ny >=0 && nx <n && ny <m){
                    if( !visited[nx][ny][0] && arr[nx][ny] == SWORD){
                        visited[nx][ny][1] = true;
                        q.add(new Point(nx,ny,p.time+1, true));
                        continue;
                    }
                    if(!visited[nx][ny][1] && p.sword == true){
                        visited[nx][ny][1] = true;
                        q.add(new Point(nx,ny,p.time+1, p.sword));
                    }
                    if(!visited[nx][ny][0] && arr[nx][ny] == ROAD){
                        visited[nx][ny][0] = true;
                        q.add(new Point(nx,ny,p.time+1, p.sword));
                    }
                }
            }
        }
        return -1;
    }

    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        String[] NMT = br.readLine().split(" ");

        int n = Integer.parseInt(NMT[0]);
        int m = Integer.parseInt(NMT[1]);
        int t = Integer.parseInt(NMT[2]);

        arr = new int[n][m];
        visited = new boolean[n][m][2];

        for(int i=0;i<n;i++){
            String[] s = br.readLine().split(" ");
            for(int j=0;j<m;j++){
                arr[i][j] = Integer.parseInt(s[j]);
            }
        }

        int result = bfs(n,m);
        StringBuilder sb = new StringBuilder();
        if(result > 0 && t >= result){
            sb.append(result+"\n");
        }
        else  {
            sb.append("Fail"+"\n");
        }

        bw.write(String.valueOf(sb));
        bw.flush();
    }

}
```
- 단순 bfs문제일 줄 알았는데 그람의 유무에 따라 visited를 따로 체크해줘야한다.
    - 따로 체크하지 않으면 그람을 얻기전에 방문한 곳이 그람을 얻은 후 방문해야하는 곳이면 visited가 true이기 때문에 최소 거리여도 방문하지 못 한다.
    - 따라서 visited를 3차원 배열로 선언해주었다.

- Point class에 x,y그리고 시간, 그람의 유무를 가질 수 있도록 한다.
- bfs를 반복하면서 공주가 있는 곳에 도착하면 지금까지 걸린시간 +1을 return주고 마지막까지 도착하지 못하면 -1를 return해준다.
    - bfs를 통해 상하좌우의 칸을 확인하고 그람을 얻기전에는 visited[i][j][0]을 체크하고 그람을 얻은 후에는 visited[i][j][1]을 체크해준다.
- 위의 bfs결과를 result에 저장하여 제한시간이내에 공주에게 도착했는지도 확인한다.
- 제한시간내에 도착하지 못하거나 아예 도착하지 못 한 경우는 "Fail"을 StringBuilder에 append해준다.
- 그외에 제한시간내에 도착하면서 result가 0보다 크면 result값을 StringBuilder에 append해준다.



### 출처
https://www.acmicpc.net/problem/17836