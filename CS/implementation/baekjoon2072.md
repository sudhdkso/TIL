## **오목**


### ***problem***
<p align="center">
<img src="https://www.acmicpc.net/JudgeOnline/upload/201007/5m.png">
</p>

19x19크기의 바둑판에, 돌을 놓을 좌표가 주어지면 이 게임이 몇 수만에 끝나는 지를 알아보려고 한다. 사용하고자 하는 바둑판의 모양은 위의 그림과 같으며, (1, 1)이 가장 왼쪽 위의 좌표이고 (19, 19)가 가장 오른쪽 아래의 좌표이다. 오목은 흑 또는 백이 5개의 돌을 가로, 세로, 혹은 대각선으로 연속으로 놓았을 경우 게임이 끝나게 된다. 즉, 다음 그림과 같은 경우를 말한다.

<p align="center">
<img src="https://www.acmicpc.net/JudgeOnline/upload/201007/5mm.png">
</p>

게임은 흑이 먼저 시작하며, 한수씩 서로 번갈아 가며 두게 된다. 다음 좌표들과 같이 차례로 돌을 놓으며 게임을 진행한다고 하자. (홀수번째는 흑, 짝수번째는 백)

- 1 - (3,3)
- 2 - (2,3)
- 3 - (3,4)
- 4 -  (2,2)
- 5 - (3,2)
- 6 - (3,1)
- 7 - (3,5)
- 8 - (2,4)
- 9 - (2,5)
- 10 - (2,1)
- 11 - (1,5)
- 12 - (4,1)
- 13 - (4,5)
- 14 - (5,5)
- 15 - (1,4)
- 16 - (5,1)
- 17 - (1,3)
- 18 - (1,1)
- 19 - (5,3)
- 20 - (5,2)
- 21 - (1,2)
- 22 - (5,4)
- 23 - (4,2)
- 24 - (4,4)
- 25 - (4,3)

위의 순서대로 바둑판에 돌을 놓으면 아래의 왼쪽 그림과 같이 된다.

<p align="center">
<img src="https://www.acmicpc.net/JudgeOnline/upload/201007/5mmm.png">
<img src="https://www.acmicpc.net/JudgeOnline/upload/201007/5m2.png">
</p>
그런데 생각해보면, 위의 좌표대로 돌을 놓았을 때 오른쪽 그림처럼 18번째의 돌을 놓는 것으로서 게임이 끝나 버리는 것을 알 수 있다. 이 경우, 답은 18이다.

바둑판에 돌을 놓는 좌표가 입력될 때, 몇 번째 수에서 오목이 끝나는지를 찾는 프로그램을 작성하시오. 
오목을 두다 보면 다음과 같이 돌이 5개를 거치지 않고 6개 이상의 돌이 나란히 놓이는 경우가 발생할 수 있다. 이런 경우에는 승리를 인정하지 않고 오목이 계속된다는 것에 주의하라.
<p align="center">
<img src="https://www.acmicpc.net/JudgeOnline/upload/201007/5m5.png">
</p>

#### **Input**
첫째 줄에 바둑판에 놓이는 돌의 개수 N(1 ≤ N ≤ 100)이 주어진다. 그 다음 N줄에는 놓이는 돌의 좌표들이 차례로 주어진다. (홀수번째는 흑, 짝수번째는 백) 돌을 놓은 곳에 또 돌을 놓는 경우는 없다.

#### **Output**
첫째 줄에 몇 번째 수에서 승패가 갈리는지를 출력한다. 승패가 갈리지 않는 경우에는 -1을 출력한다.

### ***Solution***
``` java
import java.io.*;

public class Main {
    static int M = 20;
    static int[][] arr;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        int N = Integer.parseInt(br.readLine());

        arr = new int[M][M];
        int answer = 0;
        for (int i = 0; i < N; i++) {
            String[] s = br.readLine().split(" ");
            int x = Integer.parseInt(s[0]);
            int y = Integer.parseInt(s[1]);
            arr[x][y] = ((i+1)%2)+1;
            if(i >= 8){
                if(checkWin(x,y,arr[x][y])){
                    answer = i+1;
                    break;
                }
            }
        }
        answer = answer == 0 ? -1 : answer;
        bw.write(answer + "\n");
        bw.flush();
    }

    private static boolean checkWin(int x, int y, int color){
        //가로
        if(checkHoriz(x,y,color)){
            return true;
        }
        //세로
        if(checkVertical(x,y,color)){
            return true;
        }
        if(checkDiagonal(x,y,color)){
            return true;
        }
        //대각선
        return false;
    }
    //가로 확인
    private static boolean checkHoriz(int x, int y, int color){
        int count = 1;
        for(int i=1;i<6;i++){
            if( y+i < M){
                if(arr[x][y+i] != color){
                    break;
                }
                count++;
            }
        }
        for(int i=1;i<6;i++){
            if( y-i >= 0){
                if(arr[x][y-i] != color){
                    break;
                }
                count++;
            }
        }
        if(count >= 6 || count < 5){
            return false;
        }
        return true;
    }
    //세로 확인
    private static boolean checkVertical(int x, int y, int color){
        int count = 1;
        for(int i=1;i<6;i++){
            if( x+i < M){
                if(arr[x+i][y] != color){
                    break;
                }
                count++;
            }
        }
        for(int i=1;i<6;i++){
            if( x-i >= 0){
                if(arr[x-i][y] != color){
                    break;
                }
                count++;
            }
        }
        if(count >= 6 || count < 5){
            return false;
        }
        return true;
    }

    private static boolean checkDiagonal(int x,int y, int color){
        int count = 1;
        //LR대각선
        for(int i=1;i<6;i++){
            if( x+i < M && y+i <M){
                if(arr[x+i][y+i] != color){
                    break;
                }
                count++;
            }
        }
        for(int i=1;i<6;i++){
            if(x-i >= 0 && y-i >= 0){
                if(arr[x-i][y-i] != color){
                    break;
                }
                count++;
            }
        }
        if(count == 5){
            return true;
        }
        count = 1;
        //RL대각선
        for(int i=1;i<6;i++){
            if( x-i >=0 && y+i < M){
                if(arr[x-i][y+i] != color){
                    break;
                }
                count++;
            }
        }
        for(int i=1;i<6;i++){
            if(x+i < M && y-i >= 0){
                if(arr[x+i][y-i] != color){
                    break;
                }
                count++;
            }
        }
        if(count == 5){
            return true;
        }
        return false;
    }
}
```
- `checkWin()`은 해당 돌을 놓았을때 이기는지 판별하여 return해주는 함수이다.
    - `checkHoriz()`는 해당 돌을 기준으로 가로로 돌이 5개가 놓였는지 확인하는 함수이다.
    - `checkVertical()`는 해당 돌을 기준으로 세로로 돌이 5개가 놓여있는지 확인하는 함수이다.
    - `checkDiagonal()`는 해당 돌을 기준으로 좌상대각선 또는 우상대각선으로 돌이 5개가 놓여있는지 확인하는 함수이다.
- `checkWin()`에서 `checkHoriz()`, `checkVertical()`, `checkDiagonal()` 각각의 true,false를 확인하여 셋중 하나라도 true인 경우에는 true를 반환한다.

- 그리고 `checkWin()`이 true가 되는 돌을 놓은 순번을 `answer`에 담고 돌의 입력을 받는 반복문을 종료한다.

- `answer`가 0이면 `checkWin()`이 되는 **순번이 없었다**는 의미이므로 `-1`를 출력하고 `-1`이 아니면 `answer`를 출력한다.

### **[출처]**
https://www.acmicpc.net/problem/2072