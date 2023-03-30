### **안전 영역**


#### ***problem***
재난방재청에서는 많은 비가 내리는 장마철에 대비해서 다음과 같은 일을 계획하고 있다. 먼저 어떤 지역의 높이 정보를 파악한다. 그 다음에 그 지역에 많은 비가 내렸을 때 물에 잠기지 않는 안전한 영역이 최대로 몇 개가 만들어 지는 지를 조사하려고 한다. 이때, 문제를 간단하게 하기 위하여, 장마철에 내리는 비의 양에 따라 일정한 높이 이하의 모든 지점은 물에 잠긴다고 가정한다.

어떤 지역의 높이 정보는 행과 열의 크기가 각각 N인 2차원 배열 형태로 주어지며 배열의 각 원소는 해당 지점의 높이를 표시하는 자연수이다. 예를 들어, 다음은 N=5인 지역의 높이 정보이다.

![](https://velog.velcdn.com/images/sudhdkso/post/4e7557b0-dd14-4087-847c-85c6fcdb18b0/image.png){heigth: 100px, width: 100px}

이제 위와 같은 지역에 많은 비가 내려서 높이가 4 이하인 모든 지점이 물에 잠겼다고 하자. 이 경우에 물에 잠기는 지점을 회색으로 표시하면 다음과 같다. 
![](https://velog.velcdn.com/images/sudhdkso/post/96d9a399-d5b8-4e7f-847a-e497a76a989e/image.png)
물에 잠기지 않는 안전한 영역이라 함은 물에 잠기지 않는 지점들이 위, 아래, 오른쪽 혹은 왼쪽으로 인접해 있으며 그 크기가 최대인 영역을 말한다. 위의 경우에서 물에 잠기지 않는 안전한 영역은 5개가 된다(꼭짓점으로만 붙어 있는 두 지점은 인접하지 않는다고 취급한다). 

또한 위와 같은 지역에서 높이가 6이하인 지점을 모두 잠기게 만드는 많은 비가 내리면 물에 잠기지 않는 안전한 영역은 아래 그림에서와 같이 네 개가 됨을 확인할 수 있다. 
![](https://velog.velcdn.com/images/sudhdkso/post/addf15bc-bf8c-44fa-8fe2-924ff0302dc2/image.png)

이와 같이 장마철에 내리는 비의 양에 따라서 물에 잠기지 않는 안전한 영역의 개수는 다르게 된다. 위의 예와 같은 지역에서 내리는 비의 양에 따른 모든 경우를 다 조사해 보면 물에 잠기지 않는 안전한 영역의 개수 중에서 최대인 경우는 5임을 알 수 있다. 

어떤 지역의 높이 정보가 주어졌을 때, 장마철에 물에 잠기지 않는 안전한 영역의 최대 개수를 계산하는 프로그램을 작성하시오. 


#### Input
첫째 줄에는 어떤 지역을 나타내는 2차원 배열의 행과 열의 개수를 나타내는 수 N이 입력된다. N은 2 이상 100 이하의 정수이다. 둘째 줄부터 N개의 각 줄에는 2차원 배열의 첫 번째 행부터 N번째 행까지 순서대로 한 행씩 높이 정보가 입력된다. 각 줄에는 각 행의 첫 번째 열부터 N번째 열까지 N개의 높이 정보를 나타내는 자연수가 빈 칸을 사이에 두고 입력된다. 높이는 1이상 100 이하의 정수이다.

#### Output
첫째 줄에 장마철에 물에 잠기지 않는 안전한 영역의 최대 개수를 출력한다.

### ***Solution***
``` java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.LinkedList;
import java.util.Queue;

class Point {
    int x;
    int y;
    public Point(int x, int y){
        this.x = x;
        this.y = y;
    }
}

public class Main {
    public static boolean[][] visited;
    public static int[] dx = {1,-1,0,0}, dy = {0,0,1,-1};

    public static int bfs(int a, int b, int rain, int[][] arr){
        int length = arr.length;
        int count = 0;
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
                if(nx >= 0 && ny >= 0 && nx < length && ny < length){
                    if(!visited[nx][ny] && arr[nx][ny] - rain > 0){
                        q.add(new Point(nx, ny));
                        visited[nx][ny] = true;
                        count++;
                    }
                }
            }
        }
        return count;
    }

    public static void main( String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int N = Integer.parseInt(br.readLine());
        int[][] arr = new int[N][N];

        int min = 999,max = -1;

        for(int i=0;i<N;i++){
            String[] s = br.readLine().split(" ");
            for(int j =0;j<N;j++){
                arr[i][j] = Integer.parseInt(s[j]);
                min = Math.min(min, arr[i][j]);
                max = Math.max(max, arr[i][j]);
            }
        }
        min-=1;
        int safeMax = 0;
        for(int i=min;i<=max;i++){
            visited = new boolean[N][N];
            int safeCount = 0;
            for(int n=0;n<N;n++){
                for(int m = 0;m<N;m++){
                    if(!visited[n][m] && arr[n][m] - i > 0){
                        int result = bfs(n,m,i,arr);
                        safeCount++;
                    }
                }
            }
            safeMax = Math.max(safeMax, safeCount);
        }

        bw.write(String.valueOf(safeMax));

        bw.flush();
    }
}
```
- min은 모든 영역이 가라않지 않는 강수량 - 1 이다.
- max는 모든 영역이 가라않는 강수량이다.

- bfs를 활용하여 min부터 max까지의 강수량의 경우 안전지역의 개수를 구한다.
- safeMax는 각 강수량마다 안전지역의 개수를 구한 뒤 최대 안전 지역의 개수를 저장하는 변수이다.

### 출처
https://www.acmicpc.net/problem/2468