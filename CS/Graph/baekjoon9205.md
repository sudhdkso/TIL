## **맥주 마시면서 걸어가기**


### ***problem***
송도에 사는 상근이와 친구들은 송도에서 열리는 펜타포트 락 페스티벌에 가려고 한다. 올해는 맥주를 마시면서 걸어가기로 했다. 출발은 상근이네 집에서 하고, 맥주 한 박스를 들고 출발한다. 맥주 한 박스에는 맥주가 20개 들어있다. 목이 마르면 안되기 때문에 50미터에 한 병씩 마시려고 한다. 즉, 50미터를 가려면 그 직전에 맥주 한 병을 마셔야 한다.

상근이의 집에서 페스티벌이 열리는 곳은 매우 먼 거리이다. 따라서, 맥주를 더 구매해야 할 수도 있다. 미리 인터넷으로 조사를 해보니 다행히도 맥주를 파는 편의점이 있다. 편의점에 들렸을 때, 빈 병은 버리고 새 맥주 병을 살 수 있다. 하지만, 박스에 들어있는 맥주는 20병을 넘을 수 없다. 편의점을 나선 직후에도 50미터를 가기 전에 맥주 한 병을 마셔야 한다.

편의점, 상근이네 집, 펜타포트 락 페스티벌의 좌표가 주어진다. 상근이와 친구들이 행복하게 페스티벌에 도착할 수 있는지 구하는 프로그램을 작성하시오.


#### **Input**
첫째 줄에 테스트 케이스의 개수 t가 주어진다. (t ≤ 50)

각 테스트 케이스의 첫째 줄에는 맥주를 파는 편의점의 개수 n이 주어진다. (0 ≤ n ≤ 100).

다음 n+2개 줄에는 상근이네 집, 편의점, 펜타포트 락 페스티벌 좌표가 주어진다. 각 좌표는 두 정수 x와 y로 이루어져 있다. (두 값 모두 미터, -32768 ≤ x, y ≤ 32767)

송도는 직사각형 모양으로 생긴 도시이다. 두 좌표 사이의 거리는 x 좌표의 차이 + y 좌표의 차이 이다. (맨해튼 거리)

#### **Output**
각 테스트 케이스에 대해서 상근이와 친구들이 행복하게 페스티벌에 갈 수 있으면 "happy", 중간에 맥주가 바닥나서 더 이동할 수 없으면 "sad"를 출력한다. 

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    private static class Point{
        int x,y;
        public Point(int x,int y){
            this.x = x;
            this.y = y;
        }
    }

    public static ArrayList<Point> arrayList = new ArrayList<>();
    public static Queue<Point> q = new LinkedList<>();
    public static boolean[] visited;

    public static boolean bfs(int ex, int ey){
        while(!q.isEmpty()){
            Point p = q.poll();
            int x = p.x;
            int y = p.y;
            if(calcDistance(x,y,ex,ey)) return true;

            for(int i=0;i<arrayList.size();i++){
                Point cp = arrayList.get(i);
                if(!visited[i] && calcDistance(x,y, cp.x,cp.y)) {
                    visited[i] = true;
                    q.add(cp);
                }
            }
        }
        return false;
    }

    public static boolean calcDistance(int x1, int y1,int x2, int y2){
        return Math.abs(x1-x2) + Math.abs(y1-y2) <=1000 ? true : false;
    }

    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));

        int t = Integer.parseInt(br.readLine());
        for(int i=0;i<t;i++){
            int n = Integer.parseInt(br.readLine());
            visited = new boolean[n];
            arrayList = new ArrayList<>();

            String[] s = br.readLine().split(" ");
            int sx = Integer.parseInt(s[0]);
            int sy = Integer.parseInt(s[1]);

            for(int j=0;j<n;j++){
                s = br.readLine().split(" ");
                int cx = Integer.parseInt(s[0]);
                int cy = Integer.parseInt(s[1]);
                arrayList.add(new Point(cx,cy));
            }

            s = br.readLine().split(" ");
            int ex = Integer.parseInt(s[0]);
            int ey = Integer.parseInt(s[1]);

            q.clear();
            q.offer(new Point(sx,sy));
            boolean result = bfs(ex,ey);
            if(result){
                bw.write("happy"+"\n");
            }
            else{
                bw.write("sad"+"\n");
            }
        }
        bw.flush();
    }

}

```
- 20개의 맥주, 1개의 맥주당 50m거리씩 갈 수 있지만, 편의점에 들리거나 하면 무조건 20개를 채우기 때문에 시작위치-편의점, 편의점-편의점 등으로 각 지점의 거리가 1000이하인지만 확인하면 된다.

- calcDistance 두 좌표를 받아 맨해튼 거리 방식으로 거리를 계산한 후 1000이하의 거리일 경우 true 아니면 false를 반환해주는 함수

- 시작점을 전역변수로 선언한 q에 넣어주고 bfs를 활용하여 편의점에 들리면서 목표지점까지 갈 수 있는지 확인한다.

### 출처
https://www.acmicpc.net/problem/9205