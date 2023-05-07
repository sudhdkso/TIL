## **문자열 집합**


### ***problem***
어떤 큰 도화지에 그림이 그려져 있을 때, 그 그림의 개수와, 그 그림 중 넓이가 가장 넓은 것의 넓이를 출력하여라. 단, 그림이라는 것은 1로 연결된 것을 한 그림이라고 정의하자. 가로나 세로로 연결된 것은 연결이 된 것이고 대각선으로 연결이 된 것은 떨어진 그림이다. 그림의 넓이란 그림에 포함된 1의 개수이다.

#### Input
첫째 줄에 도화지의 세로 크기 n(1 ≤ n ≤ 500)과 가로 크기 m(1 ≤ m ≤ 500)이 차례로 주어진다. 두 번째 줄부터 n+1 줄 까지 그림의 정보가 주어진다. (단 그림의 정보는 0과 1이 공백을 두고 주어지며, 0은 색칠이 안된 부분, 1은 색칠이 된 부분을 의미한다)

#### Output
첫째 줄에는 그림의 개수, 둘째 줄에는 그 중 가장 넓은 그림의 넓이를 출력하여라. 단, 그림이 하나도 없는 경우에는 가장 넓은 그림의 넓이는 0이다.

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
    public static int[][] arr;
    public static boolean[][] visited;
    public static int[] dx = {0, 0, 1, -1}, dy = {1, -1, 0, 0};
    public static int bfs(int a, int b, int N, int M){
        int count = 1;
        Queue<Point> q = new LinkedList<>();
        q.add(new Point(a,b));
        visited[a][b] = true;
        while(!q.isEmpty()){
            Point p = q.remove();
            int x = p.x;
            int y = p.y;

            for(int i=0;i<4;i++){
                int nx = x + dx[i];
                int ny = y + dy[i];
                if(nx >= 0 && ny >= 0 && nx < N && ny < M ) {
                    if(arr[nx][ny] == 1 && visited[nx][ny] == false){
                        q.add(new Point(nx,ny));
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

        String[] NM = br.readLine().split(" ");
        int N = Integer.parseInt(NM[0]);
        int M = Integer.parseInt(NM[1]);
        arr = new int[N][M];
        visited = new boolean[N][M];
        for(int i=0; i<N;i++){
            String[] s = br.readLine().split(" ");
            for(int j=0;j<M;j++){
                arr[i][j] = Integer.parseInt(s[j]);
            }
        }
        int answer = 0, max = 0;
        for(int i=0; i<N;i++){
            for(int j=0;j<M;j++){
                if(visited[i][j] == false && arr[i][j] == 1){
                    int result = bfs(i,j,N,M);
                    if(result > 0){
                        answer++;
                        max = Math.max(max, result);
                    }
                }
            }
        }
        bw.write(String.valueOf(answer)+"\n"+String.valueOf(max));
        bw.flush();


    }
}
```

- arr[i][j]에는 (i,j)의 색칠 여부를 integer타입으로 저장한다.
- visited[i][j]에는 (i,j)의 방문 여부를 boolean타입으로 
저장한다.
- bfs에서 그림의 넓이를 count해서 return해준다.
    - return결과값을 max와 비교하여 더 큰 숫자를 max에 저장한다.


### 출처
https://www.acmicpc.net/problem/1926