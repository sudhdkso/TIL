## **혼자 놀기의 달인**


### ***problem***
혼자서도 잘 노는 범희는 어느 날 방구석에 있는 숫자 카드 더미를 보더니 혼자 할 수 있는 재미있는 게임을 생각해냈습니다.

숫자 카드 더미에는 카드가 총 100장 있으며, 각 카드에는 1부터 100까지 숫자가 하나씩 적혀있습니다. 2 이상 100 이하의 자연수를 하나 정해 그 수보다 작거나 같은 숫자 카드들을 준비하고, 준비한 카드의 수만큼 작은 상자를 준비하면 게임을 시작할 수 있으며 게임 방법은 다음과 같습니다.

준비된 상자에 카드를 한 장씩 넣고, 상자를 무작위로 섞어 일렬로 나열합니다. 상자가 일렬로 나열되면 상자가 나열된 순서에 따라 1번부터 순차적으로 증가하는 번호를 붙입니다.

그 다음 임의의 상자를 하나 선택하여 선택한 상자 안의 숫자 카드를 확인합니다. 다음으로 확인한 카드에 적힌 번호에 해당하는 상자를 열어 안에 담긴 카드에 적힌 숫자를 확인합니다. 마찬가지로 숫자에 해당하는 번호를 가진 상자를 계속해서 열어가며, 열어야 하는 상자가 이미 열려있을 때까지 반복합니다.

이렇게 연 상자들은 1번 상자 그룹입니다. 이제 1번 상자 그룹을 다른 상자들과 섞이지 않도록 따로 둡니다. 만약 1번 상자 그룹을 제외하고 남는 상자가 없으면 그대로 게임이 종료되며, 이때 획득하는 점수는 0점입니다.

그렇지 않다면 남은 상자 중 다시 임의의 상자 하나를 골라 같은 방식으로 이미 열려있는 상자를 만날 때까지 상자를 엽니다. 이렇게 연 상자들은 2번 상자 그룹입니다.

1번 상자 그룹에 속한 상자의 수와 2번 상자 그룹에 속한 상자의 수를 곱한 값이 게임의 점수입니다.

상자 안에 들어있는 카드 번호가 순서대로 담긴 배열 `cards`가 매개변수로 주어질 때, 범희가 이 게임에서 얻을 수 있는 최고 점수를 구해서 return 하도록 solution 함수를 완성해주세요.

#### **제한사항**
- 2 ≤ `cards`의 길이 ≤ 100
- `cards`의 원소는 `cards`의 길이 이하인 임의의 자연수입니다.
- `cards`에는 중복되는 원소가 존재하지 않습니다.
- `cards[i]`는 i + 1번 상자에 담긴 카드에 적힌 숫자를 의미합니다.

### ***Solution***
``` java
import java.util.*;

class Solution {
    public static boolean[] check;
    public static ArrayList<Integer> arr = new ArrayList<>();
    
    public static void OpenLinkedBox(int num, int[] cards){
        int index = num;
        int count = 0;
        while(!check[index]){
            count++;
            check[index] = true;
            index = cards[index]-1;
        }
        
        if(count > 0){
            arr.add(count);
        }
    }
    public int solution(int[] cards) {
        int answer = 0;
        check = new boolean[cards.length];
        
        for(int i=0;i<cards.length;i++){
            OpenLinkedBox(i, cards);
        }

        if(arr.size() <= 1){
            answer = 0;
        }
        else{
            Collections.sort(arr,Collections.reverseOrder());
            answer = arr.get(0)*arr.get(1);
        }
        return answer;
    }
}
```
- 모든 경우의 수를 확인하는 완전 탐색 문제이다.
- arr은 그룹마다 연결된 카드의 갯수를 저장하는 ArrayList이다.
- check는 각 박스의 open여부를 확인하는 boolean타입의 일차원 배열이다.
- openLinkedBox 함수는 cards[index]번 부터 연결된 카드의 갯수를 계산하여 arr에 넣어주는 함수이다.
    - 함수내에서 카드를 통해서 이어진 박스를 더이상 열 수 없을 때 까지 while을 통해서 반복한다.
    - 이 과정에서 count를 증가 시킨다.
    - while문을 빠져왔다면 count가 0이상이면 open한 박스가 있는 것이므로 arr에 count를 추가한다.

- for문을 통해 0부터 cards.length까지 차례대로 각 index가 연결된 카드의 갯수를 찾는다.
- 모든 계산이 끝나면 arr의 size가 1이면 0을 return한다.
- arr의 size가 1보다 크면 arr을 내림차순으로 정렬한다.
- 그리고 가장 큰 값과 그 다음 큰 값, 두개의 값을 곱하여 return한다.

### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/131130