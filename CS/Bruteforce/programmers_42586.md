## **기능개발**


### ***problem***
프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

### **제한사항**
- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

### ***Solution***
``` java
import java.util.*;
class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        int[] arr = new int[101];
        int day = 1;
        
        for(int i = 0 ; i < progresses.length; i++){
            
            while(progresses[i] + (speeds[i]*day) < 100){
                day++;
            }
            arr[day]++;
        }
        int[] answer = Arrays.stream(arr).filter(i -> i!=0).toArray();
        return answer;
    }
}
```
### **문제 풀이** 
- 앞의 기능이 완수되지 않으면, 기능이 개발이 완료되어도 배포할 수 가 없다.
- progresses[i]의 개발이 완료되는 날짜를 day라고 하면, progresses[i+1]부터 day날짜에 기능 개발이 완료되었는지 확인한다.
- 그러면 day날에 개발이 완료되어 배포할 수 있는 기능의 갯수가 arr[day]에 저장된다.
- 이런식으로 확인을 하다가 다시 day날에 기능이 완료되지 않은 progresses[i]가 있다면, progreeses[i]의 기능이 완료되는 날을 구한다.
- 위의 행동을 모든 기능 개발이 완료될때까지 반복한다.

### **[출처]**
https://school.programmers.co.kr/learn/courses/30/lessons/42586?language=java