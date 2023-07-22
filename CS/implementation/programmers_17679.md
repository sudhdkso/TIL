## **프렌즈4블록**


### ***problem***
블라인드 공채를 통과한 신입 사원 라이언은 신규 게임 개발 업무를 맡게 되었다. 이번에 출시할 게임 제목은 "프렌즈4블록".
같은 모양의 카카오프렌즈 블록이 2×2 형태로 4개가 붙어있을 경우 사라지면서 점수를 얻는 게임이다.

만약 판이 위와 같이 주어질 경우, 라이언이 2×2로 배치된 7개 블록과 콘이 2×2로 배치된 4개 블록이 지워진다. 같은 블록은 여러 2×2에 포함될 수 있으며, 지워지는 조건에 만족하는 2×2 모양이 여러 개 있다면 한꺼번에 지워진다.

블록이 지워진 후에 위에 있는 블록이 아래로 떨어져 빈 공간을 채우게 된다.

만약 빈 공간을 채운 후에 다시 2×2 형태로 같은 모양의 블록이 모이면 다시 지워지고 떨어지고를 반복하게 된다.

위 초기 배치를 문자로 표시하면 아래와 같다.
```
TTTANT
RRFACC
RRRFCC
TRRRAA
TTMMMF
TMMTTJ
```

각 문자는 라이언(R), 무지(M), 어피치(A), 프로도(F), 네오(N), 튜브(T), 제이지(J), 콘(C)을 의미한다

입력으로 블록의 첫 배치가 주어졌을 때, 지워지는 블록은 모두 몇 개인지 판단하는 프로그램을 제작하라.

#### **입력형식**
- 입력으로 판의 높이 `m`, 폭 `n`과 판의 배치 정보 `board`가 들어온다.
- 2 ≦ `n`, `m` ≦ 30
- `board`는 길이 `n`인 문자열 `m`개의 배열로 주어진다. 블록을 나타내는 문자는 대문자 A에서 Z가 사용된다.

### **출력형식**
입력으로 주어진 판 정보를 가지고 몇 개의 블록이 지워질지 출력하라.

### ***Solution***
``` java
import java.util.*;
class Solution {
    static String[][] map;
    static List<Pair> list = new ArrayList<>();
    static int M,N;
    static class Pair{
        int x,y;
        public Pair(int x, int y){
            this.x = x;
            this.y = y;
        }
        
        @Override
        public boolean equals(Object obj){
            Pair other = (Pair)obj;
            if(this.x == other.x && this.y == other.y){
                return true;
            }
            return false;
        }
    }
    
    public int solution(int m, int n, String[] board) {
        int answer = 0;
        M = m; N = n;
        setBlock(board);
        while(true){
            boolean check = checkRemove();
            if(!check){
                break;
            }
            //삭제 할 블럭의 갯수 answer에 추가
            answer += list.size();
            //실제 블럭 삭제
            removeBlock();
            //list비우기
            list.clear();
            //블럭 정리
            fallDownAllBolck();
        }
        return answer;
    }
    //주어진 게임 보드를 2차원 배열로 setting하는 함수
    private static void setBlock(String[] board){
        map = new String[M][N];
        for(int i=0;i<M;i++){
            map[i] = board[i].split("");
        }
    }
    
    //삭제할 블럭이 있는지 확인
    private static boolean checkRemove(){
        boolean check = false;
        for(int i=0;i<M;i++){
            for(int j=0;j<N;j++){
                if(map[i][j].equals(" ")){
                    continue;
                }
                if(isRemove(i,j)){
                    check = true;
                }
            }
        }
        return check;
    }
    
    //블럭 삭제할 수 있는지 확인후 삭제 가능하면 list에 좌표 모두 넣기
    private static boolean isRemove(int x,int y){
        String B = map[x][y];
        int count = 0;
        for(int i=0;i<2;i++){
            int nx = x+i;
            for(int j=0;j<2;j++){
                int ny = y+j;           
                if(nx+i > M || ny+j > N){
                    return false;
                }
                if(!map[nx][ny].equals(B)){
                    return false;
                }
            }
        }
        
        //삭제 가능한 블럭 list에 넣기
        for(int i=0;i<2;i++){
            int nx = x+i;
            for(int j=0;j<2;j++){
                int ny = y+j;
                Pair p = new Pair(nx,ny);
                if(!list.contains(p)){
                    list.add(p);
                }
            }
        }
        return true;
    }
    
    //list에 들어가있는 좌표를 토대로 블럭 삭제
    private static void removeBlock(){
        while(list.size() > 0){
            Pair p = list.remove(0);
            map[p.x][p.y] = " ";
        }
    }
    
    //블럭 삭제 후 떨어트리기
    private static void fallDownAllBolck(){
        for(int j=0;j<N;j++){
            for(int i=M-2;i>=0;i--){
                if(map[i][j].equals(" ")){
                    continue;
                }
                fallDownOneBlock(i,j);
            }
        }
    }
    
    //블럭 하나를 떨어트리는 경우
    private static void fallDownOneBlock(int x, int y){
        String B = map[x][y];
        for(int i=M-1;i>x;i--){
            if(map[i][y].equals(" ")){
                map[i][y] = B;
                map[x][y] = " ";
                return;
            }
        }
    }
    
}
```
- `checkRemove()`는 삭제할 블럭이 있는지 확인하는 함수이다.
- `isRemove()`는 삭제할 블럭들을 매개변수 `(i,j)`를 받아서 `(i,j)`부터 `(i+1,j+1)`까지 4영역이 모두 같은 블럭이면 삭제할 블럭을 저장하는 List에 각 블럭의 좌표를 넣는다.
    - 이때 List에 같은 위치의 블럭이 있으면, list에 넣지 않아야한다.
- `removeBlock()`는 List에 있는 좌표의 위치의 블럭을 삭제하는 함수이다.
- `fallDownAllBolck()`함수는 삭제한 블럭들로 생긴 빈 공간에 위에 있던 블럭들을 떨어트리는 함수이다.
    - `map[M-2][y]`부터 블럭들의 있으면 `fallDownOneBlock()`함수를 통해 빈 공간에 해당 블럭을 옮간다.
        - `M-2`부터 확인하는 이유는 `M-1`이 가장 아래이다. 그래서 `M-1`에 블럭이 있으면 블럭을 떨어트릴 필요가 없기 때문에 `M-2`부터 확인을 해야한다.
    - `fallDownOneBlock()`은 블럭이 있는 위치 `(x,y)`를 매개변수로 받아 `map[M-1][y]`부터 빈 공간이 있는지 확인하고 그 빈공간에 해당 블럭을 넣어주고 `(x,y)`는 빈공간으로 만들어주는 함수이다.
        - `M-1`부터 확인하는 이유는 가장 아래의 공간부터 먼저 채워줘야하기 때문이다.
        - 위에서 부터 확인하게 되면, 블럭이 1차로 떨어지고 난 이후에 생기는 빈공간에 대해서도 계속 다시 블럭을 떨어트려 줘야한다.
        - 그래서 아래에서부터 확인해야 먼저 가장 아래 빈공간으로 떨어질 블럭과 그 블럭이 떨어져서 생기는 빈공간에 대해서도 바로 처리가 가능하다.

- 구현 방법은
1. 삭제할 블럭들이 있는지 확인하는 `checkRemove()`를 먼저 호출한다.
2. 이때 `checkRemove()`의 함수내에서도 삭제할 블럭이 있는지 확인하고 삭제할 블럭이 있으면, 그 블럭들의 위치를 List에 저장하는 `isRemove()`함수를 호출한다.
3. `checkRemove()`의 반환값이 true이면 `removeBlock()`함수를 호출하여 List에 저장되어 있는 블럭들을 공백으로 처리한다.
4. List를 비운다.
5. `fallDownAllBolck()`함수를 호출하여 통해서 삭제 처리되어 중간에 공백에 되어버린 곳을 정리한다.
### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/17679