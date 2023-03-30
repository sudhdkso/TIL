## **아이템 줍기**


#### ***problem***
다음과 같은 다각형 모양 지형에서 캐릭터가 아이템을 줍기 위해 이동하려 합니다.

<!-- rect_1.png -->

지형은 각 변이 x축, y축과 평행한 직사각형이 겹쳐진 형태로 표현하며, 캐릭터는 이 다각형의 둘레(굵은 선)를 따라서 이동합니다.

만약 직사각형을 겹친 후 다음과 같이 중앙에 빈 공간이 생기는 경우, 다각형의 가장 바깥쪽 테두리가 캐릭터의 이동 경로가 됩니다.

<!-- rect_2.png -->

단, 서로 다른 두 직사각형의 x축 좌표 또는 y축 좌표가 같은 경우는 없습니다.

<!-- rect_4.png -->

즉, 위 그림처럼 서로 다른 두 직사각형이 꼭짓점에서 만나거나, 변이 겹치는 경우 등은 없습니다.

다음 그림과 같이 지형이 2개 이상으로 분리된 경우도 없습니다.

<!-- rect_3.png -->

한 직사각형이 다른 직사각형 안에 완전히 포함되는 경우 또한 없습니다.

<!-- rect_7.png -->

지형을 나타내는 직사각형이 담긴 2차원 배열 rectangle, 초기 캐릭터의 위치 characterX, characterY, 아이템의 위치 itemX, itemY가 solution 함수의 매개변수로 주어질 때, 캐릭터가 아이템을 줍기 위해 이동해야 하는 가장 짧은 거리를 return 하도록 solution 함수를 완성해주세요.

#### **제한사항**
- rectangle의 세로(행) 길이는 1 이상 4 이하입니다.
- rectangle의 원소는 각 직사각형의 [좌측 하단 x, 좌측 하단 y, 우측 상단 x, 우측 상단 y] 좌표 형태입니다.
    - 직사각형을 나타내는 모든 좌표값은 1 이상 50 이하인 자연수입니다.
    - 서로 다른 두 직사각형의 x축 좌표, 혹은 y축 좌표가 같은 경우는 없습니다.
    - 문제에 주어진 조건에 맞는 직사각형만 입력으로 주어집니다.
- charcterX, charcterY는 1 이상 50 이하인 자연수입니다.
    - 지형을 나타내는 다각형 테두리 위의 한 점이 주어집니다.
- itemX, itemY는 1 이상 50 이하인 자연수입니다.
    - 지형을 나타내는 다각형 테두리 위의 한 점이 주어집니다.
- 캐릭터와 아이템의 처음 위치가 같은 경우는 없습니다.

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
    public static int MAX_VALUE = 100;
    public static int[][] arr = new int[MAX_VALUE][MAX_VALUE], distance = new int[MAX_VALUE][MAX_VALUE];
    public static boolean[][] visited = new boolean[MAX_VALUE][MAX_VALUE];
    public static int[] dx = {1,-1,0,0}, dy = {0,0,1,-1};
    public static int solution(int[][] rectangle, int characterX, int characterY, int itemX, int itemY) {
        int answer = 0;
        //초기화
        for(int i=0;i<MAX_VALUE;i++){
            Arrays.fill(arr[i], 0);
        }

        int length = rectangle.length;
        for(int i=0;i<length;i++){
            int x1 = rectangle[i][0]-1;
            int y1 = rectangle[i][1]-1;
            int x2 = rectangle[i][2]-1;
            int y2 = rectangle[i][3]-1;
            makeRectangle(x1*2, y1*2, x2*2, y2*2);
        }

        bfs(characterX-1,characterY-1, itemX-1, itemY-1, rectangle);
        answer = distance[(itemY-1)*2][(itemX-1)*2];
        return answer/2;
    }

    public static void bfs(int characterX, int characterY, int itemX, int itemY, int[][] rectangle){
        Queue<Point> q = new LinkedList<>();
        q.add(new Point(characterY*2, characterX*2));
        visited[characterY*2][characterX*2] = true;
        distance[characterY*2][characterX*2] = 0;
        while(!q.isEmpty()){
            Point p = q.remove();
            int x = p.x;
            int y = p.y;
            for(int i=0;i<4;i++){
                int nx = x + dx[i];
                int ny = y + dy[i];
                if(nx>=0 && ny >= 0 && nx <= MAX_VALUE && ny <= MAX_VALUE){
                    if(visited[nx][ny] == false && arr[nx][ny] != 0 && !isInAllRect(rectangle, nx, ny)){
                        q.add(new Point(nx, ny));
                        visited[nx][ny] = true;
                        distance[nx][ny] = distance[x][y] + 1;
                    }
                }
            }
        }
    }

    public static void makeRectangle(int x1, int y1, int x2, int y2){
        //사각형 아랫변
        for(int i = x1; i<=x2;i++){
            arr[y1][i] = 1;
        }
        //사각형 윗변
        for(int i = x1; i<=x2;i++){
            arr[y2][i] = 1;
        }
        //사각형 왼쪽변
        for(int i = y1; i<=y2;i++){
            arr[i][x1] = 1;
        }
        //사각형 오른쪽변
        for(int i = y1; i<=y2;i++){
            arr[i][x2] = 1;
        }
        return;
    }

    
    public static boolean isInRect(int x1, int y1, int x2, int y2, int nx, int ny){
        if(y1 < nx && nx < y2 &&  x1 < ny && ny < x2){
            return true;
        }

        return false;
    }

    public static boolean isInAllRect(int[][] rectangle, int nx, int ny){
        int length = rectangle.length;
        for(int i=0;i<length;i++){
            int x1 = rectangle[i][0]-1;
            int y1 = rectangle[i][1]-1;
            int x2 = rectangle[i][2]-1;
            int y2 = rectangle[i][3]-1;
            if(isInRect(x1*2,y1*2,x2*2,y2*2,nx,ny)){
                return true;
            }
        }
        return false;
    }
}
```
- `makeRectangle`함수는 사각형의 좌하단, 우상단 좌표를 받아 배열에 사각형을 만들어주는 함수이다. 
    - 전역적으로 선언된 arr배열에 (x1, y1) (x2, y2)로 만들 수 있는 사각형의 좌,우,위,아래 선분에 해당하는 좌표들의 값을 1로 만들어 준다.
- `isInRect`함수는 사각형의 좌표와 탐색할 좌표를 받아 탐색할 좌표가 사각형의 내부에 포함되어있는지 여부를 반환하는 함수이다.
    - 사각형의 좌표인 (x1, y1) (x2, y2)와 nx, ny를 매개변수로 받는다.
    - y1 < nx < y2 와 x1< ny < x2인 경우 (nx, ny)가 사각형 내부에 있다고 할 수 있다.
- `isInAllRect`는 모든 사각형을 탐색하여 nx, ny가 사각형안에 포함되어있는지 여부를 반환하는 함수이다.
    - 함수 내부에서 `isInRect`함수를 사용하여 하나하나의 사각형과 좌표를 확인하고 그 중 하나라도 포함되어있으면 true를 반환한다.

- 여기서 만들어지는 사각형은 입력된 값의 2배수이다.
    - 사각형이 1배수인경우 실제 사각형의 선분은 떨어져 있으나 배열상에서는 바로 옆에 위치하게 되어 올바르게 사각형을 탐색할 수 없기 때문에 2배수로 만들어 주었다.
- bfs를 활용하여 arr배열을 탐색하여 character가 item으로 이동하는 경로의 거리를 찾는다.
    - (characterY*2, characterX*2) 부터 (i,j)까지의 거리를 distance 저장한다.

- distance[(itemY-1)*2][(itemX-1)*2]/2한 값이 실제
1배수에서 (characterX, characterY) ~ (itemX, itemY)까지의 최소 거리이다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/87694