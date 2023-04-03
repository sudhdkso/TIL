## 행렬 테두리 회전하기

### ***problem***
rows x columns 크기인 행렬이 있습니다. 행렬에는 1부터 rows x columns까지의 숫자가 한 줄씩 순서대로 적혀있습니다. 이 행렬에서 직사각형 모양의 범위를 여러 번 선택해, 테두리 부분에 있는 숫자들을 시계방향으로 회전시키려 합니다. 각 회전은 (x1, y1, x2, y2)인 정수 4개로 표현하며, 그 의미는 다음과 같습니다.

x1 행 y1 열부터 x2 행 y2 열까지의 영역에 해당하는 직사각형에서 테두리에 있는 숫자들을 한 칸씩 시계방향으로 회전합니다.
다음은 6 x 6 크기 행렬의 예시입니다.

|1|2|3|4|5|6|
|---|---|---|---|---|---|
|7|8|9|10|11|12|
|13|14|15|16|17|18|
|19|20|21|22|23|24|
|25|26|27|28|29|30|
|31|32|33|34|35|36|

이 행렬에 (2, 2, 5, 4) 회전을 적용하면, 아래 그림과 같이 2행 2열부터 5행 4열까지 영역의 테두리가 시계방향으로 회전합니다. 이때, 중앙의 15와 21이 있는 영역은 회전하지 않는 것을 주의하세요.

|1|2|3|4|5|6|
|---|---|---|---|---|---|
|7|**14**|**8**|**9**|11|12|
|13|**20**|15|**10**|17|18|
|19|**26**|21|**16**|23|24|
|25|**27**|**28**|**22**|29|30|
|31|32|33|34|35|36|

행렬의 세로 길이(행 개수) rows, 가로 길이(열 개수) columns, 그리고 회전들의 목록 queries가 주어질 때, 각 회전들을 배열에 적용한 뒤, 그 회전에 의해 위치가 바뀐 숫자들 중 가장 작은 숫자들을 순서대로 배열에 담아 return 하도록 solution 함수를 완성해주세요.

제한사항
- rows는 2 이상 100 이하인 자연수입니다.
c- olumns는 2 이상 100 이하인 자연수입니다.
- 처음에 행렬에는 가로 방향으로 숫자가 1부터 하나씩 증가하면서 적혀있습니다.
    - 즉, 아무 회전도 하지 않았을 때, i 행 j 열에 있는 숫자는 ((i-1) x columns + j)입니다.
- queries의 행의 개수(회전의 개수)는 1 이상 10,000 이하입니다.
- queries의 각 행은 4개의 정수 [x1, y1, x2, y2]입니다.
    - x1 행 y1 열부터 x2 행 y2 열까지 영역의 테두리를 시계방향으로 회전한다는 뜻입니다.
    - 1 ≤ x1 < x2 ≤ rows, 1 ≤ y1 < y2 ≤ columns입니다.
    - 모든 회전은 순서대로 이루어집니다.
    - 예를 들어, 두 번째 회전에 대한 답은 첫 번째 회전을 실행한 다음, 그 상태에서 두 번째 회전을 실행했을 때 이동한 숫자 중 최솟값을 구하면 됩니다.

### ***Solution***

```java
import java.util.Vector;
class Solution {
    static int[][] arr = new int[101][101];
    public int[] solution(int rows, int columns, int[][] queries) {
        int[] answer = new int[queries.length];
        int index = 0;

        for(int i=1;i<rows+1;i++)
            for(int j=1;j<columns+1;j++)
                arr[i][j] = ++index;

        for(int i=0;i<queries.length;i++){
            int x1 = queries[i][0]; int y1 = queries[i][1];
            int x2 = queries[i][2]; int y2 = queries[i][3];

            Vector<Integer> tempList = new Vector<>();
            for(int y = y1; y < y2; y++)
                tempList.add(arr[x1][y]);
            for(int x = x1;x < x2; x++)
                tempList.add(arr[x][y2]);
            for(int y = y2; y > y1; y--)
                tempList.add(arr[x2][y]);
            for(int x = x2; x > x1; x--)
                tempList.add(arr[x][y1]);

            answer[i] = tempList.stream().min(Integer::compare).orElse(-1);
            tempList = move(tempList);
            index = 0;
            for(int y = y1; y < y2; y++)
                arr[x1][y] = tempList.elementAt(index++);
            for(int x = x1;x < x2; x++)
                arr[x][y2] = tempList.elementAt(index++);
            for(int y = y2; y > y1; y--)
                arr[x2][y] = tempList.elementAt(index++);
            for(int x = x2; x > x1; x--)
                arr[x][y1] = tempList.elementAt(index++);
        }
        return answer;
    }

    public static Vector<Integer> move(Vector<Integer> tempList){
        tempList.add(0,tempList.lastElement());
        tempList.removeElementAt(tempList.size()-1);
        return tempList;
    }
}
```
- solution함수에서 arr을 입력받은 row와 col만큼 행렬의 숫자로 채워준다.
- solution함수에서 tempList는 입력한 영역의 행렬 테두리를 저장하는 임의의 Vector이다.
- solution함수에서 tempList에 행렬 테두리를 저장할 때 순서는 [y1,y2), [x1,x2),[y2,y1),[x2,x1) 순서이다.
    - 이 순서로 해야지 숫자가 올바른 순서로 tempList에 추가가 된다.
- 그 구역에서 최솟값은 tempList내에 있으므로 tempList에서 stream의 min함수를 사용하여 최솟값을 answer에 저장한다.

- move함수는 입력한 영역의 행렬 테두리를 시계방향으로 회전시켜주는 함수이다.
- move함수에서 매개변수로 받은 tempList의 맨 앞에 맨마지막 원소를 추가한다.
- 그리고 tempList의 맨마지막 원소를 삭제한다.
- 이런식으로 하면 숫자가 오른쪽으로 한칸씩 밀린다.

- solution함수에서 이동한 tempList의 첫번째부터 아까 tempList에 저장했던 순서대로 값을 수정해준다.
### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/77485