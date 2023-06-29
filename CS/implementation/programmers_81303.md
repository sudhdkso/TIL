## **표 편집**


#### ***problem***
[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]

업무용 소프트웨어를 개발하는 니니즈웍스의 인턴인 앙몬드는 명령어 기반으로 표의 행을 선택, 삭제, 복구하는 프로그램을 작성하는 과제를 맡았습니다. 세부 요구 사항은 다음과 같습니다

<p align="left">
<img width="200px" src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d8e89054-53ba-4222-a485-dc56893f45e4/table_1.png"/>
</p>

위 그림에서 파란색으로 칠해진 칸은 현재 **선택된 행**을 나타냅니다. 단, 한 번에 한 행만 선택할 수 있으며, 표의 범위(0행 ~ 마지막 행)를 벗어날 수 없습니다. 이때, 다음과 같은 명령어를 이용하여 표를 편집합니다.

- `"U X"`: 현재 선택된 행에서 X칸 위에 있는 행을 선택합니다.
- `"D X"`: 현재 선택된 행에서 X칸 아래에 있는 행을 선택합니다.
- `"C"` : 현재 선택된 행을 삭제한 후, 바로 아래 행을 선택합니다. 단, 삭제된 행이 가장 마지막 행인 경우 바로 윗 행을 선택합니다.
- `"Z"` : 가장 최근에 삭제된 행을 원래대로 복구합니다. <u>**단, 현재 선택된 행은 바뀌지 않습니다.**</u>

예를 들어 위 표에서 `"D 2"`를 수행할 경우 아래 그림의 왼쪽처럼 4행이 선택되며, `"C"`를 수행하면 선택된 행을 삭제하고, 바로 아래 행이었던 "네오"가 적힌 행을 선택합니다(4행이 삭제되면서 아래 있던 행들이 하나씩 밀려 올라오고, 수정된 표에서 다시 4행을 선택하는 것과 동일합니다).

<p align="left">
<img width="400px" src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/453bbb71-df69-4be2-a223-67361878202c/table_2.png"/>
</p>

다음으로 `"U 3"`을 수행한 다음 `"C"`를 수행한 후의 표 상태는 아래 그림과 같습니다.

<p align="left">
<img width="400px" src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/61261fa2-701d-4db5-9aa2-a56dd85a3dbf/table_3.png"/>
</p>

다음으로 `"D 4"`를 수행한 다음 `"C"`를 수행한 후의 표 상태는 아래 그림과 같습니다. 5행이 표의 마지막 행 이므로, 이 경우 바로 윗 행을 선택하는 점에 주의합니다.

<p align="left">
<img width="400px" src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/b1a63278-be97-4e3a-a653-5a6aa0f477ba/table_4.png"/>
</p>

다음으로 `"U 2"`를 수행하면 현재 선택된 행은 2행이 됩니다.

<p align="left">
<img width="200px" src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/b1189eff-e4ee-4119-bb55-a1f06e388c29/table_5.png"/>
</p>


위 상태에서 `"Z"`를 수행할 경우 가장 최근에 제거된 `"라이언"`이 적힌 행이 원래대로 복구됩니다.

<p align="left">
<img width="200px" src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/0a386d19-0391-46a7-8086-9f36db31940d/table_6.png"/>
</p>


다시한번 `"Z"`를 수행하면 그 다음으로 최근에 제거된 `"콘"`이 적힌 행이 원래대로 복구됩니다. 이때, 현재 선택된 행은 바뀌지 않는 점에 주의하세요.

<p align="left">
<img width="200px" src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/0a386d19-0391-46a7-8086-9f36db31940d/table_6.png"/>
</p>

이때, 최종 표의 상태와 처음 주어진 표의 상태를 비교하여 삭제되지 않은 행은 `"O"`, 삭제된 행은 `"X"`로 표시하면 다음과 같습니다.

<p align="left">
<img width="200px" src="https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/87a31aeb-50fb-4c0d-9f6b-8427632b582e/table_8.png"/>
</p>

처음 표의 행 개수를 나타내는 정수 n, 처음에 선택된 행의 위치를 나타내는 정수 k, 수행한 명령어들이 담긴 문자열 배열 cmd가 매개변수로 주어질 때, 모든 명령어를 수행한 후 표의 상태와 처음 주어진 표의 상태를 비교하여 삭제되지 않은 행은 O, 삭제된 행은 X로 표시하여 문자열 형태로 return 하도록 solution 함수를 완성해주세요.

#### **제한사항**
- 5 ≤ `n` ≤ 1,000,000
- 0 ≤ `k` < `n`
- 1 ≤ `cmd`의 원소 개수 ≤ 200,000
    - `cmd`의 각 원소는 `"U X"`, `"D X"`, `"C"`, `"Z"` 중 하나입니다.
    - X는 1 이상 300,000 이하인 자연수이며 0으로 시작하지 않습니다.
    - X가 나타내는 자연수에 ',' 는 주어지지 않습니다. 예를 들어 123,456의 경우 123456으로 주어집니다.
    - `cmd`에 등장하는 모든 X들의 값을 합친 결과가 1,000,000 이하인 경우만 입력으로 주어집니다.
    - 표의 모든 행을 제거하여, 행이 하나도 남지 않는 경우는 입력으로 주어지지 않습니다.
    - 본문에서 각 행이 제거되고 복구되는 과정을 보다 자연스럽게 보이기 위해 `"이름"` 열을 사용하였으나, `"이름"`열의 내용이 실제 문제를 푸는 과정에 필요하지는 않습니다. `"이름`"열에는 서로 다른 이름들이 중복없이 채워져 있다고 가정하고 문제를 해결해 주세요.
- 표의 범위를 벗어나는 이동은 입력으로 주어지지 않습니다.
- 원래대로 복구할 행이 없을 때(즉, 삭제된 행이 없을 때) "Z"가 명령어로 주어지는 경우는 없습니다.
- 정답은 표의 0행부터 n - 1행까지에 해당되는 O, X를 순서대로 이어붙인 문자열 형태로 return 해주세요.

### ***Solution***
``` java
import java.util.*;
class Solution {
    static int[] prv, next;
    static Stack<Node>stack = new Stack<>();
    private static class Node{
        int prv,cur,next;
        public Node(int prv, int cur, int next){
            this.prv = prv;
            this.cur = cur;
            this.next = next;
        }
    }
    public String solution(int n, int k, String[] cmd) {
        String answer = "";
        int index = k;
        StringBuilder sb = new StringBuilder();
        sb.append("O".repeat(n));
        setLinkedList(n);    
        for(String c: cmd){
            String[] s = c.split(" ");
            String command = s[0];

            if(command.equals("U")){
                int num = Integer.parseInt(s[1]);
                while(num-- > 0){
                    index = prv[index];
                }
            }
            if(command.equals("D")){
                int num = Integer.parseInt(s[1]);
                while(num-- > 0){
                    index = next[index];
                }
            }
            if(command.equals("C")){ 
                //삭제 목록에 추가
                stack.push(new Node(prv[index],index,next[index]));
                sb.setCharAt(index,'X');
                
                if(prv[index] == -1){
                    prv[next[index]] = -1;
                }
                else if(next[index] == -1){
                    next[prv[index]] = -1;
                }
                else{
                    next[prv[index]] = next[index];
                    prv[next[index]] = prv[index];
                }              
                index = next[index] == -1 ? prv[index] : next[index];
            }
            if(command.equals("Z")){
                Node node = stack.pop();
                sb.setCharAt(node.cur,'O');
                if(node.prv == -1){
                    prv[node.next] = node.cur;
                }
                else if(node.next == -1){
                    next[node.prv] =node.cur;
                }
                else{
                    next[node.prv] = node.cur;
                    prv[node.next] = node.cur;
                }
            }
        }
        answer = sb.toString();
        return answer;
    }
    
    private static void setLinkedList(int n){
        prv = new int[n];
        next = new int[n];
        for(int i=0;i<n;i++){
            prv[i] = i-1;
            next[i] = i+1;
        }
        prv[0] = next[n-1] = -1;
    }
}
```
- LinkedList를 구현하여, 표에서 해당 행을 삭제할 때의 위치를 가지고 있을 수 있도록 한다.
- index는 현재 행의 위치를 의미한다.
- LinkedList의 가장 처음 노드와 가장 마지막 노드의 위치를 주의하여 구현해야한다.
### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/81303