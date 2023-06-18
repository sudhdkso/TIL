## **체스**


### ***problem***
n×m 크기의 체스 판과, 상대팀의 Queen, Knight, Pawn의 위치가 주어져 있을 때, 안전한 칸이 몇 칸인지 세는 프로그램을 작성하시오. (안전한 칸이란 말은 그 곳에 자신의 말이 있어도 잡힐 가능성이 없다는 것이다.)

<img src="https://www.acmicpc.net/JudgeOnline/upload/201007/asdf.png" />

참고로 Queen은 가로, 세로, 대각선으로 갈 수 있는 만큼 최대한 많이 이동을 할 수 있는데 만약 그 중간에 장애물이 있다면 이동을 할 수 없다. 그리고 Knight는 2×3 직사각형을 그렸을 때, 반대쪽 꼭짓점으로 이동을 할 수 있다. 아래 그림과 같은 8칸이 이에 해당한다.

<img src="https://www.acmicpc.net/JudgeOnline/upload/201007/qazwqszx.png" />

이때 Knight는 중간에 장애물이 있더라도 이동을 할 수 있다. 그리고 Pawn은 상대팀의 말은 잡을 수 없다고 하자(즉, 장애물의 역할만 한다는 것이다).

예를 들어 다음과 같이 말이 배치가 되어 있다면 진하게 표시된 부분이 안전한 칸이 될 것이다. (K : Knight, Q : Queen, P : Pawn)

#### **Input**
첫째 줄에는 체스 판의 크기 n과 m이 주어진다. (1 ≤ n, m ≤ 1000) 그리고 둘째 줄에는 Queen의 개수와 그 개수만큼의 Queen의 위치가 입력된다. 그리고 마찬가지로 셋째 줄에는 Knight의 개수와 위치, 넷째 줄에는 Pawn의 개수와 위치가 입력된다. 즉 둘째 줄, 셋째 줄, 넷째 줄은  k, r1, c1, r2, c2, ..., rk, ck 이 빈 칸을 사이에 두고 주어진다는 것이다. 여기서 ri는 i번째 말의 행 위치, ci는 i번째 말의 열 위치를 의미한다. 한 칸에는 하나의 말만 놓인다고 가정한다. Knight, Queen, Pawn의 개수는 각각 100을 넘지 않는 음이 아닌 정수이다.

#### **Output**
첫째 줄에 n×m 체스판에 안전한 칸이 몇 칸인지 출력하시오.

### ***Solution***
``` java
import java.io.*;
import java.util.*;

public class Main {
    static int[][] chess;
    static int N,M;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        chess = new int[N][M];
        String[] queen = br.readLine().split(" ");
        String[] knight = br.readLine().split(" ");
        String[] pawn = br.readLine().split(" ");
        if(Integer.parseInt(pawn[0]) > 0){
            setPawn(pawn);
        }
        if(Integer.parseInt(knight[0]) > 0){
            setKinght(knight);
        }
        if(Integer.parseInt(queen[0]) > 0){
            setQueen(queen);
        }
        int answer = calcSafe();
        bw.write(answer+"\n");
        bw.flush();
    }
    private static int calcSafe(){
        int count = 0;
        for(int i=0;i<N;i++){
            for(int j=0;j<M;j++){
                if(chess[i][j] == 0){
                    count++;
                }
            }
        }
        return count;
    }
    private static void setPawn(String[] pawn){
        int len = pawn.length-1;
        //1,2, 3,4
        int index = 1;
        while(index < len){
            int x = Integer.parseInt(pawn[index++])-1;
            int y = Integer.parseInt(pawn[index++])-1;
            chess[x][y] = 2;
        }
    }

    private static void setKinght(String[] knight){
        int len = knight.length-1;
        int[] dx = {-2,-1,-2,-1,2,1,2,1},dy = {1,2,-1,-2,1,2,-1,-2};
        int index = 1;

        while(index < len){
            int x = Integer.parseInt(knight[index++])-1;
            int y = Integer.parseInt(knight[index++])-1;
            chess[x][y] = 2;
            for(int i=0;i<8;i++){
                int nx = x + dx[i];
                int ny = y + dy[i];
                if(nx>=0 && ny>=0 && nx < N && ny < M){
                    if(chess[nx][ny] == 0){
                        chess[nx][ny] = 1;
                    }
                }
            }
        }
    }

    private static void setQueen(String[] queen){
        int len = queen.length-1;

        int index = 1;
        int[] dx = {-1, -1, 0, 1, 1, 1, 0, -1};
        int[] dy = {0, 1, 1, 1, 0, -1, -1, -1};
        while(index < len){
            int x = Integer.parseInt(queen[index++])-1;
            int y = Integer.parseInt(queen[index++])-1;
            chess[x][y] = 2;
            for(int i=0;i<8;i++){
                int nx = x + dx[i];
                int ny = y + dy[i];
                while(nx >= 0 &&ny >= 0 && nx < N &&ny < M){
                    if(chess[nx][ny] == 2){
                        break;
                    }
                    chess[nx][ny]=1;
                    nx+=dx[i];
                    ny+=dy[i];
                }
            }
        }
    }


}
```
- 폰, 나이트, 퀸 순서로 체스판에 배치한다.
    - 폰은 그저 장애물의 역할이기 때문에 가장 먼저 배치하고 나이트는 장애물에 구애받지 않고 움직일 수 있기 때문에 2번째로, 퀸은 가장 장애물에 많이 구애받기 때문에, 모든 장애물이 배치되고 난 후에 배치한다.
- setPawn()은 입력받은 폰의 위치를 통해서 pawn을 체스에 배치하는 함수이다.
- setKnight()는 입력받은 나이트의 위치를 통해서 knight를 체스에 배치하는 함수이다.
    - knight를 체스에 배치하고 knight가 움직일 수 있는 부분도 체크해준다.
    - 이떄 knight는 chess에 2로 표기하고, knight가 움직일 수 있는 영역은 1로 표기한다.
    - 2는 장애물이 될 수 있는 기물이기 때문에 2로 표기하고, 1은 안전영역은 아니지만 장애물은 아니기에 다른 기물이 움직일 수 있는 영역이 될 수 있기 때문에 구분해준다.
    - 움직일 수 있는 영역은 해당 칸이 안전 영역일때만 체크한다.
- setQueen()은 입력받은 퀸의 위치를 통해서 Queen을 체스에 배치하는 함수이다.
    - Queen을 배치하고 Queen이 움직일 수 있는 영역도 체크해준다.
    - Queen의 경우 한쪽으로 이동하다 이동 경로에 장애물이 있으면 이동 방향을 변경해준다.
- calcSafe()는 모든 기물과 이동 영역을 체크한 chess판에서 안전 영역의 갯수를 체크하여 return하는 함수이다. 


### **[출처]**
https://www.acmicpc.net/problem/1986