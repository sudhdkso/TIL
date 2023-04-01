## **무인도 여행**


#### ***problem***
메리는 여름을 맞아 무인도로 여행을 가기 위해 지도를 보고 있습니다. 지도에는 바다와 무인도들에 대한 정보가 표시돼 있습니다. 지도는 1 x 1크기의 사각형들로 이루어진 직사각형 격자 형태이며, 격자의 각 칸에는 'X' 또는 1에서 9 사이의 자연수가 적혀있습니다. 지도의 'X'는 바다를 나타내며, 숫자는 무인도를 나타냅니다. 이때, 상, 하, 좌, 우로 연결되는 땅들은 하나의 무인도를 이룹니다. 지도의 각 칸에 적힌 숫자는 식량을 나타내는데, 상, 하, 좌, 우로 연결되는 칸에 적힌 숫자를 모두 합한 값은 해당 무인도에서 최대 며칠동안 머물 수 있는지를 나타냅니다. 어떤 섬으로 놀러 갈지 못 정한 메리는 우선 각 섬에서 최대 며칠씩 머물 수 있는지 알아본 후 놀러갈 섬을 결정하려 합니다.

지도를 나타내는 문자열 배열 maps가 매개변수로 주어질 때, 각 섬에서 최대 며칠씩 머무를 수 있는지 배열에 오름차순으로 담아 return 하는 solution 함수를 완성해주세요. 만약 지낼 수 있는 무인도가 없다면 -1을 배열에 담아 return 해주세요.

#### **제한사항**
- 3 ≤ `maps`의 길이 ≤ 100
    - 3 ≤ `maps[i]`의 길이 ≤ 100
    - `maps[i]`는 'X' 또는 1 과 9 사이의 자연수로 이루어진 문자열입니다.
    - 지도는 직사각형 형태입니다.
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
    public static int[] dx = {1,-1,0,0}, dy = {0,0,1,-1};
    public static int[] solution(String[] maps) {
        int[] answer;
        int n = maps.length;
        int m = maps[0].length();

        int[][] arr = new int[n][m];
        boolean[][] visited = new boolean[n][m];

        for(int i=0;i< n;i++){
            for(int j=0;j<m;j++){
                String s = maps[i].substring(j,j+1);
                if(s.equals("X")){
                    arr[i][j] = 0;
                }
                else {
                    arr[i][j] = Integer.parseInt(s);
                }
            }
        }
        int count = 0;
        ArrayList<Integer> list = new ArrayList<>();
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(!visited[i][j] && arr[i][j]>0){
                    list.add(bfs(i,j,arr,visited,n,m));
                    count++;
                }
            }
        }

        if(list.size() > 0){
            Collections.sort(list);

            answer = new int[count];

            for(int i=0;i<count;i++){
                answer[i] = list.get(i);
            }
        }
        else {
           answer = new int[1];
           answer[0] = -1;
        }

        return answer;
    }

    public static int bfs(int a, int b, int[][] arr, boolean[][] visited, int n, int m){
        Queue<Point> q = new LinkedList<>();
        q.add(new Point(a,b));

        visited[a][b] = true;

        int result = arr[a][b];

        while(!q.isEmpty()){
            Point p = q.remove();
            int x = p.x;
            int y = p.y;
            for(int i=0;i<4;i++){
                int nx = x + dx[i];
                int ny = y + dy[i];
                if(nx >= 0 && ny >= 0 && nx < n && ny < m){
                    if(!visited[nx][ny] && arr[nx][ny] > 0){
                        q.add(new Point(nx, ny));
                        visited[nx][ny] = true;
                        result+=arr[nx][ny];
                    }
                }
            }
        }
        return result;
    }
}
```
-  

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/87946