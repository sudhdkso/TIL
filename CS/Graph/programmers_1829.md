## **하노이의 탑**


### ***problem***
출판사의 편집자인 어피치는 네오에게 컬러링북에 들어갈 원화를 그려달라고 부탁하여 여러 장의 그림을 받았다. 여러 장의 그림을 난이도 순으로 컬러링북에 넣고 싶었던 어피치는 영역이 많으면 색칠하기가 까다로워 어려워진다는 사실을 발견하고 그림의 난이도를 영역의 수로 정의하였다. (영역이란 상하좌우로 연결된 같은 색상의 공간을 의미한다.)

그림에 몇 개의 영역이 있는지와 가장 큰 영역의 넓이는 얼마인지 계산하는 프로그램을 작성해보자.

위의 그림은 총 12개 영역으로 이루어져 있으며, 가장 넓은 영역은 어피치의 얼굴면으로 넓이는 120이다.

### **입력형식**
입력은 그림의 크기를 나타내는 `m`과 `n`, 그리고 그림을 나타내는 `m × n` 크기의 2차원 배열 `picture`로 주어진다. 제한조건은 아래와 같다.

- `1 <= m, n <= 100`
- `picture`의 원소는 `0` 이상 `2^31 - 1` 이하의 임의의 값이다.
- `picture`의 원소 중 값이 `0`인 경우는 색칠하지 않는 영역을 뜻한다.

### **출력형식**
리턴 타입은 원소가 두 개인 정수 배열이다. 그림에 몇 개의 영역이 있는지와 가장 큰 영역은 몇 칸으로 이루어져 있는지를 리턴한다.

### ***Solution***
``` java
import java.util.*;

class Solution {
    static int[] dx = {1,0,-1,0}, dy = {0,1,0,-1};
    static boolean[][] visited;
    private static class Pair{
        int x,y;
        public Pair(int x, int y){
            this.x = x;
            this.y = y;
        }
    }
    public int[] solution(int m, int n, int[][] picture) {
        int numberOfArea = 0;
        int maxSizeOfOneArea = 0;

        int[] answer = new int[2];
        visited = new boolean[m][n];
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(!visited[i][j] && picture[i][j] != 0){
                    numberOfArea++;
                    int result = getArea(i,j, picture[i][j], m,n,picture);
                    maxSizeOfOneArea = Math.max(maxSizeOfOneArea, result);
                }
            }
        }
        
        answer[0] = numberOfArea;
        answer[1] = maxSizeOfOneArea;
        return answer;
    }
    
    private int getArea(int a, int b, int color, int m, int n, int[][ ] picture){
        Queue<Pair> q = new LinkedList<>();
        q.offer(new Pair(a,b));
        visited[a][b] = true;
        int count = 1;
        while(!q.isEmpty()){
            Pair p = q.poll();
            int x = p.x;
            int y = p.y;
            for(int i=0;i<4;i++){
                int nx = x + dx[i];
                int ny = y + dy[i];
                if(nx >= 0 && ny >= 0 && nx < m && ny < n){
                    if(!visited[nx][ny] && picture[nx][ny] == color){
                        q.offer(new Pair(nx,ny));
                        visited[nx][ny] = true;
                        count++;
                    }
                }
            }
        }
        return count;
    }
}
```
### **문제 풀이** 
- bfs를 활용하여 상하좌우로 연결되어 있는 같은 칸의 넓이 중 가장 큰 넓이와, 갯수를 출력하는 문제이다.





### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/1829