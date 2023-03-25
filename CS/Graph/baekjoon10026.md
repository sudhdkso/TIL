## **적록색약**


#### ***problem***
적록색약은 빨간색과 초록색의 차이를 거의 느끼지 못한다. 따라서, 적록색약인 사람이 보는 그림은 아닌 사람이 보는 그림과는 좀 다를 수 있다.

크기가 N×N인 그리드의 각 칸에 R(빨강), G(초록), B(파랑) 중 하나를 색칠한 그림이 있다. 그림은 몇 개의 구역으로 나뉘어져 있는데, 구역은 같은 색으로 이루어져 있다. 또, 같은 색상이 상하좌우로 인접해 있는 경우에 두 글자는 같은 구역에 속한다. (색상의 차이를 거의 느끼지 못하는 경우도 같은 색상이라 한다)

예를 들어, 그림이 아래와 같은 경우에

``` json
RRRBB
GGBBB
BBBRR
BBRRR
RRRRR
```

적록색약이 아닌 사람이 봤을 때 구역의 수는 총 4개이다. (빨강 2, 파랑 1, 초록 1) 하지만, 적록색약인 사람은 구역을 3개 볼 수 있다. (빨강-초록 2, 파랑 1)

그림이 입력으로 주어졌을 때, 적록색약인 사람이 봤을 때와 아닌 사람이 봤을 때 구역의 수를 구하는 프로그램을 작성하시오.

#### Input
첫째 줄에 N이 주어진다. (1 ≤ N ≤ 100)

둘째 줄부터 N개 줄에는 그림이 주어진다.

#### Output
적록색약이 아닌 사람이 봤을 때의 구역의 개수와 적록색약인 사람이 봤을 때의 구역의 수를 공백으로 구분해 출력한다.

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
    public Point(int x, int y){
        this.x = x;
        this.y = y;
    }
}

public class Main {
    public static String[][] colorN, colorB;
    public static boolean[][] visited;
    public static int[] dx = {0, 0, 1, -1}, dy = {1, -1, 0, 0};

    public static int bfs(int a,int b,int N, String[][] arr){
        Queue<Point> q = new LinkedList<>();
        q.add(new Point(a,b));
        visited[a][b] = true;
        String s = arr[a][b];
        int count = 1;
        while(!q.isEmpty()){
            Point p = q.remove();
            int x = p.x;
            int y = p.y;

            for(int i=0;i<4;i++){
                int nx = x + dx[i];
                int ny = y + dy[i];
                if(nx >= 0 && ny >= 0 && nx < N && ny < N ) {
                    if(visited[nx][ny] == false && arr[nx][ny].equals(s)){
                        q.add(new Point(nx, ny));
                        visited[nx][ny] = true;
                        count++;
                    }
                }
            }
        }
        return count;
    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int N = Integer.parseInt(br.readLine());

        String[][] colorN = new String[N][N], colorB = new String[N][N];

        visited = new boolean[N][N];

        for(int i=0;i<N;i++){
            String s = br.readLine();
            for(int j=0; j<N; j++){
                colorN[i][j] = s.substring(j,j+1);
                if(colorN[i][j].equals("G")){
                    colorB[i][j] = "R";
                }
                else {
                    colorB[i][j] = colorN[i][j];
                }
            }
        }
        int normalCnt = 0;
        for(int i=0;i<N;i++){
            Arrays.fill(visited[i], false);
        }

        for(int i=0;i<N;i++){
            for(int j=0; j<N; j++){
                if(visited[i][j] == true) continue;
                int result = bfs(i, j, N, colorN);
                if(result > 0) {
                    normalCnt++;
                }
            }
        }

        int colorBldCnt = 0;

        for(int i=0;i<N;i++){
            Arrays.fill(visited[i], false);
        }

        for(int i=0;i<N;i++){
            for(int j=0; j<N; j++){
                if(visited[i][j] == true) continue;
                int result = bfs(i, j, N, colorB);
                if(result > 0){
                    colorBldCnt++;
                }
            }
        }
        bw.write(String.valueOf(normalCnt)+" ");
        bw.write(String.valueOf(colorBldCnt)+"\n");
        bw.flush();


    }
}
```

- colorN은 적록색약이 아닌 사람이 보는 그림의 정보를 담는 이차원 String 배열입니다.
- colorB은 적록색약인 사람이 보는 그림의 정보를 담는 이차원 String 배열입니다.
- visited[i][j]는 colorN,colorB의 (i,j)의 방문여부를 저장하는 이차원 boolean배열입니다.


### 출처
https://www.acmicpc.net/problem/10026