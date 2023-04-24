## **게임 맵 최단거리**


### ***problem***
게임 맵의 상태 maps가 매개변수로 주어질 때, 캐릭터가 상대 팀 진영에 도착하기 위해서 지나가야 하는 칸의 개수의 최솟값을 return 하도록 solution 함수를 완성해주세요. 단, 상대 팀 진영에 도착할 수 없을 때는 -1을 return 해주세요.

#### **제한사항**
- maps는 n x m 크기의 게임 맵의 상태가 들어있는 2차원 배열로, n과 m은 각각 1 이상 100 이하의 자연수입니다.
- n과 m은 서로 같을 수도, 다를 수도 있지만, n과 m이 모두 1인 경우는 입력으로 주어지지 않습니다.
- maps는 0과 1로만 이루어져 있으며, 0은 벽이 있는 자리, 1은 벽이 없는 자리를 나타냅니다.
- 처음에 캐릭터는 게임 맵의 좌측 상단인 (1, 1) 위치에 있으며, 상대방 진영은 게임 맵의 우측 하단인 (n, m) 위치에 있습니다.

### ***Solution***
``` java
import java.util.*;

class Point {
    int x;
    int y;
    public Point(int x, int y){
        this.x = x;
        this.y = y;
    }
}

class Solution {
    public static boolean[][] visited;
    public static int[] dx = {0, 0, 1, -1}, dy = {1, -1, 0, 0};
    public static int[][] distance;
    
    public static int solution(int[][] maps) {
        int answer = 0;
        int n = maps.length;
        int m = maps[0].length;

        distance = new int[n][m];
        visited = new boolean[n][m];

        bfs(maps, n ,m);
        
        if(distance[n-1][m-1] > 0){
            answer = distance[n-1][m-1];
        }
        else {
            answer = -1;
        }
        return answer;
    }
    public static void bfs(int[][] maps, int n, int m) {
        Queue<Point> q = new LinkedList<>();
        q.add(new Point(0,0));
        visited[0][0] = true;
        distance[0][0] = 1;
        while (!q.isEmpty()){
            Point p = q.remove();
            int x = p.x;
            int y = p.y;

            for(int i=0;i<4;i++){
                int nx = x + dx[i];
                int ny = y + dy[i];

                if(nx >=0 && ny >=0 && nx < n && ny < m){
                    if(visited[nx][ny] == false && maps[nx][ny] != 0){
                        q.add(new Point(nx, ny));
                        visited[nx][ny] = true;
                        distance[nx][ny] = distance[x][y] + 1;
                    }
                }
            }
        }

    }
}
```
- visited는 (i , j)의 방문여부를 확인하는 boolean타입의 이차원 배열이다.
- distance는 (0 , 0) ~ (i , j)까지의 거리를 저장하는 int타입의 이차원 배열이다.

- bfs를 활용하여 그래프를 탐색한 후 distance를 체크한다.
    -  적진의 좌표인 (n-1, m-1)이 0이면 적진까지 방문 불가능하기 때문에 answer에 -1을 저장한다. 
    - 그게 아니면 방문 가능한 경우이기에  answer를 distance[n-1][m-1]값을 저장한다.


### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/1844