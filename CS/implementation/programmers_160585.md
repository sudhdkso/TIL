## **혼자서 하는 틱택토**


### ***problem***
틱택토는 두 사람이 하는 게임으로 처음에 3x3의 빈칸으로 이루어진 게임판에 선공이 "O", 후공이 "X"를 번갈아가면서 빈칸에 표시하는 게임입니다. 가로, 세로, 대각선으로 3개가 같은 표시가 만들어지면 같은 표시를 만든 사람이 승리하고 게임이 종료되며 9칸이 모두 차서 더 이상 표시를 할 수 없는 경우에는 무승부로 게임이 종료됩니다.

할 일이 없어 한가한 머쓱이는 두 사람이 하는 게임인 틱택토를 다음과 같이 혼자서 하려고 합니다.

- 혼자서 선공과 후공을 둘 다 맡는다.
- 틱택토 게임을 시작한 후 "O"와 "X"를 혼자서 번갈아 가면서 표시를 하면서 진행한다.

틱택토는 단순한 규칙으로 게임이 금방 끝나기에 머쓱이는 한 게임이 종료되면 다시 3x3 빈칸을 그린 뒤 다시 게임을 반복했습니다. 그렇게 틱택토 수 십 판을 했더니 머쓱이는 게임 도중에 다음과 같이 규칙을 어기는 실수를 했을 수도 있습니다.

- "O"를 표시할 차례인데 "X"를 표시하거나 반대로 "X"를 표시할 차례인데 "O"를 표시한다.
- 선공이나 후공이 승리해서 게임이 종료되었음에도 그 게임을 진행한다.

게임 도중 게임판을 본 어느 순간 머쓱이는 본인이 실수를 했는지 의문이 생겼습니다. 혼자서 틱택토를 했기에 게임하는 과정을 지켜본 사람이 없어 이를 알 수는 없습니다. 그러나 게임판만 봤을 때 실제로 틱택토 규칙을 지켜서 진행했을 때 나올 수 있는 상황인지는 판단할 수 있을 것 같고 문제가 없다면 게임을 이어서 하려고 합니다.

머쓱이가 혼자서 게임을 진행하다 의문이 생긴 틱택토 게임판의 정보를 담고 있는 문자열 배열 `board`가 매개변수로 주어질 때, 이 게임판이 규칙을 지켜서 틱택토를 진행했을 때 나올 수 있는 게임 상황이면 1을 아니라면 0을 return 하는 solution 함수를 작성해 주세요.

### **제한사항**
- `board`의 길이 = `board[i]`의 길이 = 3
    - `board`의 원소는 모두 "O", "X", "."으로만 이루어져 있습니다.
- `board[i][j]`는 `i` + 1행 `j` + 1열에 해당하는 칸의 상태를 나타냅니다.
    - "."은 빈칸을, "O"와 "X"는 해당 문자로 칸이 표시되어 있다는 의미입니다.

### ***Solution***
``` java
class Solution {
    static int N = 3;
    public int solution(String[] board) {
        int answer = -1;
        int[] count = calcOX(board);
        if(count[0] < count[1]){
            return 0;
        }
        if(count[0]-count[1] != 0 && count[0]-count[1] != 1){
            return 0;
        }
        boolean oWin = isWin(board,"O");
        boolean xWin = isWin(board,"X");
        //둘다 승리일 경우
        if(oWin && xWin){
            return 0;
        }
        if(oWin){
            return count[0]-count[1] == 1 ? 1 : 0;
        }
        if(xWin){
            return count[0]-count[1] == 0 ? 1 : 0;
        }
        

        return 1;
    }
    
    private static int[] calcOX(String[] board){
        int[] count = new int[2];
        for(int i=0;i<N;i++){
            count[0] += N-board[i].replace("O","").length();
            count[1] += N-board[i].replace("X","").length();
        }
        return count;
    }
    
    private static boolean isWin(String[] board, String target){
        //가로
        for(int i=0;i<N;i++){
            int count = N-board[i].replace(target,"").length();
            if(count == N){
                return true;
            }
        }
        
        //세로
        for(int i=0;i<N;i++){
            int count = 1;
            String s = board[0].substring(i,i+1);
            if(!s.equals(target)){
                continue;
            }
            for(int j=1;j<N;j++){
                if(board[j].substring(i,i+1).equals(target)){
                    count++;
                }
            }
            if(count == N){
                return true;
            }
        }
        //대각선
        if(board[0].substring(0,1).equals(target)){
            int count = 0;
            for(int i=0;i<N;i++){
                if(board[i].substring(i,i+1).equals(target)){
                    count++;
                }
            }
            if(count == N){
                return true;
            }
        }
        if(board[0].substring(N-1,N).equals(target)){
            int count = 0;
            for(int i=N-1;i>=0;i--){
                if(board[(N-1)-i].substring(i,i+1).equals(target)){
                    count++;
                }
            }
            if(count == N){
                return true;
            }
        }
        return false;
    }
}
```
- 틱택톡 게임에서 규칙이 제대로 적용되어있는지 확인하는 경우는 크게 네가지가 있다.
1. O와 X의 갯수차이가 0 or 1인지
    - O를 선공으로 게임을 규칙대로 진행했다면 O와 X의 차이는 반드시 0개 또는 1개 차이가 나야한다.
2. O와 X가 모두 빙고를 이루는지
    - O와 X가 동시에 빙고를 이루는 일은 있을 수 없다.
3. O가 빙고를 이룰 때 O의 갯수가 X의 갯수보다 1개 많은지
    - O가 선공이기 때문에 O가 빙고를 이룰 때는 반드시 O의 갯수가 X의 갯수보다 1개 많아야한다.
4. X가 빙고를 이룰 때 X의 갯수가 O의 갯수와 동일한지
    - X는 후공이기 때문에 X가 빙고를 이룰 때는 O와 X의 갯수가 동일해야한다.

- 위의 네가지 경우에 따라 규칙을 잘 적용했는지 확인한다.
- calcOX는 먼저 O와 X의 갯수를 세어 배열로 return하는 함수이다.
- isWin은 target으로 O또는 X를 받아 각각 O와 X가 빙고를 이루었는지 boolean타입으로 return하는 함수이다.
    - 가로, 세로, 대각선을 각각 확인한다.
### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/160585#