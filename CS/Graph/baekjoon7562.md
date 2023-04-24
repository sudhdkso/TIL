## **나이트의 이동**


### **problem**
체스판 위에 한 나이트가 놓여져 있다. 나이트가 한 번에 이동할 수 있는 칸은 아래 그림에 나와있다. 나이트가 이동하려고 하는 칸이 주어진다. 나이트는 몇 번 움직이면 이 칸으로 이동할 수 있을까?

#### **Input**
입력의 첫째 줄에는 테스트 케이스의 개수가 주어진다.

각 테스트 케이스는 세 줄로 이루어져 있다. 첫째 줄에는 체스판의 한 변의 길이 l(4 ≤ l ≤ 300)이 주어진다. 체스판의 크기는 l × l이다. 체스판의 각 칸은 두 수의 쌍 {0, ..., l-1} × {0, ..., l-1}로 나타낼 수 있다. 둘째 줄과 셋째 줄에는 나이트가 현재 있는 칸, 나이트가 이동하려고 하는 칸이 주어진다.

#### **Output**
각 테스트 케이스마다 나이트가 최소 몇 번만에 이동할 수 있는지 출력한다.

### **Solution**
``` java
import java.io.*;
import java.util.*;

public class Main {
    private static class Point{
        int x,y,count;
        public Point(int x,int y, int count){
            this.x = x;
            this.y = y;
            this.count = count;
        }
    }

    public static ArrayList<Point> arrayList = new ArrayList<>();
    public static boolean[][] visited;
    public static int[][] arr;
    public static int[] dx = {-2,-1,2,1,-2,-1,2,1},dy={-1,-2,-1,-2,1,2,1,2};

    public static int bfs(int sx,int sy,int ex,int ey, int n) {
        Queue<Point> q = new LinkedList<>();
        q.offer(new Point(sx,sy,0));
        visited[sx][sy] = true;
        while(!q.isEmpty()){
            Point p = q.poll();
            int x = p.x;
            int y = p.y;
            if(x == ex && y == ey){
                return p.count;
            }
            for(int i=0;i<8;i++){
                int nx = x + dx[i];
                int ny = y + dy[i];
                if(nx >= 0 && ny >= 0 && nx < n && ny < n){
                    if(!visited[nx][ny]){
                        visited[nx][ny] = true;
                        q.offer(new Point(nx,ny,p.count+1));
                    }
                }
            }
        }
        return -1;
    }



    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int t = Integer.parseInt(br.readLine());

        while(t-- > 0){
            int n = Integer.parseInt(br.readLine());

            String[] s = br.readLine().split(" ");
            int sx = Integer.parseInt(s[0]);
            int sy = Integer.parseInt(s[1]);

            String[] e = br.readLine().split(" ");
            int ex = Integer.parseInt(e[0]);
            int ey = Integer.parseInt(e[1]);
            visited = new boolean[n][n];

            int result = bfs(sx,sy,ex,ey,n);
            bw.write(result+"\n");
        }

        bw.flush();
    }

}

```
- Point는 위치(x,y)와 count를 가지고 있는 class이다.
- visited는 방문여부를 저장하는 boolean타입의 이차원 배열이다.
- dx,dy는 나이트가 이동할 수 있는 좌표 총 8개를 탐색할 수 있도록 선언해주었다.

- bfs로 나이트의 시작점인 (sx,sy)부터 탐색을 시작한다.
    - (sx,sy)부터 갈 수 있는 곳을 q에 넣어 모두 탐색하며 ex,ey가 되었을때 count를 return해준다.

### 출처
https://www.acmicpc.net/problem/7562