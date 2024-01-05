## **과제 진행하기**


#### ***problem***
과제를 받은 루는 다음과 같은 순서대로 과제를 하려고 계획을 세웠습니다.

- 과제는 시작하기로 한 시각이 되면 시작합니다.
- 새로운 과제를 시작할 시각이 되었을 때, 기존에 진행 중이던 과제가 있다면 진행 중이던 과제를 멈추고 새로운 과제를 시작합니다.
- 진행중이던 과제를 끝냈을 때, 잠시 멈춘 과제가 있다면, 멈춰둔 과제를 이어서 진행합니다.
    - 만약, 과제를 끝낸 시각에 새로 시작해야 되는 과제와 잠시 멈춰둔 과제가 모두 있다면, 새로 시작해야 하는 과제부터 진행합니다.
- 멈춰둔 과제가 여러 개일 경우, 가장 최근에 멈춘 과제부터 시작합니다.

과제 계획을 담은 이차원 문자열 배열 `plans`가 매개변수로 주어질 때, 과제를 끝낸 순서대로 이름을 배열에 담아 return 하는 solution 함수를 완성해주세요.



#### **제한사항**
- 3 ≤ `plans`의 길이 ≤ 1,000
    - `plans`의 원소는 [name, start, playtime]의 구조로 이루어져 있습니다.
        - name : 과제의 이름을 의미합니다.
            - 2 ≤ name의 길이 ≤ 10
            - name은 알파벳 소문자로만 이루어져 있습니다.
            - name이 중복되는 원소는 없습니다.
        - start : 과제의 시작 시각을 나타냅니다.
            - "hh:mm"의 형태로 "00:00" ~ "23:59" 사이의 시간값만 들어가 있습니다.
            - 모든 과제의 시작 시각은 달라서 겹칠 일이 없습니다.
            - 과제는 "00:00" ... "23:59" 순으로 시작하면 됩니다. 즉, 시와 분의 값이 작을수록 더 빨리 시작한 과제입니다.
        - playtime : 과제를 마치는데 걸리는 시간을 의미하며, 단위는 분입니다.
            - 1 ≤ playtime ≤ 100
            - playtime은 0으로 시작하지 않습니다.
        - 배열은 시간순으로 정렬되어 있지 않을 수 있습니다.
- 진행중이던 과제가 끝나는 시각과 새로운 과제를 시작해야하는 시각이 같은 경우 진행중이던 과제는 끝난 것으로 판단합니다.


### ***Solution***
``` java
import java.util.*;
class Solution {
    private static class Pair{
        String name;
        int playTime;
        
        public Pair(String name, int playTime){
            this.name = name;
            this.playTime = playTime;
        }
    }

    public static int transMin(String time){
        return Integer.parseInt(time.split(":")[0]) * 60 + Integer.parseInt(time.split(":")[1]);
    }

    public static List<String> solution(String[][] plans) {
        Arrays.sort(plans,(o1, o2) -> transMin(o1[1]) - transMin(o2[1]));
        int length = plans.length;

        List<String> answer = new ArrayList<String>();
        Stack<Pair> stop = new Stack<>();

        for(int i=0;i<length-1;i++){
            int current = transMin(plans[i][1]) + Integer.parseInt(plans[i][2]);
            int next = transMin(plans[i+1][1]);

            if(current <= next){
                answer.add(plans[i][0]);
                int remain = next - current;
                //남은 과제 해결
                while(!stop.isEmpty()){
                    if(remain <= 0){
                        break;
                    }
                    Pair p = stop.pop();
                    if(p.playTime > remain){
                        stop.add(new Pair(p.name,p.playTime-remain));
                        break;
                    }
                    else{
                        remain-=p.playTime;
                        answer.add(p.name);
                    }
                }
            }
            else{
                stop.push(new Pair(plans[i][0],current-next));
                continue;
            }
        }
        answer.add(plans[length-1][0]);
        while(!stop.isEmpty()){
            answer.add(stop.pop().name);
        }
        return answer;
    }
}
```
- Pair는 과목 명과 남은 시간을 가지고 있는 Class이다.
- 문제로 주어진 배열의 plans[i][1]번째는 시작 시간으로 시작 시간에 대해서 오름차순으로 정렬해준다.
    - 이때 transMin()은 문자열로 입력된 시간을 Int형 분으로 바꿔서 반환해주는 함수이다.
- stop은 과제를 하다가 멈춘 과제들을 Pair로 저장하는 Stack이다.
    - Stack을 사용한 이유는 멈춘 과제들 중 가장 최근에 멈춘 과제가 제일 먼저 다시 시작되어야하기 때문에 LIFO의 특성가진 Stack을 선택하였다.
- 시작시간에 대해서 오름차순으로 정렬된 plans를 차례로 탐색한다.
    - current는 진행 중인 과제의 시작 시간 + 진행 시간
    - next는 다음에 진행되어야 하는 과제의 시작 시간
    - current와 next를 비교한다.
        - current <= next인 경우는 현재 진행중인 과제를 모두 끝낼 수 있는 경우이다.
            - answer에 현재 진행중인 과제명을 넣는다.
            - stop한 과제가 있는지 확인하고 next-current 시간동안 가장 최근에 진행한 과제를 진행한다.
        - 그 외의경우는 현재 진행중인 과제를 멈춰야한다.
            - stop에 Pair를 생성하여 현재 진행중인 과제와 과제의 남은 시간을 계산하여 넣어준다.
### 출처
https://school.programmers.co.kr/learn/courses/30/lessons/176962